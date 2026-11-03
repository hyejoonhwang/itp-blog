
# Packet Analysis

This week's assignment: record the network traffic on my computer, then figure out what's in it. How much is web traffic? How much is SSH or ping? How many strangers on the internet tried to get in, and who tried the most?

## The analogy that made it click

Claude helped me understand the concept with an apartment building analogy, where my Mac is the building.

| Real thing         | Building version                                                   |
| ------------------ | ------------------------------------------------------------------ |
| My Mac             | An apartment building                                              |
| Packet             | An envelope. All data travels in small envelopes, going in and out |
| IP address         | The building's address                                             |
| Port               | An apartment door number                                           |
| Program            | Whoever lives behind that door                                     |
| Interface          | An entrance to the building. WiFi is one (called en0 on a Mac)     |
| tcpdump            | A security camera at the entrance                                  |
| .pcap file         | The saved footage                                                  |
| Wireshark          | The app for playing the footage back                               |
| Encryption (HTTPS) | Envelopes you can see but can't open                               |

So packet analysis is basically this: point a camera at the entrance, record every envelope that passes, then play it back and count. Most envelopes are sealed, but the outside still tells you who sent it, where it's going, and what kind of mail it is.

## Things I got confused about (and the answers)

**Why is WiFi called an "interface"?** In tech, an interface is anywhere two things meet. A user interface is where a person meets software. A network interface is where my computer meets the network.

**Is an IP address where my Mac physically is?** Not really. It's more like a mailing address on the network. It changes when I switch WiFi, and it only loosely hints at location, which is why the IP lookup sites in class showed the wrong cities. At home my Mac has an inside address, and everyone in the household shares one outside address through the router.

**Port vs program vs server?** The IP address is WHERE the building is, the port is WHICH door, and a server is WHAT the building is for. A server is just a computer whose job is answering visitors. It's a role, not a special machine.

**Does every Mac have its own server somewhere?** No. Companies like YouTube run thousands of servers, and my Mac just visits them. If I rent one (like on DigitalOcean), it's basically a virtual computer in a data center, usually running Linux with no screen, controlled by typing commands over SSH.

## Types of mail

| Type        | What it is                                                       | Door    |
| ----------- | ---------------------------------------------------------------- | ------- |
| HTTP        | Web pages sent openly. Rare now                                  | 80      |
| HTTPS       | Web pages in a sealed envelope                                   | 443     |
| DNS         | The internet's phone book: "what's the address for youtube.com?" | 53      |
| SSH         | Remote control of another computer by typing commands            | 22      |
| Ping (ICMP) | Knocking to ask "are you there?"                                 | No door |

Ping also tells you how long the reply takes, so it's the quickest way to check if something is reachable and how fast.

All of this travels one of two ways.

**TCP is like a phone call.** Both sides say hello, confirm every piece arrived, and say goodbye. Web pages and SSH use it because they need every single piece, in order. A missing piece means a broken image or a wrong command. I assumed this was about security, but it's about completeness. Security comes from encryption, which works either way.

**UDP is like tossing postcards.** Fast, no hello, no confirmation. Video calls use it because a live call needs speed more than perfection: if a piece gets lost, it's better to skip it and see a tiny glitch than freeze while waiting. DNS uses it because the question and answer are so short that a whole hello and goodbye would take longer than the question itself. If the answer gets lost, my Mac just asks again.

Fun twist: newer websites (HTTP/3) actually run on UDP with their own confirming built on top.

## How a TCP call starts

In tcpdump, every line has a little code in brackets:

| Code | Meaning |
| --- | --- |
| \[S\] | "Hi, can we talk?" |
| \[S.\] | "Sure, I'm here!" |
| \[.\] | "Got it" |
| \[P.\] | Actual data being sent |
| \[F.\] | "Bye!" |
| \[R.\] | "Nobody's home" |

The pattern that matters most for this assignment: a stranger sends \[S\], my Mac answers \[R.\]. That's someone knocking on a door where nobody lives.

## What I've done so far

**Step 1: Find my entrances.**

```
tcpdump -D
```

This just lists the entrances my building has, and records nothing. WiFi is en0.

*Screenshot: my tcpdump -D output*
![[Pasted image 20260929122349.png]]

