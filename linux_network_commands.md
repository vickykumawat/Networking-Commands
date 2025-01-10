# Linux - Network commands

## Routing tables
- Servers maintain routing tables containing the addresses of each node in the network.
- The IP Routing protocols enable routers to build up a forwarding table that correlates final destinations with the next hop addresses.

---

## Common utilities

#### `ifconfig`
- Displays currently active network interfaces.
- We may have to run as the superuser, or at least, give the full path, i.e., /sbin/ifconfig, on some distributions.
- Network Configuration Files are essential to ensure that interfaces function correctly.

#### `ip`
- A very powerful and versatile program.
- Replaces most of the functionality of older `ifconfig`, `route`, `netstat`, etc.
- http://unix.stackexchange.com/questions/83096/what-is-the-difference-between-sbin-ip-route-and-sbin-route

#### `ping`
- Sends a special network packet called an IMCP ECHO_REQUEST to a specified host.
- Check whether a machine attached to the network can receive and send data.
- Confirm that the remote host is online and is responding.
- Measure network latency between machines.

#### `route`
- Used to **view or change the IP routing table**.
- e.g., change the IP routing table to add, delete or modify specific (static) routes to specific hosts or networks

#### `traceroute`
- Displays a listing of all the "hops" that network traffic takes to get from the local system to a specified host.
- Useful for troubleshooting network delays and errors.
- Allows us to isolate connectivity issues between hops, which helps resolve them faster

#### `ethtool`
- Queries network interfaces and can also set various parameters such as the speed.

#### `netstat`
- Displays all active connections and routing tables.
- Useful for monitoring performance and troubleshooting.

#### `nmap`    
- Scans open ports on a network. Important for security analysis

#### `tcpdump`
- Dumps network traffic for analysis.

#### `iptraf`
- Monitors network traffic in text mode.

#### `hostname`
- Used to view a system’s hostname.

---

## Common operations

#### Viewing the IP address
```
$ ip addr show
```

#### Viewing the routing information
```
$ route –n
```

```
$ ip route show
```

#### Adding static route
```
$ route add -net address
```

#### Deleting static route
```
$ route del -net address
```

#### Confirming that the remote host is online and responding
-  check if the remote host is alive and responding
-  `ping <hostname>` then abort by `Ctrl-C`
-  `ping -c <n> <hostname>` to specify the number of packets to be shown

```
$ ping mnishiguchi.com
PING mnishiguchi.com (192.30.252.153): 56 data bytes
64 bytes from 192.30.252.153: icmp_seq=0 ttl=54 time=15.744 ms
64 bytes from 192.30.252.153: icmp_seq=1 ttl=54 time=14.228 ms
^C
--- mnishiguchi.com ping statistics ---
13 packets transmitted, 12 packets received, 7.7% packet loss
round-trip min/avg/max/stddev = 14.228/18.630/24.027/3.395 ms
```

#### Printing the route taken by the packet to reach the network host
- For routers that do not provide identifying information (because of router configuration, network congestion, firewalls, etc.), we see `* * *`.

```
$ traceroute mnishiguchi.com
traceroute to mnishiguchi.com (192.30.252.153), 64 hops max, 52 byte packets
 1  10.0.0.1 (10.0.0.1)  14.108 ms  2.418 ms  2.962 ms
 2  96.120.106.33 (96.120.106.33)  17.234 ms  15.315 ms  11.679 ms
 3  xe-3-1-2-sur03.michiaganave.dc.bad.comcast.net (68.85.147.53)  12.732 ms  11.524 ms  21.986 ms
 4  ae-5-0-ar01.capitolhghts.md.bad.comcast.net (68.86.204.205)  13.062 ms  45.409 ms  18.175 ms
 5  ae13-ar01.capitolhghts.md.bad.comcast.net (68.87.168.61)  18.467 ms  21.439 ms  18.148 ms
 6  be-33657-cr02.ashburn.va.ibone.comcast.net (68.86.90.57)  17.178 ms  16.239 ms  18.044 ms
 7  be-10142-pe01.ashburn.va.ibone.comcast.net (68.86.86.34)  14.702 ms  12.925 ms  16.796 ms
 8  66.208.229.158 (66.208.229.158)  14.368 ms  17.040 ms  13.704 ms
 9  * * *
10  * * *
11  lb-192-30-252-153-iad.github.com (192.30.252.153)  17.718 ms  14.388 ms  12.612 ms
```

#### Showing information about a network device
- `ethtool <devname>`

e.g., showing information about the network device, eth0.
```
test1@ubuntu:~$ ethtool eth0

Settings for eth0:
Supported ports:[TP]    
Supported link modes:   10baseT/Half 10baseT/Full
100baseT/Half 100baseT/Full
1000baseT/Full
Supported pause frame use: No   
Supports auto-negotiation: Yes  
...
```

#### Displaying the kernel's network routing tables

```
$ netstat -r
Routing tables

Internet:
Destination        Gateway            Flags        Refs      Use   Netif Expire
default            10.0.0.1           UGSc          110       46     en0
...

Internet6:
Destination        Gateway            Flags         Netif Expire
default            fe80::200:caff:fe1 UGc             en0
localhost          localhost          UHL             lo0
...
```

#### Viewing a system’s hostname

```
$ hostname

Masas-Mac.local
```

#### Downloading files
- [What is the difference between curl and wget?](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget)
- [curl notes](https://gist.github.com/mnishiguchi/60cb3b6ac795a96594470134a82d72da)

```bash
wget http://mnishiguchi.com
```

```bash
$ curl http://mnishiguchi.com -o outfile.html
```
