---
layout: post
title:  "Cybersecurity: Who is roaming the halls?"
date:   2021-01-08 07:00:00 EDT
categories: cybersecurity networks
---
This brief article describes how to use tcpdump to collect data for analyzing communication patterns in a computer network. Analyzing communication patterns will give you insight into how the network is being used.

---

**There is always a way into your organization.**

- There are always ways to break into an organization's IT infrastructure. (cite solar winds and jetbrains)

A common myth is that an organization can somehow protect itself from cyberattacks. It is true that you can set up defenses, and layered defenses help prevent many attacks. Unfortunately, it is also true that no matter how strong your defenses are, eventually someone will get through.

The supply chain attack is one example of how an attacker can bypass all of your security.

**Production networks are noisy.**

- A production network is noisy. Most often the network use differs from the network design.


**You can do analysis with a professional tool or with open source.**

- A professional tool can help you understand the network, but you also can do analysis for free with tcpdump.

**Capturing Network Traffic with Tcpdump**

*What follows is not meant to be an elaborate tutorial. Instead, I want to whet your appetite so you feel motivated to learn more about network analysis on your own.*

Assuming you want to create your own analysis solution and learn more while doing it, you can use freely available network capture tools. Some command line tools you can use include `tcpdump`, `windump`, and `tshark`. For this article I will use `tcpdump`, which is available on Linux, Mac OS, and other Unix-variant systems. The Tcpdump project is open source[[1]](#1), so you can even dive into the source code if you want.

When we capture network traffic, we want to see the Ethernet addresses, the IP addresses, and the ports (if applicable). To insure the information we need is displayed in a machine-processable format, we will use the `-e` and `-n` arguments. Also, for this article we are only interested in ARP traffic. After setting the network interface to promiscuous mode (so it captures all traffic), this command works on my system:
```
sudo tcpdump -e -n arp
```
On my system, the command above will generate output lines that looks similar to this:
```
23:17:18.347162 f8:47:2a:ad:08:19 > d3:9a:c7:8b:fc:31, ethertype ARP (0x0806), length 42: Reply 10.0.0.1 is-at f8:47:2a:ad:08:19, length 28
23:19:49.175519 f8:47:2a:ad:08:19 > ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 42: Request who-has 10.0.0.23 tell 10.0.0.1, length 28
```

**Extracting Communication Pairs**

We want to know who is talking to whom on the network. For this article we will look at link-level communications. Pairs of Ethernet addresses will give us the information we are looking for. I like Perl, so that's what I will use in this example.
```
perl -n -e 'm/([^ ]+) > ([^ ]+),/ and print "$1,$2\n";'
```
The resulting output looks like this:
```
f8:47:2a:ad:08:19,d3:9a:c7:8b:fc:31
f8:47:2a:ad:08:19,ff:ff:ff:ff:ff:ff
```

Now you have a comma separated data set that can be read into a spreadsheet.


**References**

<a name="1">[1]</a> [TCPDUMP/LIBPCAP public repository](https://www.tcpdump.org).
