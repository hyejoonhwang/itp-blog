### https://itp.nyu.edu/classes/undnet/geography-of-the-internet/

### Network Topologies

 1. **distributed network** :  A message must take several hops to get across a distributed network (aka **random network** : links between nodes are placed randomly you need to know the distance measured in links)
 
 ![[Screenshot 2026-09-13 at 7.33.25 PM.png|297]]
2. **Centralized networks** : communication flows through the center

![[Screenshot 2026-09-13 at 7.34.55 PM.png|149]]

3. **Decentralized networks** : collections of  several hubs of nodes. Each hub has a central node, but the hubs are all linked together through other links.

![[Screenshot 2026-09-13 at 7.36.30 PM.png|143]]

**internet’s structure**
![[Screenshot 2026-09-13 at 7.40.35 PM.png]]
### Link Density
Each network provider connects to multiple other network providers, because the more possible paths there are for your messages, the more reliable their service will be.

 **complete network** : (n2-n)/2 links (n=nodes)
 In most physical networks, links are expensive and difficult to maintain, so you don’t see many complete networks in practice.
 ![[Screenshot 2026-09-13 at 7.42.21 PM.png|215]]
### Small Worlds Networks

![[Screenshot 2026-09-13 at 7.46.03 PM.png|207]]

## How are computer networks structured?

routers made by Cisco, phones made by Apple, laptops made by Dell, cables laid by different telecom companies, wifi chips made by Qualcomm, and so on.

For all of these completely different pieces of hardware and software to talk to each other and actually work as one network, everyone has to agree on how to do certain things the same way. That agreement is what a "standard" is.

The three organizations mentioned each play a role in creating those agreements:

