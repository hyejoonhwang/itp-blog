
# Everything I do goes through the same three machines

This week's assignment was traceroute analysis: follow the paths my traffic actually takes, figure out where the routers are, and work out whose networks I depend on without ever having agreed to anything.

I traced six sites I use constantly — Instagram, YouTube, NYU, Zoom, Chase, Amazon — all from my apartment on Verizon Fios.

## How traceroute works

Data doesn't go straight from my laptop to a website. It gets handed router to router, like a package moving through sorting facilities. Traceroute makes each stop announce itself, so you get a numbered list of every handoff and how long each one took.

Here's a clean one, to Google:

```
 1  cr1000a.mynetworksettings.com (192.168.1.1)      3.8 ms
 2  lo0-100.nycmny-vfttp-334.verizon-gni.net         11.7 ms
 3  100.41.64.200                                     9.3 ms
 4  * * *
 5  customer.alter.net (204.148.5.98)                12.5 ms
 6  bj-in-f113.1e100.net (142.250.31.113)            14.0 ms
```

Six hops. My router, two Verizon machines, one router that refused to identify itself, a handoff point, and Google. The whole trip took 14 milliseconds and probably never left the New York metro area.

(`1e100.net` is Google's domain for its servers — 1 followed by 100 zeros, a googol. `*` means a router passed my traffic along but declined to say who it was.)

## The first three hops are always identical

This is the finding that actually changed how I think about it.

I ran six traces to six completely different destinations. Every single one starts:

```
1  cr1000a.mynetworksettings.com (192.168.1.1)     ← my router
2  lo0-100.nycmny-vfttp-334.verizon-gni.net        ← Verizon, Manhattan
3  100.41.64.x                                      ← Verizon backbone
```

My bank, my school, my group chats, my shopping — all of it leaves through the same three machines. Reading the hostname tells you a lot: `nycmny` is New York, Manhattan, and `vfttp` is Verizon Fiber To The Premises. That's the box my fiber terminates at.

Out of 35 unique routers across all my traces, **12 belonged to Verizon** — and more importantly, Verizon appeared in **6 out of 6** traces. Every other network showed up in one or two. Verizon is the only company positioned to see all of it.

I knew abstractly that my ISP sees my traffic. Seeing the same three lines print at the top of six different files made it concrete in a way that reading about it didn't.

## Who lets you watch, and who doesn't

| Site | What I could see |
|---|---|
| Google / YouTube | all 6 hops, no resistance |
| Instagram | 11 hops, then blocked at the door |
| NYU | reaches NYU's own router, mostly silence |
| Zoom | 6 hops, then nothing |
| Chase | one lonely hop, then nothing |
| **Amazon** | **nothing at all past hop 3** |

Amazon was a total wall. After leaving Verizon's backbone I got seventeen consecutive rows of stars.

Instagram was so slow to block me that it broke my first attempt — it doesn't reply to probes, so my laptop patiently waited five seconds at each of 64 hops. Over five minutes of nothing for one site. I re-ran everything with limits:

```bash
traceroute -m 20 -w 1 -q 2 "$site"
```

Twenty hops max, one-second timeout, two probes per hop instead of three. Bounded at ~40 seconds even when a site ignores me completely.

Who refuses to be traced turns out to be as interesting as the routes themselves. The pattern is roughly: advertising companies don't mind being watched, and banks and retailers do.

## Where my traffic physically goes

Router names follow a convention once you notice it — usually airport codes.

**Instagram goes to Boston.** This was the most surprising route:

```
5   ewr-b13-link.ip.twelve99.net          ← EWR = Newark
7   bost-b5-link.ip.twelve99.net          ← Boston
8   meta-ic-365429.ip.twelve99-cust.net   ← handoff to Meta
9   po4001.asw04.bos5.tfbnw.net           ← Meta's Boston datacenter
```

`tfbnw.net` stands for **TheFaceBook NetWork**. And `twelve99.net` belongs to **Arelion**, a Swedish backbone carrier — AS1299.

So a photo I look at in Manhattan comes from Boston, and it gets to me via a Swedish company's American network. I had never heard of Arelion. They're a top-four carrier of my personal internet traffic.

**Chase goes to Denver.** Hop 10 was a Zayo router in Dallas, and the destination geolocated to Denver. The latency gave it away before the lookup did — **92ms and 46ms**, against roughly 10ms for everything else. That gap is real physical distance.

**NYU is basically next door** — 13–18ms, reaching `pa7500-ae13092.net.nyu.edu` via Zayo.

## Looking up who owns the routers

Names are suggestive but not proof. `ipinfo.io` gives you the real registration:

```bash
curl https://ipinfo.io/62.115.122.203/json
```

```json
{
  "hostname": "bost-b5-link.ip.twelve99.net",
  "city": "Boston",
  "region": "Massachusetts",
  "org": "AS1299 Arelion Sweden AB"
}
```

I pulled every IP out of my traces and looked them all up:

```bash
grep -ohE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' *.txt | sort -u > all-ips.txt

for ip in $(cat all-ips.txt); do
  info=$(curl -s "https://ipinfo.io/$ip/json")
  city=$(echo "$info" | grep '"city"' | cut -d'"' -f4)
  org=$(echo "$info" | grep '"org"' | cut -d'"' -f4)
  echo "$ip | $city | $org"
  sleep 0.3
done | tee ip-lookups.txt
```

35 addresses, geolocated and attributed in about thirty seconds.

## Autonomous systems: who actually carries my life

An **autonomous system** is one company's piece of the internet — the routers they own and control, under one number. The internet is roughly 75,000 of these agreeing to pass traffic to each other.

Counting them in my data:

```
 12  AS701    Verizon Business
  7  AS32934  Facebook, Inc.
  3  AS6461   Zayo Bandwidth
  3  AS1299   Arelion Sweden AB
  2  AS15169  Google LLC
  2  AS13335  Cloudflare, Inc.
  2  AS12     New York University
  1  AS3486   JPMorgan Chase & Co.
  1  AS209242 Cloudflare London, LLC
  1  AS14618  Amazon.com, Inc.
```

The raw count is misleading, though, so it's worth splitting them by role:

**Carriers — companies that move my data:** Verizon (every trace), Arelion, Zayo, Cloudflare.

**Destinations — companies I was going to:** Meta, Google, NYU, Chase, Amazon.

Meta's 7 looks enormous, but those are seven routers *inside one Meta building in Boston* — visible only because Meta doesn't hide its internal network. Amazon shows 1 because Amazon hides everything.

**The counts measure secrecy as much as size.**

A couple of things I didn't expect:

- **Chase runs its own autonomous system** (AS3486). Most companies rent network space from carriers. My bank owns a piece of the internet outright.
- **Zoom's address is registered to Cloudflare** (AS209242). I thought I was connecting to Zoom. I was connecting to Cloudflare, who connects to Zoom.

## How much do I trust this map?

Less than I did at the start of the assignment.

YouTube's final hop was named `dfw25s48-in-f14.1e100.net`. DFW is Dallas — 1,500 miles away. But the round trip took **10 milliseconds**, and light in fiber physically can't make that trip in under ~40ms. The name was wrong, or at least stale. When I looked the IP up, ipinfo put it in **New York City**, which matches the latency.

Zoom was worse. One IP, three different answers: the org field says *Cloudflare London*, the city field says *San Jose*, and I reached it through Newark.

So geolocation is a guess dressed up as a fact. Latency turned out to be the more honest signal — you can lie about a hostname, but you can't beat the speed of light. Where the two disagree, I'd believe the timing.

I mapped the routes with [Traceroute Mapper](https://stefansundin.github.io/traceroute-mapper/), which draws hops on a globe. The Instagram route running up the coast to Boston is a good picture of something I'd otherwise never have known.

## What's next

Everything here is from one network — my apartment, on Verizon. That's half the assignment.

The next step is running the identical six traces from **NYU's campus network**, saved to their own folder so the two don't mix:

```bash
mkdir nyu-campus && cd nyu-campus
for site in instagram.com youtube.com nyu.edu zoom.us chase.com amazon.com; do
  traceroute -m 20 -w 1 -q 2 "$site" > "$site.txt" 2>&1
done
```

I expect the contrast to be sharp. From campus, `nyu.edu` should take two or three hops instead of thirteen. Verizon should disappear entirely, replaced by NYU's own AS12 and whoever NYU buys transit from. Whether Instagram still routes through Boston — and still through a Swedish carrier — is the part I'm most curious about.

The interesting question isn't where the sites are. It's whether changing where *I* sit changes who gets to watch.

---

*Setup: macOS, `traceroute`, `ipinfo.io` for geolocation and ASN lookups, [Traceroute Mapper](https://stefansundin.github.io/traceroute-mapper/) for the maps. Six sites traced from a Verizon Fios connection in Manhattan.*
