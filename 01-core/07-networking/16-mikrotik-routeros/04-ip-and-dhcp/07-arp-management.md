# ARP Management

Address Resolution Protocol (ARP) is used in IPv4 networks to resolve an IP address into its corresponding MAC address.

IP communication uses Layer 3 addresses, but Ethernet frames require Layer 2 MAC addresses for delivery across a local network.

For example, suppose a MikroTik router has the following LAN configuration:

| Device | IP Address | MAC Address |
| --- | --- | --- |
| MikroTik Router | `192.168.10.1` | `AA:BB:CC:DD:EE:01` |
| Client PC | `192.168.10.10` | `AA:BB:CC:DD:EE:10` |

When the router needs to send an Ethernet frame to `192.168.10.10`, it must know the client's MAC address.

ARP performs this resolution.

# 🗁 How ARP Works

ARP resolution normally follows this process:

* A device checks its local ARP cache for the destination IP address.
* If an entry exists, it uses the associated MAC address.
* If no entry exists, it broadcasts an ARP request on the local Layer 2 network.
* The device owning the requested IP address responds with its MAC address.
* The requesting device stores the mapping in its ARP cache.
* Communication can then continue using the resolved MAC address.

Example ARP request:

```text
Who has 192.168.10.10?
Tell 192.168.10.1
```

Example ARP reply:

```text
192.168.10.10 is at AA:BB:CC:DD:EE:10
```

ARP broadcasts remain within their Layer 2 broadcast domain. Routers do not forward ordinary ARP broadcasts between different Layer 3 networks.

# 🗁 ARP Table in RouterOS

RouterOS maintains an ARP table containing IP-to-MAC address mappings.

The ARP table can contain:

* Dynamic entries learned automatically.
* Static entries configured manually.
* Entries associated with DHCP leases.
* Proxy ARP entries.

The ARP configuration menu is:

```routeros
/ip arp
```

Display the ARP table:

```routeros
/ip arp print
```

Display detailed information:

```routeros
/ip arp print detail
```

Display entries associated with a particular interface:

```routeros
/ip arp print where interface=bridge-LAN
```

Display an entry for a specific IP address:

```routeros
/ip arp print where address=192.168.10.10
```

# 🗁 Understanding ARP Entry Properties

RouterOS provides several properties to describe ARP entries.

| Property | Description |
| --- | --- |
| `address` | IPv4 address associated with the entry |
| `mac-address` | MAC address associated with the IPv4 address |
| `interface` | Interface where the ARP entry is associated |
| `dynamic` | Indicates that the entry was learned dynamically |
| `complete` | Indicates that the MAC address resolution is complete |
| `invalid` | Indicates that the entry is invalid |
| `disabled` | Indicates that the entry is disabled |
| `dhcp` | Indicates that the entry was added by the DHCP server |
| `status` | Shows the ARP neighbor state |

Common ARP neighbor states include:

| Status | Meaning |
| --- | --- |
| `incomplete` | MAC address resolution has not completed |
| `reachable` | The neighbor was successfully resolved and is considered reachable |
| `stale` | The entry exists but has not recently been confirmed |
| `delay` | Neighbor validation has been delayed |
| `probe` | RouterOS is actively probing the neighbor |
| `failed` | ARP resolution failed |
| `permanent` | The entry is manually configured and persistent |

The exact flags displayed by `print` depend on the entry and RouterOS version. Use `print detail` to inspect the properties and state.

# 🗁 Dynamic ARP Entries

Dynamic ARP entries are automatically created when RouterOS learns the relationship between an IPv4 address and a MAC address.

The default ARP mode is `enabled`.

Example:

```routeros
/ip arp print
```

A dynamic entry might look conceptually like this:

```text
Flags: D - DYNAMIC
 #   ADDRESS          MAC-ADDRESS        INTERFACE
 0 D 192.168.10.10    AA:BB:CC:DD:EE:10  bridge-LAN
```

Dynamic entries are maintained according to ARP neighbor reachability and timeout behavior.

They are not intended to provide permanent IP-to-MAC bindings.

# 🗁 Static ARP Entries

A static ARP entry manually associates an IPv4 address with a specific MAC address.

Static entries are useful when a device should always be associated with a known IP and MAC address.

Common use cases include:

* Servers with fixed IP addresses.
* Network infrastructure devices.
* Controlled LAN environments.
* IP-to-MAC binding policies.
* Networks using the `reply-only` ARP mode.

Add a static ARP entry:

