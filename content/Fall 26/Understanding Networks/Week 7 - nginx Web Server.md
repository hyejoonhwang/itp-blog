

## The assignment

This week's assignment was to set up nginx as a web server on my network host, configure it for both HTTP and HTTPS, and make a basic home page with HTML, CSS and JavaScript.

## Readings

Before starting I went through the readings:

- [REST (Wikipedia)](https://en.wikipedia.org/wiki/REST)
- [Building Web Services the REST Way (xfront)](https://www.xfront.com/REST-Web-Services.html)
- [How To Install Nginx on Ubuntu 20.04 (DigitalOcean)](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
- [How To Secure Nginx with Let's Encrypt on Ubuntu 20.04 (DigitalOcean)](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04)

Honestly a lot of it went over my head at first, so I used an LLM (Claude) to help me break down the concepts in plain words and ask questions as I went.

## What I understood

A server is just a computer whose job is to wait for requests and answer them. My DigitalOcean droplet is one. But the droplet alone doesn't serve a website. You need a program on it that listens for browsers asking for something, finds the right file, and sends it back. That's what nginx does. It's like, the droplet is the kitchen and nginx is the waiter.

HTTP is the language the browser and nginx use to talk to each other. The browser sends a request like "GET /" (give me the home page), and nginx sends back a status code and the file. HTTPS is the same conversation but encrypted, so nobody in between can read or change it. To get HTTPS you need a certificate, which proves the site really belongs to that domain. Let's Encrypt gives these out for free, and certbot is the tool that asks for one and sets it up in nginx for you.

REST was the more abstract part. It's not something you install. It's a set of ideas about how the web should work, written up by Roy Fielding in 2000. Every thing gets its own address (URL), you use a few simple verbs like GET and POST, and the server doesn't remember you between requests, so each request has to carry everything it needs. The xfront article explains this with a made up parts company where every part has its own URL.


## What I already had

When I SSHed into my droplet to start, I checked which ports were already in use and found out nginx was already running on 80 and 443. I'd set it up for earlier projects. There are already three sites on it, each with its own HTTPS certificate:

- summerlaptop.me (crowd controls)
- shyfacetime.live
- whatsyournycstory.live

All three domains were already being used and I didn't want to break anything. So instead of installing nginx from scratch, I added a fourth site on a new subdomain: net.summerlaptop.me.

## Adding net.summerlaptop.me

**DNS.** First the new name had to point to my droplet. I didn't remember where I bought summerlaptop.me, so I used nslookup to check its nameservers, and they came back as Namecheap's. In Namecheap's Advanced DNS I added an A record with the host "net" pointing to my droplet's IP. A few minutes later nslookup on net.summerlaptop.me showed my droplet's IP, so that part worked.

**Home page.** I made a new folder for the site in /var/www and wrote an index.html in nano. It's a simple page with a heading, a line that shows the current time using JavaScript, a button, and a bit of CSS.

**Config.** Then I wrote a config file for the site. It's a short server block that tells nginx to listen on port 80, answer for net.summerlaptop.me, and serve files from my new folder. Nginx keeps all the configs in one folder and only reads the ones you switch on in a second folder, so I made a shortcut there to turn it on.

**First problem: 404.** When I opened net.summerlaptop.me in the browser I got a 404 Not Found from nginx. So DNS was working and the request was reaching my droplet, but nginx had no page for that name. I tried running certbot anyway. It got the certificate but couldn't install it, because it couldn't find any server block with my domain in it.

![[Screenshot 2026-11-03 at 2.23.57PM.png|379]]

Turns out it was the same problem both times. When I listed the config folders, my net config wasn't there at all. I had not actually saved the file in nano. Since nginx didn't know about net, it fell back to one of my other sites, and that config answers any name it doesn't recognize with a 404.

I wrote the config again, actually saved it this time, turned it on, tested the config and reloaded nginx. Refreshed the page and it showed up. HTTP done.


**HTTPS.** The certificate was already saved from before, so I just had certbot install it. Then I looked at my config file to see what certbot changed. It split my one server block into two:

- The first one now listens on port 443 with SSL and points to the certificate and private key from Let's Encrypt.
- The second one only listens on port 80 and sends back a 301 redirect to the https version.

So now if you go to http://net.summerlaptop.me it jumps to https on its own and shows the lock icon.

![[Screenshot 2026-11-03 at 2.27.57PM.png|429]]

**Renewal test.** Let's Encrypt certificates expire every 90 days and certbot renews them automatically on a timer. I ran a dry run to test this and it failed twice with "Connection reset by peer." From what I understand, the dry run talks to Let's Encrypt's separate test server, while getting my real certificate (which worked) used their main one. So my setup seems fine and it's more likely something between my droplet and their test server. Renewal isn't due until January, so I'm leaving it for now but want to keep an eye on it.

## First look at the access log

The last thing I checked was the access log. Every request nginx answers gets one line with the IP, the time, what was asked for, the status code, and the user agent (the browser or bot). Just in the last 10 lines there was:

- someone on an iPhone visiting summerlaptop.me, loading a bunch of images and then opening a websocket (status 101)
- a bot called Databot asking for robots.txt
- a scanner asking for /.env, which is where apps often keep passwords. It got a 404 from me.
- another request with what looks like a fake, really old Chrome user agent

