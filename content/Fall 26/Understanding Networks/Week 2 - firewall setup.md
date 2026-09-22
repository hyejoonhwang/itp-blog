Class instructions: [Setting up a firewall on an embedded Linux device](https://itp.nyu.edu/networks/setting-up-a-firewall-on-an-embedded-linux-device/).*


# Closing doors I didn't know were open

For this week's Understanding Networks assignment we had to set up a firewall. I did mine on a DigitalOcean droplet that's been running my projects for the better part of a year — which turned out to be a more interesting exercise than doing it on a fresh machine, because I had to figure out what was actually on it first.

## Finding out what I was running

Before I could decide what to block, I needed to know what was listening. This command lists every program on the machine waiting for incoming connections:

```bash
ss -tlnp
```

I expected maybe three things. I got eight:

| Port | Program    |
| ---- | ---------- |
| 22   | sshd       |
| 80   | nginx      |
| 443  | nginx      |
| 3000 | a Node app |
| 3001 | a Node app |
| 5001 | a Node app |
| 5002 | a Node app |
| 8080 | a Node app |

Every single one of those was reachable from anywhere on the internet. 

Two entries in the output looked different:

```
LISTEN  127.0.0.1:20241   cloudflared
LISTEN  127.0.0.53:53     systemd-resolve
```

`127.0.0.1` is localhost — "only this machine can reach me." Those two were already private no matter what I did. The `0.0.0.0` and `*` entries were the ones open to the world. I hadn't known to look at that column before.

## The thing that changed how I thought about it

I have three apps sitting behind domain names with HTTPS: `summerlaptop.me`, `shyfacetime.live`, and `whatsyournycstory.live`. I'd set those up with nginx and Certbot months ago and hadn't thought about them since.

Reading the nginx config again with firewall eyes, I finally understood what it was actually doing:

```nginx
server {
    server_name whatsyournycstory.live;
    location / {
        proxy_pass http://127.0.0.1:5001;
    }
    listen 443 ssl;
}
```

Nginx listens on 443. When a request comes in, it reads the `Host` header the browser sent, matches it against `server_name`, and forwards the request to `127.0.0.1:5001` — **an internal hop that never touches the network.**

Which means port 5001 doesn't need to be open to the internet *at all*. Nobody outside the machine ever needs to connect to it. Same for 3000 and 3001.

I'd been running those three apps with their ports exposed for months for no reason. They already had a proper front door. The side doors were just... still standing open.


## Default deny

The part that took me longest to actually get: **you don't close ports with a firewall. You open them.**

One thing i was confused about at first is "which ports should I block". But that's backwards. One line does the blocking:

```bash
ufw default deny incoming
```

That closes all 65,535 ports at once. Every command after it is an exception — a door you're choosing to leave open. Everything you don't mention stays shut, including ports you've never heard of and ports that don't have anything behind them yet.

That last part is the real value. Today I know what's running. But if I install Postgres next month, or run a dev server that binds to `0.0.0.0`, it's exposed the second it starts and I'd never notice. With default-deny, the new thing is already closed before it exists.

## The actual setup

`ufw` — Uncomplicated Firewall — is a friendly wrapper around `iptables`. Same firewall underneath, much less punctuation.

```bash
ufw default allow outgoing
ufw default deny incoming
ufw allow ssh
ufw allow http/tcp
ufw allow https/tcp
ufw allow 8080/tcp
ufw allow 5002/tcp
```

Notes on the choices:

- **`allow ssh` comes before enabling, always.** I'm connected to this machine over SSH. Turning on a deny-all firewall without that line would have cut the connection I was typing into, with no way back in except DigitalOcean's browser recovery console. The class instructions flag this and they're right to.
- **80 and 443 both stay open.** 443 serves all three sites. Port 80 is easy to think you can drop since everything's HTTPS — but it handles the redirect when someone types a bare domain, and Certbot renews certificates through it every 90 days. Close it and the sites silently expire in three months.
- **8080 and 5002** are two apps that don't have domains yet, so the raw `IP:port` address is the only way to reach them. They need real exceptions.
- **3000, 3001, 5001 get nothing.** Fronted by nginx. This is the whole point.
- **I skipped the `8081` from the class instructions** — that's for `p5.serialserver`, which I don't run. Opening a port to nothing is pure downside.

Then check the list *before* flipping the switch:

```bash
ufw show added
```

Confirmed `22/tcp` was there, then:

```bash
ufw enable
ufw status verbose
```

```
Status: active
Default: deny (incoming), allow (outgoing), disabled (routed)

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
8080/tcp                   ALLOW IN    Anywhere
5002/tcp                   ALLOW IN    Anywhere
```

Each rule also gets a `(v6)` twin for IPv6, automatically. Forgetting IPv6 is apparently a classic way to leave a door open you think you closed.

## Proving it worked

The satisfying part. My story app runs on port 5001, and it's also served at `whatsyournycstory.live`.

- `https://whatsyournycstory.live` → **loads normally**
- `http://[my-ip]:5001` → **hangs, then times out**

Same app. Same process. One path open, one path filtered. The app never even hears the second request — the firewall drops the packet before it gets there.

That's also the difference between a *closed* port and a *filtered* one. A closed port actively refuses, which tells a scanner "nothing here, but this host is alive." A filtered port says nothing at all. Silence gives away less.

## The part I didn't expect

UFW logs what it blocks:

```bash
grep 'UFW BLOCK' /var/log/ufw.log | tail -20
```

Within minutes there were entries — connection attempts from IP addresses I don't recognize, from all over, probing ports I never opened. Automated scanners, sweeping the address space looking for anything that answers.

They were doing that before today too. The only difference is that now something is writing it down and saying no.

---

*Setup: Ubuntu 24.04 on a DigitalOcean droplet, `ufw`, nginx as a reverse proxy, Let's Encrypt certs via Certbot. 




traceroute analysis