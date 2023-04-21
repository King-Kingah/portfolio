---
title: "Must-Know Network and Troubleshooting Tools for Linux"
description: "Important Network and Troubleshooting Tools for Linux"
dateString: Dec 29, 2021
draft: false
tags: ["Linux", "performance", "productivity", "Ubuntu"]
weight: 102
cover:
    image: "/blog/must-know-linux-commands/cover.jpg"
---


# Introduction
At times, navigating Linux operating system can be daunting for new users and so is the troubleshooting process. Unlike in Windows where one simply navigates to the control panel to troubleshoot network issues, Linux is often not so direct and obvious.

Therefore, this article seeks to simplify this process by discussing some of the basic Linux commands to view the network status and troubleshoot any related issues.

# Tools and Commands

## 1. ifconfig

**ifconfig** displays the status of the currently active interfaces if not argument is given.
Useful options:

- `-a` to view all interfaces which are currently available, even if down
- `-s` display a short list (like `netstat -i`)

Example of **ifconfig** output:

```terminal

eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:0a  
          inet addr:172.17.0.10  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2235 errors:0 dropped:0 overruns:0 frame:0
          TX packets:667 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:508519 (508.5 KB)  TX bytes:92572 (92.5 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

- `eth0` - the Ethernet interface. Can be **eth1, eth2,..., eth-nth**
- `lo` - the loopback interface which is used by the system to communicate with itself.


## 2. ping

Basically used to test connectivity. Common and handy **flags**:

- `-c`, `-count` *count*

Stop after sending (and receiving) count ECHO_RESPONSE packets.

- `-q`, `-quiet`

Quiet output. Nothing is displayed except the summary lines at startup time and when finished.

- `-i`, `-interval` *wait*

Wait *wait* seconds *between sending each packet*. The default is to wait for one second between each packet. This option is incompatible with the `-f` option.


## 3. whois

**whois** searches for an object in a **RFC 3912 (WHOIS)** database.
**WHOIS** is a query and response protocol that is commonly used for accessing databases that contain the registered users of an Internet resource, such as a [domain name] or a [IP] [address] block, but it can also be used for a wider range of information.

The majority of recent implementations of **whois** attempt to guess the correct server to query for the requested object. **Whois** will connect to [whois.networksolutions.com](whois.networksolutions.com) for **NIC** handles or [whois.arin.net](whois.arin.net) for [IPv4] addresses and network names if no guess can be made.
Often not installed by default on most systems but can be installed using **apt** - `sudo apt install whois`


## 4. nslookup

`nslookup` is used to determine the Internet name servers interactively. It stands for Name Server Lookup.
**nslookup** has two modes:

1. **Interactive**: useful in querying name servers for information about 
various hosts and domains or to print a list of hosts in a domain.
2. **Non-interactive**: useful to print just the name and requested information for a host or domain.
Usage:

```terminal
nslookup [-option] [name | -] [server]
```

Where host [server] looks for information for a host using the default server or server, if one is given.

## 5. traceroute

`traceroute` prints the route packets trace to network host.
Traceroute records the path packets follow from an IP network to a certain host. It makes use of the IP protocol's time to live (**TTL**) parameter to try to get an ICMP TIME EXCEEDED response from each gateway on the route to the host.

## 6. netstat

`netstat` gets the network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.
This tool prints information about the Linux subsytem. Useful arguments include:

```terminal
--route, -r
```

> Display the kernel routing tables. See the description in route(8) for details. `netstat -r` and `route`
`-e` produce the same output.

```terminal
--groups, -g
```

> Display multicast group membership information for **IPv4** and **IPv6**.

```terminal
--interfaces, -i
```

> Display a table of all network interfaces.

```terminal
--statistics, -s
```

> Display summary statistics for each protocol.

## 7. dig

`dig` is a **DNS lookup** tool. This means `dig` is a flexible tool for interrogating **DNS name servers**. It performs DNS lookups and displays the answers that are returned from the name server(s) that were queried.

Most DNS administrators use dig to troubleshoot DNS problems because of its flexibility, ease of use and clarity of output. Other lookup tools tend to have less functionality than dig.

Simple usage:

```terminal
dig @server name type
```

Where:
> **server** is the name or IP address of the name server to query. When the supplied server argument is a hostname, `dig` resolves that name before querying that name server.
If no server argument is provided, `dig` consults `/etc/resolv.conf`; if an address is found there, it queries the name server at that address. If either of the -4 or -6 options are in use, then only addresses for the corresponding transport will be tried. If no usable addresses are found, `dig` will send the query to the local host. The reply from the name server that responds is displayed.

> **name**
is the name of the resource record that is to be looked up.

> **type**
indicates what type of query is required — ANY, A, MX, SIG, etc. type can be any valid query type. If no type argument is supplied, `dig` will perform a lookup for an **A record**.

# Conclusion

This article looked at seven handy network tools for Linux: *ifconfig, ping, whois, nslookup, traceroute, netstat* and *dig*.

We also discussed the usage of each tool and its importance in troubleshooting various network issues.