```routeros
/ip arp add address=192.168.10.10 mac-address=AA:BB:CC:DD:EE:10 interface=bridge-LAN
```

Verify the entry:

```routeros
/ip arp print detail where address=192.168.10.10
```

Remove a static ARP entry:

```routeros
/ip arp remove [find address=192.168.10.10]
```

Static ARP entries should match the actual IP address, MAC address, and interface of the intended device.

# 🗁 ARP Modes in RouterOS

RouterOS supports different ARP modes on interfaces.

The available modes are:

| Mode | Description |
| --- | --- |
| `enabled` | Normal ARP operation with automatic neighbor discovery |
| `disabled` | ARP is not used on the interface |
| `reply-only` | Only configured static IP-to-MAC mappings are used for neighbor resolution |
| `proxy-arp` | Router responds to ARP requests for reachable destinations through other interfaces |
| `local-proxy-arp` | Router performs proxy ARP for destinations reachable through the same interface |

Inspect an interface's ARP configuration:

```routeros
/interface ethernet print detail
```

Inspect bridge ARP configuration:

```routeros
/interface bridge print detail
```

# 🗁 Enabled Mode

`enabled` is the default ARP mode on supported interfaces.

In this mode:

* RouterOS responds to normal ARP requests for its own addresses.
* It learns neighbor MAC addresses dynamically.
* Dynamic ARP entries are maintained automatically.

Configure an interface:

```routeros
/interface bridge set bridge-LAN arp=enabled
```

This is the normal choice for most LAN and WAN configurations.

# 🗁 Disabled Mode

When `arp=disabled` is configured, the interface does not use ARP.

RouterOS will not answer ARP requests on that interface. Devices communicating through it must have the required neighbor mappings configured through another mechanism, such as static ARP entries on their own operating systems.

Configure:

```routeros
/interface bridge set bridge-LAN arp=disabled
```

This mode is uncommon in ordinary Ethernet networks because normal IPv4 neighbor discovery is unavailable on that interface.

# 🗁 Reply-Only Mode

`reply-only` allows RouterOS to use only manually configured ARP mappings instead of automatically learning dynamic entries.

Configure:

```routeros
/interface bridge set bridge-LAN arp=reply-only
```

Before enabling this mode, add the required static entries:

```routeros
/ip arp add address=192.168.10.10 mac-address=AA:BB:CC:DD:EE:10 interface=bridge-LAN

/ip arp add address=192.168.10.11 mac-address=AA:BB:CC:DD:EE:11 interface=bridge-LAN
```

Verify the mappings:

```routeros
/ip arp print detail
```

Then enable reply-only mode:

```routeros
/interface bridge set bridge-LAN arp=reply-only
```

Important behavior:

* RouterOS does not automatically create dynamic ARP entries on that interface.
* Neighbor resolution depends on valid static ARP mappings.
* A device without a matching mapping may lose IPv4 connectivity through the interface.
* The router can still answer ARP requests for its own addresses.

**Important:** Do not enable `reply-only` on a production LAN before preparing and verifying all required static mappings. Otherwise, existing clients may lose connectivity.

# 🗁 Proxy ARP Mode

Proxy ARP allows a router to answer an ARP request on behalf of another reachable IP address.

Instead of returning the destination host's MAC address, the router returns its own MAC address.

The requesting device then sends the Ethernet frame to the router, which forwards the IP packet toward the actual destination.

Configure:

```routeros
/interface ethernet set ether2 arp=proxy-arp
```

A typical use case is a network where hosts incorrectly believe that remote destinations are on their local subnet because of an oversized subnet mask.

Proxy ARP can allow communication in such designs, but correcting the IP addressing and routing design is generally preferable.

Use proxy ARP only when the network design requires it.

# 🗁 Local Proxy ARP Mode

`local-proxy-arp` allows RouterOS to perform proxy ARP for destinations reachable through the same interface.

In a normal LAN, two devices in the same subnet communicate directly at Layer 2.

With local proxy ARP enabled:

* A client sends an ARP request for another client.
* RouterOS responds using its own interface MAC address.
* The client sends the traffic to the router.
* RouterOS routes the traffic back through the same interface.

Configure:

```routeros
/interface bridge set bridge-LAN arp=local-proxy-arp
```

This mode can be useful with isolated bridge ports or client-isolation designs where hosts should communicate through the router rather than directly through the Layer 2 network.

It must be combined with an appropriate routing, bridge, and firewall design.