**Why so many?** My Mac doesn't have just one entrance. It has the real WiFi chip, maybe an Ethernet port, plus a bunch of software only entrances the system creates for itself. Most of them aren't carrying my internet traffic:

| Name | What it is |
| --- | --- |
| en0 | My WiFi chip. The real one: everything I do online goes through here |
| ap1, awdl0, llw0 | Apple's private wireless channels. When I AirDrop a photo to a friend's phone, my Mac talks to it directly without the router, and that happens here |
| utun0 to utun5 | Tunnels. VPN or iCloud Private Relay traffic gets wrapped up here before it leaves |
| lo0 | Loopback, my Mac talking to itself. If I run a site on my laptop at localhost:3000, that data never touches WiFi |
| en1 to en6, anpi0 to anpi3, bridge0 | Real hardware entrances with nothing plugged in, like an Ethernet port with no cable |
| gif0, stf0 | Leftover legacy stuff. Ignore |

**How I knew en0 was the right one:** look at the labels in brackets. Only two said Associated, meaning "currently joined to a WiFi network": en0 and awdl0. And awdl0 is the AirDrop one, so en0 it is. Everything marked Disconnected is a dead end, and recording there shows nothing.

**Step 2: Check which doors are open.**

```
sudo lsof -iTCP -sTCP:LISTEN -n -P
```

What each part means:

| Part | Meaning |
| --- | --- |
| `sudo` | Run as admin, which is why it asked for my password |
| `lsof` | "List open files." On a Mac, network connections count as files, so this can list them |
| `-iTCP` | Only show connections that use TCP |
| `-sTCP:LISTEN` | Only the ones in listening mode, meaning a program is waiting behind the door for visitors |
| `-n` | Show addresses as numbers, not names |
| `-P` | Show door numbers as numbers (5000) instead of names |

All together: "show me every door on my Mac where a program is waiting for visitors."

*Screenshot: my lsof output*
![[Pasted image 20260929122405.png]]

This lists every door where a program is waiting for visitors. I got a long list, but the key was the address column. Anything with 127.0.0.1 is an inside door, reachable only from my own Mac (Figma, GitHub, and so on). Only three programs had doors facing the network, all Apple features:

| Program | Doors | What it is |
| --- | --- | --- |
| ControlCenter | 5000, 7000 | AirPlay Receiver |
| rapportd | 52060, 52061, 63574 | Continuity, how my Mac talks to my iPhone |
| symptomsd | 51467 | Apple's network diagnostics |

So any knock on another door, like 22 or 80, is a knock on a closed door.

The difference between these two commands confused me at first: the first shows the entrances, and the second shows which apartments have someone home. Neither one records anything.

**Step 3: Start recording.**

```
sudo tcpdump -i en0 -nn -w capture.pcap
```

| Part              | Meaning                                          |
| ----------------- | ------------------------------------------------ |
| `sudo tcpdump`    | Turn on the camera, as admin                     |
| `-i en0`          | Point it at the WiFi entrance                    |
| `-nn`             | Show numbers instead of names, which is faster   |
| `-w capture.pcap` | Save the footage into a file called capture.pcap |

The screen looks like nothing's happening, but it's recording quietly. The plan is to let it run for about an hour while I use my computer normally, then stop it with Ctrl + C.

![[Pasted image 20260929122424.png]]

**Step 4: Stop and check the result.**

I let it run for about 20 minutes, then pressed Ctrl + C. Here's what Terminal said:

```
tcpdump: listening on en0, link-type EN10MB (Ethernet), snapshot length 524288 bytes
^C222119 packets captured
222130 packets received by filter
0 packets dropped by kernel
```

|                                   |                                                                             |
| --------------------------------- | --------------------------------------------------------------------------- |
| Line                              | Meaning                                                                     |
| listening on en0                  | The camera was pointed at my WiFi entrance                                  |
| link-type EN10MB (Ethernet)       | The recording format. Macs label WiFi this way, so it's normal              |
| snapshot length 524288 bytes      | The max it saves per envelope, big enough to keep every envelope whole      |
| ^C                                | Me pressing Ctrl + C to stop                                                |
| 222119 packets captured           | **222,119 envelopes saved** in about 20 minutes                             |
| 222130 packets received by filter | 11 more arrived right as I stopped and didn't make it into the file. Normal |
| 0 packets dropped by kernel       | The camera didn't miss anything                                             |
**Step 5: Open the footage in Wireshark.**

