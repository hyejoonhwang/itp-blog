
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

|   |   |   |   |
|---|---|---|---|
|Type|Envelopes|% of all|What it is|
|QUIC|148,659|66.9%|Newer websites and video (HTTP/3), the postcard style|
|Data|20,169|9.1%|UDP that Wireshark couldn't label, likely apps like video calls|
|TLS|15,511|7.0%|Regular sealed web traffic (HTTPS)|
|DNS|1,850|0.8%|Phone book lookups|
|ARP|885|0.4%|The router shouting in the hallway|
|mDNS|635|0.3%|Apple devices announcing themselves nearby (AirPlay, AirDrop)|
|ICMP (ping)|335|0.2%|Pings, including the IPv6 kind|
|HTTP|12|about 0%|Unsealed web pages. Half were just certificate checks|
|SSH|0|0%|Not in the list at all|

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

|   |   |   |   |
|---|---|---|---|
|Rank|Address|Envelopes|Who it is|
|1|74.125.174.70|58,588 (26%)|Google, likely YouTube video|
|2|160.79.104.10|20,146|Anthropic (Claude, which I was using to learn all this)|
|3|57.144.180.192|19,458|Meta (Instagram, Facebook, WhatsApp)|
|4|157.240.241.63|17,301|Meta|
|5|31.13.71.52|15,519|Meta|
|6|104.18.41.41|8,537|Cloudflare, which runs sites for lots of companies|
|8|17.248.175.24|6,775|Apple (iCloud)|
|14|172.16.20.1|2,235|My WiFi router itself|

Fun finding: all the Meta rows together add up to about 57,000 envelopes, almost tied with Google for first.

### My traffic over time

Statistics › I/O Graphs.

_Screenshot: I/O Graph_
![[Pasted image 20260929124745.png]]

The line shows envelopes per second across the whole 25 minutes. For the first 15 minutes it's mostly quiet, with a few small spikes (loading a page). Around the 16 minute mark it turns into a steady rhythm of regular spikes, which looks like video streaming: video arrives in chunks every few seconds. The biggest bursts are at the very end, up to about 14,000 envelopes in a single second. The red line (TCP errors) stays flat near zero, so the connection was healthy.

## What I learned

My Mac is mostly a visitor, not a host. Nobody from outside reached it, because the router guards the entrance. Almost everything I sent and received was sealed, and most of it used the newer UDP based HTTP/3, not the classic TCP web. Even without opening a single envelope, the outsides told me who I talk to, how much, and when.