# 🗁 Comparing ARP Modes

| Mode | Dynamic Learning | Proxy Behavior | Typical Use |
| --- | --- | --- | --- |
| `enabled` | Yes | No | Normal networking |
| `disabled` | No | No | Special environments without ARP |
| `reply-only` | No | No | Static IP-to-MAC control |
| `proxy-arp` | Normal ARP behavior plus proxy responses | Between interfaces | Specific proxy ARP designs |
| `local-proxy-arp` | Normal ARP behavior plus local proxy responses | Same interface | Isolated LAN clients |

# 🗁 ARP and DHCP Integration

RouterOS DHCP servers can add ARP entries for clients.

This is useful when DHCP-managed devices need to be associated with their assigned IP and MAC addresses.

Inspect DHCP-related ARP entries:

```routeros
/ip arp print where dhcp=yes
```

A common controlled-network design combines:

* DHCP static leases.
* Static ARP mappings.
* `reply-only` mode.

Example static DHCP lease:

```routeros
/ip dhcp-server lease add address=192.168.10.10 mac-address=AA:BB:CC:DD:EE:10 server=dhcp-LAN comment="Office-PC"
```

Add the corresponding static ARP mapping:

```routeros
/ip arp add address=192.168.10.10 mac-address=AA:BB:CC:DD:EE:10 interface=bridge-LAN
```

The DHCP lease and ARP entry should remain consistent.

Do not assume that configuring a DHCP static lease alone automatically guarantees that every required static ARP mapping exists.

# 🗁 ARP Timeout

ARP entries are not necessarily permanent.

RouterOS uses ARP timeout settings to determine how long dynamically learned neighbor information remains valid without receiving packets from the associated IP address.

Inspect IP settings:

```routeros
/ip settings print
```

Inspect interface ARP timeout settings:

```routeros
/interface ethernet print detail
```

Example interface configuration:

```routeros
/interface ethernet set ether2 arp-timeout=1m
```

The `auto` value uses the system-level ARP timeout setting. MikroTik documents a default system ARP timeout of 30 seconds.

Avoid changing ARP timeout values without a specific operational requirement.

# 🗁 Gratuitous ARP

Gratuitous ARP is an ARP message sent by a device to announce or update the relationship between its own IP address and MAC address.

It can be used for:

* Announcing an IP-to-MAC mapping.
* Updating neighbor caches after a MAC address change.
* Supporting certain high-availability and failover designs.
* Detecting possible duplicate IPv4 addresses.

For example, when a device takes over an IP address previously used by another device, a gratuitous ARP announcement can help other devices update their ARP caches.

RouterOS also provides a Traffic Generator method for injecting a gratuitous ARP frame. That method requires constructing the appropriate Ethernet and ARP frame data and should be used only when the exact frame parameters are understood.

# 🗁 ARP Spoofing and Security

ARP itself does not authenticate the sender of an ARP reply.

A malicious device on the same Layer 2 network can send forged ARP messages to associate another device's IP address with an incorrect MAC address.

This is commonly called ARP spoofing or ARP poisoning.

Possible consequences include:

* Man-in-the-middle attacks.
* Traffic interception.
* Network communication disruption.
* Impersonation of the default gateway.
* Denial of service.

Useful defensive practices include:

* Use static ARP entries for critical devices where practical.
* Use `reply-only` on carefully controlled networks when its operational constraints are acceptable.
* Separate clients using VLANs.
* Use switch-level DHCP snooping and Dynamic ARP Inspection where supported.
* Restrict access to network infrastructure.
* Monitor unexpected IP-to-MAC changes.
* Apply appropriate firewall and network access controls.

Static ARP and `reply-only` can reduce certain risks, but they do not replace comprehensive Layer 2 security controls.

# 🗁 Monitoring ARP Traffic

RouterOS provides packet-sniffing tools that can help diagnose ARP problems.

Capture ARP traffic on a LAN bridge:

```routeros
/tool sniffer quick interface=bridge-LAN mac-protocol=arp
```

This can help identify:

* ARP requests without replies.
* Unexpected ARP announcements.
* Duplicate IP address activity.
* Incorrect MAC address responses.
* Repeated ARP resolution attempts.

You can also inspect the ARP table while generating traffic:

```routeros
/ip arp print detail
```

# 🗁 Troubleshooting ARP Problems

**Problem: ARP entry is incomplete**

Possible causes:

* The destination device is offline.
* The destination IP address is incorrect.
* The physical connection is unavailable.
* The devices are in different VLANs.
* The destination device is not responding to ARP requests.

Useful checks:

```routeros
/ip arp print detail
/interface print
/interface bridge port print
```

**Problem: Incorrect MAC address appears**

Possible causes:

* Duplicate IP addresses.
* ARP spoofing.
* A device changed its network adapter.
* A stale neighbor entry.
* Incorrect static ARP configuration.

Check:

```routeros
/ip arp print detail where address=192.168.10.10
```

Verify the actual MAC address on the destination device.

**Problem: Client cannot communicate after enabling reply-only**

Possible causes:

* Missing static ARP entry.
* Incorrect MAC address.
* Wrong interface assignment.
* Client IP address does not match the configured mapping.

Check:

```routeros
/ip arp print detail
/interface bridge print detail
```

Add or correct the static entry before testing again.

**Problem: Devices in the same subnet cannot communicate**

Possible causes:

* VLAN mismatch.
* Bridge port isolation.
* Incorrect subnet mask.
* Firewall restrictions.
* Local proxy ARP or bridge configuration issues.

Check the VLAN and bridge configuration:

```routeros
/interface bridge print detail
/interface bridge port print detail
/interface bridge vlan print detail
```

**Problem: Duplicate IP address detected**

Possible causes:

* Two devices configured with the same static IP.
* A static IP overlaps with a DHCP address pool.
* An incorrect DHCP reservation.
* A device using an unexpected IP configuration.

Check the ARP table and identify which MAC address is associated with the conflicting IP.

# 🗁 Practical Lab: Inspect and Configure ARP

**Lab objective**

Understand dynamic ARP learning, static ARP configuration, and reply-only mode.

**Network topology**

```text
                  MikroTik Router
                  192.168.10.1/24
                         |
                    bridge-LAN
                         |
                    Client PC
                  192.168.10.10/24
```

**Step A: Configure the LAN interface**

Assume the bridge and IP address are already configured:

```routeros
/ip address print
/interface bridge print
```

**Step B: Generate traffic**

From the client PC, ping the router:

```bash
ping 192.168.10.1
```

From MikroTik, ping the client:

```routeros
/ping 192.168.10.10
```

**Step C: Inspect the ARP table**

```routeros
/ip arp print
```

Identify the client's dynamically learned MAC address.

**Step D: Add a static ARP entry**

Use the actual MAC address shown in your ARP table:

```routeros
/ip arp add address=192.168.10.10 mac-address=AA:BB:CC:DD:EE:10 interface=bridge-LAN comment="Lab Client"
```

**Step E: Verify the static entry**

```routeros
/ip arp print detail where address=192.168.10.10
```

**Step F: Enable reply-only mode**

Only perform this step after verifying that every required client has a correct static mapping.

```routeros
/interface bridge set bridge-LAN arp=reply-only
```

**Step G: Test connectivity**

From the client:

```bash
ping 192.168.10.1
```

From MikroTik:

```routeros
/ping 192.168.10.10
```

**Step H: Restore normal ARP behavior**

For the lab, restore the default mode:

```routeros
/interface bridge set bridge-LAN arp=enabled
```

Remove the manually added entry if it is no longer needed:

```routeros
/ip arp remove [find comment="Lab Client"]
```

# 🗁 Best Practices

* Keep `arp=enabled` for ordinary LAN and WAN interfaces unless a specific design requires another mode.
* Use static ARP for important devices when fixed IP-to-MAC mappings are needed.
* Prepare all required mappings before enabling `reply-only`.
* Keep DHCP reservations and static ARP entries consistent.
* Avoid using proxy ARP to compensate for poor IP addressing or routing design.
* Use `local-proxy-arp` only when same-interface proxying is part of the network design.
* Investigate unexpected IP-to-MAC changes.
* Use VLAN segmentation and switch-level security features to reduce Layer 2 attack exposure.
* Document static ARP mappings so they can be maintained and audited.

# 🗁 Official Documentation

* [MikroTik RouterOS — ARP](https://help.mikrotik.com/docs/spaces/ROS/pages/100892687/ARP)
* [MikroTik RouterOS — IPv4 and IPv6 Fundamentals](https://help.mikrotik.com/docs/spaces/ROS/pages/119144661/IPv4%2Band%2BIPv6%2BFundamentals)
* [MikroTik RouterOS — Ethernet](https://help.mikrotik.com/docs/spaces/ROS/pages/8323191/Ethernet)