```
open capture.pcap
```

_Screenshot: capture.pcap open in Wireshark_![[Screenshot 2026-09-29 at 12.30.24 PM.png]]
![[Screen Recording 2026-09-29 at 12.30.50 PM.mov]]
The screen has three parts. The top pane is one row per envelope, in the order they passed the camera. The middle left shows the layers of whichever row I click, envelope inside envelope. The middle right is the raw contents in computer code, which I can ignore.

My Mac's address is 172.16.21.49. It shows up in every row as either Source (sent from me) or Destination (arriving to me).

Some of my first rows, translated :

|         |                                                                                                              |
| ------- | ------------------------------------------------------------------------------------------------------------ |
| Row     | What's happening                                                                                             |
| 1       | UDP from door 443 to me. A newer website (HTTP/3) sending data the postcard way                              |
| 2, 5, 6 | TLS Application Data: sealed envelopes, encrypted web traffic I can't open                                   |
| 3, 8, 9 | [ACK]: my Mac saying "got it"                                                                                |
| 4       | ARP: the router shouting in the hallway, "who lives at 172.16.20.219?" A broadcast to everyone, not about me |
| 7, 10   | [FIN, ACK]: both sides saying "bye"                                                                          |
| 14      | Keep Alive: "you still there?" to hold a connection open                                                     |



## The answers

### Questions 1 and 2: what types of traffic?

Statistics › Protocol Hierarchy.

_Screenshot: Protocol Hierarchy_
![[Pasted image 20260929124714.png]]

The tree works like folders, so the percentages overlap (TCP 22.3% already includes TLS 7.0%). The innermost rows are the real answers:

|             |           |          |                                                                 |
| ----------- | --------- | -------- | --------------------------------------------------------------- |
| Type        | Envelopes | % of all | What it is                                                      |
| QUIC        | 148,659   | 66.9%    | Newer websites and video (HTTP/3), the postcard style           |
| Data        | 20,169    | 9.1%     | UDP that Wireshark couldn't label, likely apps like video calls |
| TLS         | 15,511    | 7.0%     | Regular sealed web traffic (HTTPS)                              |
| DNS         | 1,850     | 0.8%     | Phone book lookups                                              |
| ARP         | 885       | 0.4%     | The router shouting in the hallway                              |
| mDNS        | 635       | 0.3%     | Apple devices announcing themselves nearby (AirPlay, AirDrop)   |
| ICMP (ping) | 335       | 0.2%     | Pings, including the IPv6 kind                                  |
| HTTP        | 12        | about 0% | Unsealed web pages. Half were just certificate checks           |
| SSH         | 0         | 0%       | Not in the list at all                                          |

**HTTP was basically zero:** 12 out of 222,119 envelopes. Almost every site I visited was sealed. **SSH was zero** too, since I never logged into another computer, and **ICMP was tiny** at 0.2%. The surprise was QUIC: 67% of envelopes and 79% of all data. Most of my traffic used the newer HTTP/3 postcard style, probably from streaming video.

### Questions 3 and 4: strangers knocking on closed doors

This filter shows every time someone outside said "Hi, can we talk?" to my Mac first:

```
ip.dst==172.16.21.49 && tcp.flags.syn==1 && tcp.flags.ack==0
```

_Screenshot: filter result showing Displayed: 0_
![[Pasted image 20260929124727.png]]

**Result: zero knocks, and zero strangers.** My Mac sits behind the WiFi router, which works like a building doorman. My Mac only has an inside address, so strangers on the internet can't even find my building, and the router turns away anyone my Mac didn't invite. Every conversation in my capture was started by my Mac.

### Question 5: who was most active?

Statistics › Conversations › IPv4 tab, sorted by Packets.

_Screenshot: Conversations_
![[Pasted image 20260929124735.png]]

My Mac talked with **267 different addresses** in 25 minutes. Since nobody contacted me first, these aren't strangers. They're all servers my Mac went out and visited. I looked up who owns the busiest ones:

|      |                |              |                                                         |
| ---- | -------------- | ------------ | ------------------------------------------------------- |
| Rank | Address        | Envelopes    | Who it is                                               |
| 1    | 74.125.174.70  | 58,588 (26%) | Google, likely YouTube video                            |
| 2    | 160.79.104.10  | 20,146       | Anthropic (Claude, which I was using to learn all this) |
| 3    | 57.144.180.192 | 19,458       | Meta (Instagram, Facebook, WhatsApp)                    |
| 4    | 157.240.241.63 | 17,301       | Meta                                                    |
| 5    | 31.13.71.52    | 15,519       | Meta                                                    |
| 6    | 104.18.41.41   | 8,537        | Cloudflare, which runs sites for lots of companies      |
| 8    | 17.248.175.24  | 6,775        | Apple (iCloud)                                          |
| 14   | 172.16.20.1    | 2,235        | My WiFi router itself                                   |

Fun finding: all the Meta rows together add up to about 57,000 envelopes, almost tied with Google for first.

### My traffic over time

Statistics › I/O Graphs.

_Screenshot: I/O Graph_
![[Pasted image 20260929124745.png]]

The line shows envelopes per second across the whole 25 minutes. For the first 15 minutes it's mostly quiet, with a few small spikes (loading a page). Around the 16 minute mark it turns into a steady rhythm of regular spikes, which looks like video streaming: video arrives in chunks every few seconds. The biggest bursts are at the very end, up to about 14,000 envelopes in a single second. The red line (TCP errors) stays flat near zero, so the connection was healthy.

## What I learned

My Mac is mostly a visitor, not a host. Nobody from outside reached it, because the router guards the entrance. Almost everything I sent and received was sealed, and most of it used the newer UDP based HTTP/3, not the classic TCP web. Even without opening a single envelope, the outsides told me who I talk to, how much, and when.

i tried packet analysis with my droplet



![[Screenshot 2026-09-29 at 3.59.41 PM.png]]

### What you found

**Entrances (`tcpdump -D`):** Linux names them differently than a Mac. There's no `en0` here, which is why your last command failed.

|Name|What it is|
|---|---|
|eth0|The public entrance. The whole internet reaches your droplet through here. **This is the one to record**|
|eth1|A private entrance inside DigitalOcean's own network, only for talking to other droplets|
|lo|Loopback, the server talking to itself|
|the rest|System stuff. Ignore|

# Packet Analysis, Part 2: The Server

In Part 1, I recorded my laptop and zero strangers knocked. My WiFi router works like a building doorman, so nobody from outside could even find my Mac. That made questions 3 to 5 pretty boring.

So I tried again on my DigitalOcean droplet. A droplet is basically a rented computer in a data center, running Linux with no screen. It has a public address (165.227.98.201) and no doorman, so anyone on the internet can walk right up to it.

## Setting up

I logged into the droplet over SSH and ran the same first two commands as last time.

### Finding the entrance

```
tcpdump -D
```

Linux names its entrances differently than a Mac, so there's no `en0` here. That's why my first recording attempt failed with "No such device exists."

| Name     | What it is                                                                                         |
| -------- | -------------------------------------------------------------------------------------------------- |
| eth0     | The public entrance. The whole internet reaches my droplet through here. This is the one to record |
| eth1     | A private entrance inside DigitalOcean's own network, only for talking to other droplets           |
| lo       | Loopback, the server talking to itself                                                             |
| the rest | System stuff I can ignore                                                                          |
![[Screenshot 2026-09-29 at 4.15.25 PM.png]]
### Checking which doors are open

```
sudo lsof -iTCP -sTCP:LISTEN -n -P
```

A lot more open doors than my laptop. Doors marked with `*` face the whole internet:

|Door|Program|What it is|
|---|---|---|
|22|sshd|SSH, how I log in|
|80, 443|nginx|My web server|
|3000, 3001, 5001, 5002, 8080|node|My own apps, each on its own door|
|20241, 20242|cloudflared|Inside only. A Cloudflare tunnel|
|53|systemd resolve|Inside only. The server's own phone book|

So any knock on a door that isn't 22, 80, 443, 3000, 3001, 5001, 5002, or 8080 is a knock on a closed door.

![[Screenshot 2026-09-29 at 4.15.51 PM.png]]


### How to record on the droplet

Four changes from the laptop version: use `eth0`, skip your own SSH session so you don't record yourself, keep it running after you close Terminal, and give the file a different name so it doesn't overwrite your laptop capture.

**Step 1: Find your own address** (so you can exclude it). On the droplet, type:

```
echo $SSH_CLIENT
```

The first number is your address right now. Copy it.