[International Standards Organization (ISO)](https://www.iso.org/home.html)is a broad international body that sets standards across tons of industries, not just tech. The OSI model itself came out of ISO's work.

 [Institute of Electrical and Electronics Engineers (IEEE)](https://www.ieee.org/about/ieee-history.html) is more specifically focused on electrical and computing standards. This is the group behind things like Ethernet (802.3) and WiFi (802.11), and it's also who assigns the MAC address ranges to hardware manufacturers, which is mentioned later in that same section.

 [International Telecommunication Union (ITU)](https://www.itu.int/en/Pages/default.aspx)is a United Nations agency, so it operates at the level of governments rather than just companies. It coordinates things like international phone numbering, satellite spectrum use, and telecom policy between countries.


### The Open Systems (OSI) Interconnection Model

This model breaks a network into several layers, each of which describes a subset of the activities that happen on the network.  By breaking the various functions of a network up into discrete chunks, it enables different companies to concentrate on different layers, and know that their hardware and software will interoperate with other companies’ equipment.

**Layer 7, Application**: This is where you and the network actually meet. It's the layer that generates or uses the data based on what you're doing, browsing a website, sending an email. 

The question this layer answers: what are you actually doing with this data?

**Layer 6, Presentation**: This layer handles formatting the data so the application can understand it, and it's usually where encryption happens too. 

The question: how is the data formatted or encrypted?

**Layer 5, Session**: This manages the actual back and forth connection between two devices, keeping track of when a connection starts and when it ends. 

The question: how do you say hello and goodbye?

**Layer 4, Transport**: This decides how data gets sent, in what order packets should arrive, whether the receiver needs to confirm it got everything, and what happens if something needs to be resent. This is where TCP lives, if you've heard that term. 

The question: how are you sending the data, and should the receiver acknowledge it?

**Layer 3, Network**: This is where IP addresses come in. It handles getting data from one network to another, organizing it into packets that each carry an IP address so they know where they're headed. 

IP, Internet Protocol, is the set of rules that handles addressing and getting data from one network to another.

The question: what's your network address, and how do you reach other networks?

**Layer 2, Datalink**: This manages traffic on whatever physical medium you're using and handles addressing for devices themselves, using **Media Access Control (MAC)** addresses rather than  **internet protocol (IP)** addresses. Data here is organized into frames. 

The question: what's your physical device's address, regardless of what network it's on?

**Layer 1, Physical**: This is the actual medium the data travels through, fiber optic cable, copper wire, radio waves, along with things like voltage levels and connector shapes. 

The question: what physical medium are you using?


## Who Assigns Addresses?

**MAC addresses** get assigned at the hardware level. Every network interface, your laptop's WiFi chip, your phone's ethernet port, needs a MAC address, and manufacturers don't just make these up randomly. 

They license blocks of them from the IEEE. A MAC address is six bytes long, and it's split into two halves: the first three bytes are called the OUI (Organizationally Unique Identifier), which identifies the manufacturer itself, think of it like a company's assigned prefix. The last three bytes are unique to that specific device. So in theory, you could look at the first half of a MAC address and know which company made that network card.

**IP addresses** work through a completely different system, since they're not tied to hardware, they're tied to location on a network. The overall authority is *IANA (Internet Assigned Numbers Authority)*, which manages this through something called *Public Technical Identifiers*. *IANA* doesn't hand out addresses to you directly though. It delegates that job down to five Regional Internet Registries, one per major world region: ARIN for North America, RIPE NCC for Europe, the Middle East, and Central Asia, APNIC for Asia-Pacific, LACNIC for Latin America, and AFRINIC for Africa. Those regional registries then allocate address blocks to Internet Service Providers, who in turn assign individual addresses to their customers, which is why the page mentioned earlier that address distribution is organized geographically.


**How this actually plays out when a device joins a network**
Your device announces its MAC address and asks for an IP address. The router then uses something called *ARP, Address Resolution Protocol*, to link that MAC address to an IP address, and it decides whether to grant or deny that device access based on the network's own policies, like a password-protected WiFi network refusing devices that don't have the right credentials.

**public vs private**
The system splits addresses into two categories: public addresses, which must be unique across the entire internet, and private addresses, which only need to be unique inside one local network

Back when IPv4 addressing was first designed, addresses were split into classes based on network size.
**10.0.0.0** comes from the Class A range. This block alone gives you about 16.7 million possible addresses. It's meant for huge networks, think a large corporation or university with tons of internal devices across many buildings.

**172.16.0.0** comes from the Class B range. This gives you about 1 million addresses, meant for mid sized organizations, a mid size company's office network, for example.

**192.168.0.0** comes from the Class C range. This gives you about 65,000 addresses, meant for small networks. This is why it's the one you recognize, it's the default for pretty much every home router, since a household only ever has a handful of devices.

When your laptop actually wants to reach something on the internet, say load a website, your router swaps out your private address for its own single public address before sending the request out. This swap is called ***NAT, Network Address Translation***. To the rest of the internet, it looks like the request came from your router's public IP, not your laptop's private one. When the response comes back, the router remembers which private device asked for it and routes it back to the correct device inside your home.


So put together, MAC address is your device's fixed hardware identity, permanently baked in by the manufacturer, while IP address is more like an address you're issued depending on which network you're currently plugged into. That's part of why comparing them earlier, at the Datalink versus Network layer, made sense: one identifies the device itself, the other identifies where that device currently sits on the internet.


In order to connect to a network, a private network has to have a **public gateway**. Its router performs this function, and therefore has two network addresses, one public and one private.

![[Screenshot 2026-09-13 at 9.18.46 PM.png]]


One large network may combine several smaller ones. A large network can combine a combination of public and private networks. Ultimately every device with a private address will be “represented” to the rest of the internet by the first router above it with a public address.
![[Screenshot 2026-09-13 at 9.20.43 PM.png]]

**Autonomous Systems** are networks of networks.
![[Screenshot 2026-09-13 at 9.21.43 PM.png]]


# https://itp.nyu.edu/groups/flyby/unix-intro-1/

### What is Unix?
Unix is a family of operating systems, stretching back to its origins at AT&T Bell Laboratories in 1969.
Which means, it's a lineage of that foundational software layer, and the point is that Unix's specific design choices for that layer (how it organizes files, how it lets programs talk to each other, how it handles permissions) turned out to be so good that they got copied into nearly every OS running today
### What is Operating System?
An operating system is the **software** that runs first when a computer turns on, and its job is to **manage** the computer's **hardware** (the CPU, memory, storage, screen, keyboard) and act as the **middleman** between that hardware and every other program you run. Windows, macOS, Linux, Android, and iOS are all operating systems.

### The Shell
This is the text interface where you type commands and get text back, instead of clicking icons. A session looks like: you see a prompt, you type a command, you see output, repeat, then you type `exit` when you're done. The core loop never really changes no matter how complex the commands get.