# Networking Commands

Networking commands are essential tools for configuring, inspecting, monitoring, and troubleshooting computer networks.

Network administrators and engineers use command-line utilities to:

* Inspect network interfaces and IP addresses.
* Examine routing tables and gateways.
* Test connectivity between devices.
* Diagnose DNS resolution problems.
* Identify listening ports and active connections.
* Inspect ARP and neighbor tables.
* Configure and troubleshoot firewalls.
* Capture and analyze network packets.
* Monitor wireless connections.
* Manage MikroTik RouterOS devices.

This document focuses on two environments:

* **Ubuntu/Linux:** Commands commonly available on Ubuntu and other Linux distributions.
* **MikroTik RouterOS v7:** Commands for managing MikroTik routers through Terminal, SSH, or WinBox Terminal.

Some Linux commands require additional packages. Install missing utilities using:

```bash
sudo apt update
sudo apt install iproute2 iputils-ping traceroute dnsutils \
  net-tools curl wget tcpdump nmap mtr-tiny \
  ethtool iw wireless-tools
```

# 🗁 Network Interface Commands

### 🖆 Display Network Interfaces

Display all network interfaces:

```bash
ip link show
```

Display interfaces with IP addresses:

```bash
ip address show
```

Short form:

```bash
ip addr
```

Display a specific interface:

```bash
ip addr show dev enp0s3
```

The interface name varies between systems. Examples include `eth0`, `ens33`, `enp0s3`, and `wlan0`.

### 🖆 Enable and Disable Interfaces

Enable an interface:

```bash
sudo ip link set enp0s3 up
```

Disable an interface:

```bash
sudo ip link set enp0s3 down
```

These changes are temporary and may be overwritten by the network configuration manager.

### 🖆 Display Interface Statistics

```bash
ip -s link show
```

Display detailed Ethernet information:

```bash
sudo ethtool enp0s3
```

Display interface driver information:

```bash
sudo ethtool -i enp0s3
```

Display negotiated link speed:

```bash
sudo ethtool enp0s3 | grep -E 'Speed|Duplex|Link detected'
```

### 🖆 Legacy Interface Commands

The `ifconfig` utility belongs to the older `net-tools` package.

Display interfaces:

```bash
ifconfig
```

Display all interfaces, including inactive ones:

```bash
ifconfig -a
```

Modern Linux systems should generally use `ip` instead of `ifconfig`.

# 🗁 IP Address Commands

### 🖆 Display IP Addresses

Display IPv4 and IPv6 addresses:

```bash
ip address show
```

Display only IPv4 addresses:

```bash
ip -4 address show
```

Display only IPv6 addresses:

```bash
ip -6 address show
```

Display the primary IPv4 address assigned to a specific interface:

```bash
ip -4 addr show dev enp0s3
```

### 🖆 Assign an IP Address Temporarily

Add an IPv4 address:

```bash
sudo ip address add 192.168.1.10/24 dev enp0s3
```

Add another address to the same interface:

```bash
sudo ip address add 192.168.1.20/24 dev enp0s3
```

Remove an address:

```bash
sudo ip address del 192.168.1.20/24 dev enp0s3
```

These commands modify the running network configuration. They do not permanently configure the address across reboots.

### 🖆 Display MAC Addresses

```bash
ip link show
```

Display MAC addresses in a concise format:

```bash
ip -br link
```

### 🖆 Display the Default Gateway

```bash
ip route show default
```

Display the complete routing table:

```bash
ip route show
```

### 🖆 Display Hostname and System Network Identity

Display the hostname:

```bash
hostname
```

Display the fully qualified domain name, if configured:

```bash
hostname -f
```

Display hostname and related system information:

```bash
hostnamectl
```

# 🗁 Routing Commands

### 🖆 Display the Routing Table

Display IPv4 routes:

```bash
ip route show
```

Display IPv6 routes:

```bash
ip -6 route show
```

Display the routing table in a concise format:

```bash
ip -br route
```

### 🖆 Add a Static Route

Add a route to a remote network:

```bash
sudo ip route add 192.168.20.0/24 via 192.168.1.1
```

Specify the outgoing interface:

```bash
sudo ip route add 192.168.30.0/24 dev enp0s3
```

Add a default route:

```bash
sudo ip route add default via 192.168.1.1
```

### 🖆 Remove a Route

Remove a specific route:

```bash
sudo ip route del 192.168.20.0/24
```

Remove the default route:

```bash
sudo ip route del default
```

### 🖆 Inspect the Route to a Destination

Determine how Linux will reach a destination:

```bash
ip route get 8.8.8.8
```

This displays the selected route, outgoing interface, preferred source address, and next hop when applicable.

### 🖆 Legacy Routing Commands

Display the routing table:

```bash
route -n
```

This command is provided by the older `net-tools` package. Prefer `ip route`.

# 🗁 DNS Commands

### 🖆 Test DNS Resolution

Resolve a domain name:

```bash
nslookup example.com
```

Query a DNS server:

```bash
nslookup example.com 8.8.8.8
```

Use `dig` to inspect DNS records:

```bash
dig example.com
```

Query a specific DNS server:

```bash
dig @8.8.8.8 example.com
```

Display only the answer section:

```bash
dig example.com +short
```

### 🖆 Query Specific DNS Record Types

Query an A record:

```bash
dig example.com A
```

Query an AAAA record:

```bash
dig example.com AAAA
```

Query a MX record:

```bash
dig example.com MX
```

Query a NS record:

```bash
dig example.com NS
```

Query a TXT record:

```bash
dig example.com TXT
```

Query a CNAME record:

```bash
dig www.example.com CNAME
```

### 🖆 Inspect Local DNS Configuration

Display configured DNS information:

```bash
resolvectl status
```

Inspect the resolver configuration file:

```bash
cat /etc/resolv.conf
```

On systems using `systemd-resolved`, `/etc/resolv.conf` may point to a local stub resolver rather than directly listing upstream DNS servers.

### 🖆 Flush the DNS Cache

On Ubuntu systems using `systemd-resolved`:

```bash
sudo resolvectl flush-caches
```

Inspect DNS statistics:

```bash
resolvectl statistics
```

# 🗁 Connectivity Testing Commands

### 🖆 Ping

Test connectivity to an IP address:

```bash
ping 8.8.8.8
```

Test connectivity to a domain:

```bash
ping example.com
```

Send a specific number of packets:

```bash
ping -c 5 8.8.8.8
```

Specify the packet size:

```bash
ping -s 1200 8.8.8.8
```

Specify the interval between packets:

```bash
ping -i 2 8.8.8.8
```

Ping a destination using IPv6:

```bash
ping -6 2001:4860:4860::8888
```

Important output fields:

* `time`: Round-trip time for the packet.
* `ttl`: Remaining IPv4 Time To Live value in the response.
* `packet loss`: Percentage of packets that did not receive replies.
* `rtt`: Round-trip time statistics.

A failed ping does not necessarily mean the destination is unavailable. Firewalls and network policies may block or deprioritize ICMP.

### 🖆 Traceroute

Display the network path to a destination:

```bash
traceroute example.com
```

Trace a route to an IP address:

```bash
traceroute 8.8.8.8
```

Use ICMP probes:

```bash
traceroute -I 8.8.8.8
```

Use TCP probes:

```bash
traceroute -T -p 443 example.com
```

Limit the maximum number of hops:

```bash
traceroute -m 20 example.com
```

### 🖆 Tracepath

Trace the route and investigate Path MTU:

```bash
tracepath example.com
```

Trace a specific destination:

```bash
tracepath 8.8.8.8
```

### 🖆 MTR

MTR combines continuous ping and traceroute-style path analysis.

Run MTR:

```bash
mtr example.com
```

Generate a report with 10 probes:

```bash
mtr -r -c 10 example.com
```

Use numeric addresses without DNS lookups:

```bash
mtr -n example.com
```

MTR is useful for observing packet loss and latency across multiple hops. Loss reported by an intermediate router may reflect ICMP rate limiting rather than actual transit packet loss.

### 🖆 Test HTTP and HTTPS Connectivity

Check HTTP response headers:

```bash
curl -I http://example.com
```

Check HTTPS response headers:

```bash
curl -I https://example.com
```

Display detailed connection information:

```bash
curl -v https://example.com
```

Test whether a TCP port is reachable:

```bash
curl -v telnet://192.168.1.100:22
```

# 🗁 Ports and Socket Commands

### 🖆 Display Listening Ports

Display listening TCP and UDP sockets:

```bash
sudo ss -tuln
```

Display listening sockets with process information:

```bash
sudo ss -tulnp
```