**Step 2: Open a session that survives closing Terminal:**

```
tmux new -s capture
```

**Step 3: Start recording.** Replace YOUR_IP with the number from step 1:

```
tcpdump -i eth0 -nn -w server.pcap 'not (host YOUR_IP and port 22)'
```

You're logged in as root, so `sudo` isn't needed. The last part means "record everything except my own SSH conversation," so the bots knocking on door 22 still get recorded.

**Step 4: Leave it running.** Press **Ctrl + B**, let go, then press **D**. That detaches, so the recording keeps going even if you close Terminal or your laptop. Let it run for at least an hour. Overnight is even better.

![[Screenshot 2026-09-29 at 4.32.12 PM.png]]


**Step 5: Stop it later.** SSH back in, then:

```
tmux attach -t capture
```

Press **Ctrl + C** to stop, then type `exit`.

**Step 6: Copy the file to your Mac.** Open a _new_ Terminal window on your Mac (not inside the droplet) and run:

```
scp root@165.227.98.201:~/server.pcap ~/server.pcap
```

Then `open ~/server.pcap` and use the same Wireshark steps as before, just with `165.227.98.201` in the filters instead of `172.16.21.49`.

## Recording

```
tcpdump -i eth0 -nn -w server.pcap 'not (host MY_IP and port 22)'
```

The new part at the end means "record everything except my own SSH conversation." Without it, I'd mostly be recording myself typing. Bots knocking on door 22 still get recorded.

To find MY_IP, I ran `echo $SSH_CLIENT`. It prints three numbers, but only the first one is my address. I pasted all three the first time and got "syntax error." Lesson learned.

I let it run for about 17 minutes, then stopped with Ctrl + C:

```
9825 packets captured
9827 packets received by filter
0 packets dropped by kernel
```

**9,825 envelopes, about 2.2 MB.** Way fewer than my laptop's 222,119, because nobody's streaming video on this server. But almost all of it came from strangers.

### Getting the file to my Mac

```
scp root@165.227.98.201:~/server.pcap ~/server.pcap
```

This has to run from my Mac, not from inside the droplet. The first time I ran it while still logged into the droplet, so it just copied the file onto itself. The prompt tells you where you are: `root@ubuntu...` means the droplet, `summer@Mac` means my laptop.

Then `open ~/server.pcap` to open it in Wireshark.


## The answers

### Questions 1 and 2: what types of traffic?

Statistics › Protocol Hierarchy.
![[Screenshot 2026-09-29 at 4.45.13 PM.png]]

|Type|Envelopes|% of all|What it means|
|---|---|---|---|
|SSH|2,594|26.4%|I excluded my own session, so this is all strangers trying to log in|
|Data|2,103|21.4%|Unlabeled UDP|
|QUIC|1,011|10.3%|Likely my Cloudflare tunnel talking to Cloudflare|
|TLS|331|3.4%|Sealed web visits to my nginx|
|HTTP|275|2.8%|Unsealed web requests. 137 have readable text inside|
|ICMP|217|2.2%|Pings, plus "nobody's home" replies|
|DNS|28|0.3%|Phone book lookups|
|SIP, OpenVPN, NetBIOS, NAT Port Mapping|6 total|about 0%|Scanners checking for services I don't run|

**HTTP was 2.8%**, compared to basically 0% on my laptop. My droplet runs a web server, so people (and bots) actually visit it.

**SSH was the headline:** 26% of all envelopes and 31.5% of all data, and none of it was me. **ICMP was 2.2%.**

The tiny rows at the bottom are sneaky. SIP is for internet phone calls, OpenVPN is a VPN, NetBIOS is old Windows file sharing. I don't run any of those. That's scanners knocking to see if I happen to.

![[Screenshot 2026-09-29 at 4.46.23 PM.png]]



![[Screenshot 2026-09-29 at 4.47.38 PM.png]]![[Screenshot 2026-09-29 at 4.49.03 PM.png]]![[Screenshot 2026-09-29 at 4.50.04 PM.png]]


### Question 3: strangers knocking on closed doors

First, every uninvited knock. This filter shows every time someone outside said "Hi, can we talk?" to my server first:

```
ip.dst==165.227.98.201 && tcp.flags.syn==1 && tcp.flags.ack==0
```

Show Image