Display established TCP connections:

```bash
ss -tn state established
```

Display all TCP connections:

```bash
ss -ant
```

Display all UDP sockets:

```bash
ss -anu
```

### 🖆 Find a Specific Port

Check whether port 22 is listening:

```bash
sudo ss -tulnp | grep ':22'
```

Check whether port 80 is listening:

```bash
sudo ss -tulnp | grep ':80'
```

Check whether port 443 is listening:

```bash
sudo ss -tulnp | grep ':443'
```

### 🖆 Identify Processes Using Ports

```bash
sudo lsof -i
```

Find the process using TCP port 8080:

```bash
sudo lsof -i :8080
```

Find processes using a specific UDP port:

```bash
sudo lsof -i UDP:53
```

### 🖆 Test Remote TCP Ports

Using Netcat:

```bash
nc -vz 192.168.1.100 22
```

Test an HTTPS port:

```bash
nc -vz example.com 443
```

Test a range of ports:

```bash
nc -vz 192.168.1.100 20-25
```

Netcat options and output can vary between implementations.

### 🖆 Scan Ports with Nmap

Scan a host:

```bash
nmap 192.168.1.100
```

Scan specific ports:

```bash
nmap -p 22,80,443 192.168.1.100
```

Scan all TCP ports:

```bash
nmap -p- 192.168.1.100
```

Detect service versions:

```bash
nmap -sV 192.168.1.100
```

Scan a local subnet:

```bash
nmap -sn 192.168.1.0/24
```

Use scanning only on networks and systems you are authorized to assess.

# 🗁 ARP and Neighbor Commands

### 🖆 Display the ARP Table

Display the IPv4 neighbor table:

```bash
ip neigh show
```

Display neighbors on a specific interface:

```bash
ip neigh show dev enp0s3
```

Display the legacy ARP table:

```bash
arp -n
```

### 🖆 Generate an ARP Entry

Ping a local device to trigger neighbor discovery:

```bash
ping -c 1 192.168.1.20
```

Then inspect the neighbor table:

```bash
ip neigh show
```

### 🖆 Delete a Neighbor Entry

```bash
sudo ip neigh del 192.168.1.20 dev enp0s3
```

### 🖆 Monitor Neighbor Changes

```bash
ip monitor neigh
```

This command continuously displays neighbor-table changes.

IPv4 commonly uses ARP for local address resolution. IPv6 uses Neighbor Discovery Protocol (NDP), which operates through ICMPv6.

# 🗁 Firewall Commands

### 🖆 UFW Firewall

UFW is a commonly used firewall management interface on Ubuntu.

Check firewall status:

```bash
sudo ufw status
```

Display detailed status:

```bash
sudo ufw status verbose
```

Enable UFW:

```bash
sudo ufw enable
```

Disable UFW:

```bash
sudo ufw disable
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Allow a specific source IP:

```bash
sudo ufw allow from 192.168.1.50
```

Allow a source IP to access a specific port:

```bash
sudo ufw allow from 192.168.1.50 to any port 22 proto tcp
```

Delete a firewall rule:

```bash
sudo ufw delete allow 80/tcp
```

List numbered rules:

```bash
sudo ufw status numbered
```

### 🖆 nftables

Display the nftables ruleset:

```bash
sudo nft list ruleset
```

Display rules in a specific table:

```bash
sudo nft list table inet filter
```

Display available tables:

```bash
sudo nft list tables
```

Inspect nftables service status:

```bash
sudo systemctl status nftables
```

### 🖆 iptables

Display IPv4 filter rules:

```bash
sudo iptables -L -n -v
```

Display NAT rules:

```bash
sudo iptables -t nat -L -n -v
```

Display rules with line numbers:

```bash
sudo iptables -L -n --line-numbers
```

Display IPv6 firewall rules:

```bash
sudo ip6tables -L -n -v
```

Modern Linux systems commonly use nftables as the underlying packet-filtering framework. The `iptables` command may operate through an nftables compatibility backend.

# 🗁 Packet Capture Commands

### 🖆 tcpdump

`tcpdump` captures and displays packets traveling through network interfaces.

List available capture interfaces:

```bash
sudo tcpdump -D
```

Capture packets on a specific interface:

```bash
sudo tcpdump -i enp0s3
```

Capture packets on all interfaces:

```bash
sudo tcpdump -i any
```

Capture only a specific number of packets:

```bash
sudo tcpdump -i enp0s3 -c 20
```

Capture traffic involving a host:

```bash
sudo tcpdump -i enp0s3 host 192.168.1.100
```

Capture traffic from a source IP:

```bash
sudo tcpdump -i enp0s3 src host 192.168.1.100
```

Capture traffic to a destination IP:

```bash
sudo tcpdump -i enp0s3 dst host 192.168.1.100
```

Capture DNS traffic:

```bash
sudo tcpdump -i enp0s3 port 53
```

Capture HTTP traffic:

```bash
sudo tcpdump -i enp0s3 tcp port 80
```

Capture HTTPS traffic:

```bash
sudo tcpdump -i enp0s3 tcp port 443
```

Capture ICMP traffic:

```bash
sudo tcpdump -i enp0s3 icmp
```

Capture traffic from or to a subnet:

```bash
sudo tcpdump -i enp0s3 net 192.168.1.0/24
```

### 🖆 Save Packet Captures

Save captured packets to a PCAP file:

```bash
sudo tcpdump -i enp0s3 -w capture.pcap
```

Read an existing PCAP file:

```bash
tcpdump -r capture.pcap
```

Read a capture and display packet contents in hexadecimal and ASCII:

```bash
tcpdump -nn -XX -r capture.pcap
```

Display packet contents in ASCII:

```bash
tcpdump -A -r capture.pcap
```

The PCAP file can also be opened in Wireshark for graphical packet analysis.

### 🖆 Wireshark

Launch Wireshark:

```bash
wireshark
```

Capture traffic from the command line using `tshark`:

```bash
tshark -i enp0s3
```

Read a PCAP file:

```bash
tshark -r capture.pcap
```

Capture DNS packets:

```bash
tshark -i enp0s3 -f "port 53"
```

Display packets matching a DNS display filter:

```bash
tshark -r capture.pcap -Y dns
```

Capture filters and display filters use different syntax. For example, `port 53` is a capture filter, while `dns` is a display filter.

# 🗁 DHCP Commands

### 🖆 Inspect DHCP Client Information

Display interface addresses:

```bash
ip address show
```

Inspect DHCP-related NetworkManager information:

```bash
nmcli device show enp0s3
```

Display NetworkManager connection information:

```bash
nmcli connection show
```

Display active connections:

```bash
nmcli connection show --active
```

### 🖆 Renew a DHCP Lease with NetworkManager

Reconnect an interface:

```bash
sudo nmcli device disconnect enp0s3
sudo nmcli device connect enp0s3
```

Display device status:

```bash
nmcli device status
```

### 🖆 Renew a DHCP Lease with dhclient

If the system uses `dhclient`, release the current lease:

```bash
sudo dhclient -r enp0s3
```

Request a new lease:

```bash
sudo dhclient enp0s3
```

`dhclient` may not be installed or used by default on modern Ubuntu installations. Use the DHCP client associated with the active network manager.

### 🖆 Inspect DHCP Logs

View NetworkManager logs:

```bash
sudo journalctl -u NetworkManager
```

View recent logs:

```bash
sudo journalctl -u NetworkManager -n 100
```

Follow logs in real time:

```bash
sudo journalctl -u NetworkManager -f
```

# 🗁 Wireless Networking Commands

### 🖆 Display Wireless Interfaces

Display wireless devices:

```bash
iw dev
```

Display wireless interface information:

```bash
iwconfig
```

Display wireless interface status:

```bash
ip link show
```

### 🖆 Scan Nearby Wi-Fi Networks

Scan using `iw`:

```bash
sudo iw dev wlan0 scan
```

Scan using NetworkManager:

```bash
nmcli device wifi list
```

### 🖆 Connect to a Wi-Fi Network

Connect using NetworkManager:

```bash
nmcli device wifi connect "WiFi-Name" password "WiFi-Password"
```

Display saved connections:

```bash
nmcli connection show
```

Disconnect a wireless interface:

```bash
sudo nmcli device disconnect wlan0
```

### 🖆 Inspect Wireless Connection Information

```bash
iw dev wlan0 link
```

Display wireless device details:

```bash
iw phy
```

Display NetworkManager device details:

```bash
nmcli device show wlan0
```

Wireless interface names vary. Identify the correct interface using `iw dev` or `nmcli device status`.

# 🗁 MikroTik RouterOS Commands

### 🖆 RouterOS Command-Line Basics

MikroTik RouterOS provides a hierarchical command-line interface.

Display system resources:

```routeros
/system resource print
```

Display system identity:

```routeros
/system identity print
```

Display RouterOS version:

```routeros
/system package print
```

Display system clock:

```routeros
/system clock print
```

Display system uptime:

```routeros
/system resource print
```

### 🖆 Interface Management

Display interfaces:

```routeros
/interface print
```

Display detailed interface information:

```routeros
/interface print detail
```

Display interface statistics:

```routeros
/interface monitor-traffic ether1
```

Enable an interface:

```routeros
/interface enable ether1
```

Disable an interface:

```routeros
/interface disable ether1
```

Rename an interface:

```routeros
/interface set ether1 name=WAN
```

Display Ethernet link information:

```routeros
/interface ethernet monitor ether1 once
```

### 🖆 IP Address Management

Display configured IP addresses:

```routeros
/ip address print
```

Add an IP address:

```routeros
/ip address add address=192.168.1.1/24 interface=bridge
```

Remove an IP address by its item number:

```routeros
/ip address remove 0
```

Inspect a specific address:

```routeros
/ip address print detail
```

### 🖆 Routing Management

Display the IPv4 routing table:

```routeros
/ip route print
```

Display detailed route information:

```routeros
/ip route print detail
```

Add a static route:

```routeros
/ip route add dst-address=192.168.20.0/24 gateway=192.168.1.2
```

Add a default route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.254
```

Add a route with a specific administrative distance:

```routeros
/ip route add dst-address=192.168.30.0/24 gateway=192.168.1.2 distance=10
```

Remove a route by its item number:

```routeros
/ip route remove 0
```

Inspect IPv6 routes:

```routeros
/ipv6 route print detail
```

A route's distance determines preference among competing routes, but distance alone does not detect an upstream Internet failure. Reliable failover requires an appropriate gateway health-check strategy, such as recursive routing or another monitoring mechanism.

### 🖆 DNS Management

Display DNS configuration:

```routeros
/ip dns print
```

Configure DNS servers:

```routeros
/ip dns set servers=1.1.1.1,8.8.8.8
```

Enable DNS requests from LAN clients:

```routeros
/ip dns set allow-remote-requests=yes
```

Inspect DNS cache:

```routeros
/ip dns cache print
```

Clear DNS cache:

```routeros
/ip dns cache flush
```

Allow remote DNS requests only when appropriate firewall rules restrict access to trusted networks. Do not expose an unrestricted DNS resolver to the Internet.

### 🖆 Connectivity Testing

Ping an IP address:

```routeros
/ping 8.8.8.8
```

Ping a domain:

```routeros
/ping example.com
```

Specify the source address:

```routeros
/ping 8.8.8.8 src-address=192.168.1.1
```

Specify the outgoing interface:

```routeros
/ping 8.8.8.8 interface=ether1
```

Trace a route:

```routeros
/tool traceroute 8.8.8.8
```

Run a bandwidth test:

```routeros
/tool bandwidth-test address=192.168.1.2 user=test password=secret direction=both
```

Bandwidth testing consumes network resources and can affect router performance. Run it only against an authorized test endpoint and avoid unnecessary tests on production routers.

### 🖆 ARP and Neighbor Management

Display the ARP table:

```routeros
/ip arp print
```

Display detailed ARP information:

```routeros
/ip arp print detail
```

Inspect IPv6 neighbors:

```routeros
/ipv6 neighbor print
```

Monitor ARP entries:

```routeros
/ip arp print where interface=bridge
```

### 🖆 Firewall Inspection

Display IPv4 firewall filter rules:

```routeros
/ip firewall filter print
```

Display detailed filter rules:

```routeros
/ip firewall filter print detail
```

Display NAT rules:

```routeros
/ip firewall nat print
```

Display mangle rules:

```routeros
/ip firewall mangle print
```

Display address lists:

```routeros
/ip firewall address-list print
```

Display connection tracking entries:

```routeros
/ip firewall connection print
```

Display IPv6 firewall rules:

```routeros
/ipv6 firewall filter print
```

### 🖆 NAT Inspection

Display source NAT rules:

```routeros
/ip firewall nat print where chain=srcnat
```

Display destination NAT rules:

```routeros
/ip firewall nat print where chain=dstnat
```