**381 uninvited knocks in about 17 minutes**, roughly one every 3 seconds. Most of them were on door 22. But door 22 is actually open on my droplet, so those aren't knocks on closed doors. They're strangers walking up to my real SSH door and trying to get in.

To count only the closed doors, I hid knocks on doors I actually have open:

```
ip.dst==165.227.98.201 && tcp.flags.syn==1 && tcp.flags.ack==0 && !(tcp.dstport in {22, 80, 443, 3000, 3001, 5001, 5002, 8080})
```

(My first try had no commas between the numbers and the filter bar turned red. This Wireshark version needs the commas.)

Show Image

**154 knocks on closed doors**, roughly one every 7 seconds.

So the full picture of the 381 knocks:

|Where they knocked|Knocks|
|---|---|
|Doors I have open (mostly SSH on 22)|227|
|Doors where nobody lives|154|

A few things stood out:

- **Every closed door number was different:** 6807, 8477, 30083, 31739, 9000, 14400, and on and on. Nobody was looking for something specific. It's like someone walking down a hallway jiggling every doorknob.
- **Lots of knocks came from the same neighborhood:** 85.217.140.4, .10, .15, .47, .49, .50, .51, and more. That's one organization using many addresses to scan faster.
- **One scanner, 204.76.203.4, kept coming back** from its door 1001, each time trying a different random door on my server.
- **Some rows showed up red as "TCP Retransmission."** That stranger knocked, got no answer at all, and tried again three more times. Something, probably DigitalOcean's firewall, silently ignored them instead of saying "nobody's home."

### Question 4: how many unique strangers?

With the "all knocks" filter on: Statistics › Endpoints › IPv4 tab, "Limit to display filter" ticked.

Show Image

**140 unique strangers** in about 17 minutes. (The tab says 141, but the top row is my own server receiving the knocks.)

### Question 5: who was most active?

Same Endpoints window, sorted by Packets. The useful columns:

- **Packets:** how many times they knocked
- **Total Packets:** everything they sent, including what happened after the door opened
- **Percent Filtered:** what share of their traffic was just knocking. 100% means they knocked and never went further

My strangers split into two very different groups:

|Group|Address|Knocks|Total envelopes|What they're doing|
|---|---|---|---|---|
|Password guessers|109.160.32.169|143|3,692|Knocking on SSH over and over and trying passwords each time|
||165.232.148.95|30|770|Same thing, from another DigitalOcean server|
||92.118.39.77|12|349|Same thing, smaller|
|Scanners|204.76.203.4, 62.210.142.173, and most of the list|1 to 8 each|Same as knocks|Jiggle a few doorknobs, then leave|

**One single address, 109.160.32.169, sent about 38% of everything in my capture.** A few strangers are very busy and very persistent. Then there's a long tail of 100+ strangers who each knock just a handful of times.

Fun detail: 165.232.148.95 is also a DigitalOcean address. So one of my attackers is probably another droplet that got hacked and is now attacking others.

## Laptop vs server

||Laptop|Server|
|---|---|---|
|Recording time|About 25 min|About 17 min|
|Envelopes|222,119|9,825|
|HTTP|12 (about 0%)|275 (2.8%)|
|SSH|0|2,594 (26.4%)|
|Uninvited knocks|0|381|
|Knocks on closed doors|0|154|
|Unique strangers|0|140|
|Who started the conversations|My Mac, every time|Mostly strangers|

## What I learned

The difference between the two is the doorman. My laptop is a visitor that goes out and talks to Google, Meta, and Claude, and the router keeps everyone else out. My server is a host with a public address, and within seconds of recording, strangers were at the door. Some were just mapping the internet. Others were actively trying to break in.

Even without opening a single sealed envelope, the outsides told me who was knocking, which doors they tried, and how hard they were trying.

Next step for me: lock down that SSH door.

## Filter cheat sheet

For future me:

| What I want                        | Filter                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------- |
| Everything to or from one address  | `ip.addr==x.x.x.x`                                                               |
| Every uninvited knock on my server | `ip.dst==MY_SERVER && tcp.flags.syn==1 && tcp.flags.ack==0`                      |
| Knocks on closed doors only        | add `&& !(tcp.dstport in {22, 80, 443})` with my open doors, separated by commas |
| Just one type of traffic           | `http`, `tls`, `quic`, `dns`, `ssh`, `icmp`                                      |