A common masquerade rule for a WAN interface is:

```routeros
/ip firewall nat add chain=srcnat out-interface-list=WAN action=masquerade
```

This example assumes that an interface list named `WAN` already exists and contains the appropriate Internet-facing interface.

### 🖆 DHCP Server Management

Display DHCP servers:

```routeros
/ip dhcp-server print
```

Display DHCP server networks:

```routeros
/ip dhcp-server network print
```

Display DHCP address pools:

```routeros
/ip pool print
```

Display active DHCP leases:

```routeros
/ip dhcp-server lease print
```

Display detailed DHCP leases:

```routeros
/ip dhcp-server lease print detail
```

Make a lease static by its item number:

```routeros
/ip dhcp-server lease make-static 0
```

Release a dynamic lease by removing its entry:

```routeros
/ip dhcp-server lease remove 0
```

### 🖆 VLAN and Bridge Inspection

Display bridges:

```routeros
/interface bridge print
```

Display bridge ports:

```routeros
/interface bridge port print
```

Display VLAN configuration:

```routeros
/interface bridge vlan print
```

Display VLAN interface details:

```routeros
/interface vlan print detail
```

Display the bridge host table:

```routeros
/interface bridge host print
```

### 🖆 Wireless Inspection

For RouterOS devices using the legacy wireless package:

```routeros
/interface wireless print
```

Display wireless registration information:

```routeros
/interface wireless registration-table print
```

For devices using the newer WiFi configuration menus, inspect:

```routeros
/interface wifi print
```

Display WiFi registration information:

```routeros
/interface wifi registration-table print
```

Available wireless menus depend on the device, RouterOS version, and installed wireless package.

### 🖆 Monitoring and Traffic Analysis

Monitor interface traffic:

```routeros
/interface monitor-traffic ether1
```

Display CPU and memory usage:

```routeros
/system resource print
```

Display active processes:

```routeros
/tool profile
```

Monitor real-time traffic:

```routeros
/tool torch ether1
```

Capture packets using the built-in sniffer:

```routeros
/tool sniffer start interface=ether1
```

Stop packet capture:

```routeros
/tool sniffer stop
```

Display sniffer configuration and status:

```routeros
/tool sniffer print
```

### 🖆 Logging and Diagnostics

Display system logs:

```routeros
/log print
```

Display the most recent log entries:

```routeros
/log print without-paging
```

Display configured logging actions:

```routeros
/system logging print
```

Display installed packages:

```routeros
/system package print
```

Display system health information where supported:

```routeros
/system health print
```

The available health sensors depend on the router hardware.

### 🖆 RouterOS Backup and Export

Create a binary backup:

```routeros
/system backup save name=router-backup
```

Export the configuration as readable commands:

```routeros
/export file=router-config
```

Export sensitive configuration details when required:

```routeros
/export show-sensitive file=router-config-sensitive
```

Sensitive exports may contain credentials, keys, or other confidential configuration. Store them securely and restrict access.

# 🗁 Troubleshooting Workflows

### 🖆 Check Local Network Connectivity

When a Linux computer cannot access the network, follow this sequence:

```bash
ip link show
ip address show
ip route show
ip neigh show
ping -c 4 127.0.0.1
ping -c 4 <default-gateway>
ping -c 4 8.8.8.8
```

Interpretation:

* Interface is down: investigate physical connectivity or interface configuration.
* No valid IP address: investigate DHCP or static IP configuration.
* Gateway unreachable: investigate the local network, VLAN, switch port, or gateway.
* Public IP unreachable: investigate routing, firewall, NAT, or upstream connectivity.

### 🖆 Diagnose DNS Problems

First test connectivity using an IP address:

```bash
ping -c 4 8.8.8.8
```

Then test domain resolution:

```bash
dig example.com
```

Query a known DNS server directly:

```bash
dig @1.1.1.1 example.com
```

Interpretation:

* IP connectivity works but DNS resolution fails: investigate DNS configuration, resolver reachability, or DNS filtering.
* Direct DNS query works but normal resolution fails: investigate the system resolver configuration.
* Both fail: investigate general connectivity before focusing on DNS.

### 🖆 Diagnose a Port Connectivity Problem

Check whether the application is listening locally:

```bash
sudo ss -tulnp
```

Test the remote port:

```bash
nc -vz 192.168.1.100 443
```

Inspect firewall rules:

```bash
sudo ufw status verbose
sudo nft list ruleset
```

Capture relevant traffic:

```bash
sudo tcpdump -ni any host 192.168.1.100 and port 443
```

Investigate the application, local firewall, network firewall, routing path, and remote service.

### 🖆 Diagnose Packet Loss

Run a basic ping test:

```bash
ping -c 20 8.8.8.8
```

Inspect the path:

```bash
mtr -r -c 20 8.8.8.8
```

Check interface counters:

```bash
ip -s link show
```

Investigate:

* Interface errors and dropped packets.
* Congestion and bandwidth utilization.
* Duplex or link-speed mismatches.
* Wireless interference.
* Routing problems.
* Firewall policies and ICMP rate limiting.

### 🖆 Diagnose a MikroTik WAN Problem

Start with interface and address inspection:

```routeros
/interface print
/ip address print
```

Inspect the default route:

```routeros
/ip route print detail
```

Test the upstream gateway:

```routeros
/ping 192.168.1.254
```

Test public IP connectivity:

```routeros
/ping 8.8.8.8
```

Test DNS resolution:

```routeros
/ping example.com
/ip dns cache print
```

Inspect NAT and firewall rules:

```routeros
/ip firewall nat print
/ip firewall filter print
```

Monitor WAN traffic:

```routeros
/interface monitor-traffic ether1
```

Use the results to isolate whether the issue is related to the physical link, IP configuration, gateway, routing, NAT, firewall, or DNS.

# 🗁 Command Reference Summary

### 🖆 Linux Networking Commands

| Purpose | Command |
| --- | --- |
| Display interfaces | `ip link show` |
| Display IP addresses | `ip addr` |
| Display interface statistics | `ip -s link` |
| Display routes | `ip route` |
| Display default gateway | `ip route show default` |
| Inspect destination route | `ip route get 8.8.8.8` |
| Test connectivity | `ping 8.8.8.8` |
| Trace network path | `traceroute 8.8.8.8` |
| Inspect DNS | `dig example.com` |
| Display neighbor table | `ip neigh` |
| Display listening ports | `ss -tulnp` |
| Inspect processes using ports | `lsof -i` |
| Test TCP port | `nc -vz host port` |
| Scan authorized host | `nmap host` |
| Inspect firewall | `sudo ufw status` |
| Display nftables rules | `sudo nft list ruleset` |
| Capture packets | `sudo tcpdump -i any` |
| Display Wi-Fi networks | `nmcli device wifi list` |
| Display network connections | `nmcli connection show` |

### 🖆 MikroTik RouterOS Commands

| Purpose | Command |
| --- | --- |
| Display system resources | `/system resource print` |
| Display interfaces | `/interface print` |
| Monitor interface traffic | `/interface monitor-traffic ether1` |
| Display IP addresses | `/ip address print` |
| Display routes | `/ip route print detail` |
| Display DNS configuration | `/ip dns print` |
| Display DNS cache | `/ip dns cache print` |
| Ping a destination | `/ping 8.8.8.8` |
| Trace network path | `/tool traceroute 8.8.8.8` |
| Display ARP table | `/ip arp print` |
| Display firewall filters | `/ip firewall filter print` |
| Display NAT rules | `/ip firewall nat print` |
| Display DHCP leases | `/ip dhcp-server lease print` |
| Display bridge ports | `/interface bridge port print` |
| Display VLAN configuration | `/interface bridge vlan print` |
| Monitor traffic | `/tool torch ether1` |
| Display logs | `/log print` |
| Export configuration | `/export file=router-config` |

# 🗁 Best Practices

* Understand what a command does before executing it on a production system.
* Prefer modern Linux utilities such as `ip`, `ss`, and `resolvectl` over legacy alternatives.
* Use `man command` to read the manual for a Linux command.
* Use `command --help` to inspect supported options.
* Use RouterOS `?` and Tab completion to explore available commands.
* Use `print detail` when you need more configuration information in RouterOS.
* Capture packet traffic on the correct interface and use appropriate filters.
* Avoid exposing sensitive information in terminal output, logs, exports, or screenshots.
* Record diagnostic results before changing network configuration.
* Verify connectivity after every significant configuration change.

Networking commands are most effective when used systematically. Start at the physical and interface layers, verify IP addressing and routing, then investigate DNS, transport ports, firewall policies, and application behavior.
