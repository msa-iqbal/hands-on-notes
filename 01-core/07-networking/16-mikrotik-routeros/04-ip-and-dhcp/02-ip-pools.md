# IP Pools in MikroTik RouterOS

An IP pool is a predefined range of IP addresses that RouterOS can dynamically assign to network clients and services.

Instead of manually configuring an IP address for every client, an administrator can create an IP pool and allow RouterOS to allocate addresses automatically.

IP pools are commonly used with:

* DHCP servers.
* PPPoE servers.
* Hotspot services.
* VPN services.
* Point-to-point connections.
* Other RouterOS services that support dynamic address allocation.

IP pools simplify address management, reduce manual configuration, and help prevent accidental duplicate assignments.

# 🗁 Understanding IP Pools

An IP pool defines one or more ranges of IP addresses available for allocation.

For example:

```text
Network:       192.168.10.0/24
Gateway:       192.168.10.1
Network Range: 192.168.10.2 - 192.168.10.254
DHCP Pool:     192.168.10.100 - 192.168.10.200
```

The pool contains 101 addresses that can be dynamically assigned to clients.

The network address, broadcast address, and gateway address are excluded from this pool.

# 🗁 IP Pool Management Menu

RouterOS manages IP pools through:

```routeros
/ip pool
```

Common operations include:

* Creating an IP pool.
* Modifying an existing pool.
* Removing a pool.
* Displaying pool configuration.
* Inspecting pool usage.
* Creating multiple address ranges.

# 🗁 Create an IP Pool

Use the `add` command to create a new pool.

Example:

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.100-192.168.10.200
```

This creates an IP pool named `pool-LAN`.

Configuration details:

| Parameter | Value |
| --- | --- |
| Pool name | pool-LAN |
| Starting address | 192.168.10.100 |
| Ending address | 192.168.10.200 |
| Total addresses | 101 |

The pool is now available for services that support IP pool assignment.

# 🗁 Configure Multiple IP Ranges

A single IP pool can contain multiple non-contiguous address ranges.

This is useful when an administrator wants to allocate addresses from different portions of a network.

Example:

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.100-192.168.10.150,192.168.10.200-192.168.10.250
```

The pool contains two separate ranges:

| Range | Available addresses |
| --- | ---: |
| 192.168.10.100–192.168.10.150 | 51 |
| 192.168.10.200–192.168.10.250 | 51 |
| Total | 102 |

RouterOS can allocate addresses from either range.

# 🗁 Create Multiple IP Pools

Different services or network segments can use separate IP pools.

Example:

```routeros
/ip pool
add name=pool-management ranges=192.168.10.100-192.168.10.150

add name=pool-office ranges=192.168.20.100-192.168.20.200

add name=pool-guest ranges=192.168.30.100-192.168.30.250

add name=pool-vpn ranges=10.10.10.10-10.10.10.100
```

Example allocation plan:

| Pool | Network | Purpose |
| --- | --- | --- |
| pool-management | 192.168.10.0/24 | Management clients |
| pool-office | 192.168.20.0/24 | Office clients |
| pool-guest | 192.168.30.0/24 | Guest clients |
| pool-vpn | 10.10.10.0/24 | VPN clients |

Using separate pools makes address allocation easier to organize and troubleshoot.

# 🗁 Display IP Pools

Display all configured pools:

```routeros
/ip pool print
```

Display detailed information:

```routeros
/ip pool print detail
```

Display a specific pool:

```routeros
/ip pool print where name="pool-LAN"
```

Example output:

```text
Columns: NAME, RANGES
# NAME      RANGES
0 pool-LAN  192.168.10.100-192.168.10.200
```

# 🗁 Modify an Existing IP Pool

Use the `set` command to modify an existing pool.

First, inspect the pool list:

```routeros
/ip pool print
```

Change the address range:

```routeros
/ip pool
set [find name="pool-LAN"] ranges=192.168.10.100-192.168.10.220
```

Add multiple ranges:

```routeros
/ip pool
set [find name="pool-LAN"] ranges=192.168.10.100-192.168.10.150,192.168.10.200-192.168.10.250
```

Verify the changes:

```routeros
/ip pool print detail
```

Changing a pool does not necessarily change addresses that have already been assigned to clients. Existing DHCP leases and service sessions must be considered separately.

# 🗁 Remove an IP Pool

Remove a pool by name:

```routeros
/ip pool
remove [find name="pool-LAN"]
```

Before removing a pool, verify that no DHCP server, PPP profile, Hotspot profile, or other service depends on it.

Inspect configured DHCP servers:

```routeros
/ip dhcp-server print detail
```

Inspect PPP profiles:

```routeros
/ppp profile print detail
```

Removing a pool that is referenced by an active service may disrupt address allocation or cause configuration dependency problems.

# 🗁 IP Pool Allocation and DHCP Server

An IP pool is commonly used by a DHCP server to assign addresses automatically.

Example network:

| Parameter | Value |
| --- | --- |
| Network | 192.168.10.0/24 |
| Gateway | 192.168.10.1 |
| DHCP pool | 192.168.10.100–192.168.10.200 |
| Interface | bridge-LAN |

Create the pool:

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.100-192.168.10.200
```

Configure the DHCP server:

```routeros
/ip dhcp-server
add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN disabled=no
```

Configure the DHCP network:

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=1.1.1.1,8.8.8.8
```

The DHCP server can now allocate addresses from `pool-LAN` to connected clients.

# 🗁 IP Pool Allocation and PPPoE

MikroTik PPP profiles can use IP pools to assign addresses to PPP clients.

For example, an ISP can create a pool for PPPoE subscribers.

Create the pool:

```routeros
/ip pool
add name=pool-pppoe ranges=10.20.0.2-10.20.0.254
```

Create a PPP profile:

```routeros
/ppp profile
add name=profile-pppoe local-address=10.20.0.1 remote-address=pool-pppoe
```

When a PPP session uses this profile:

* `local-address` identifies the router-side address.
* `remote-address` identifies the address assigned to the client.
* The remote address is allocated from `pool-pppoe`.

A PPPoE server or user profile must reference the appropriate PPP profile for this allocation to take effect.

# 🗁 IP Pool Allocation and Hotspot

Hotspot services can use IP pools to allocate addresses to connected clients.

Example:

```routeros
/ip pool
add name=pool-hotspot ranges=192.168.50.10-192.168.50.250
```

A Hotspot server profile can reference the pool:

```routeros
/ip hotspot profile
add name=profile-hotspot hotspot-address=192.168.50.1
```

Configure the Hotspot server to use the pool:

```routeros
/ip hotspot
add name=hotspot1 interface=bridge-HOTSPOT address-pool=pool-hotspot profile=profile-hotspot
```

The Hotspot interface must have an appropriate IP address, and the surrounding network and DHCP configuration must match the design.

# 🗁 IP Pool Allocation and VPN

Some VPN configurations use IP pools to assign addresses to remote clients.

For example, a PPP-based VPN service can use a pool through a PPP profile.

Create the pool:

```routeros
/ip pool
add name=pool-vpn ranges=10.30.0.10-10.30.0.100
```

Create a profile:

```routeros
/ppp profile
add name=profile-vpn local-address=10.30.0.1 remote-address=pool-vpn
```

A compatible PPP-based VPN server can use this profile to assign addresses to remote users.

WireGuard uses its own interface and peer addressing configuration rather than directly allocating peer addresses from a RouterOS IP pool.

# 🗁 IP Pool Usage and Allocation Behavior

RouterOS manages addresses assigned from pools through the services using those pools.

An IP pool is not itself a DHCP server or an address reservation system.

For DHCP, inspect active leases:

```routeros
/ip dhcp-server lease print
```

For PPP sessions, inspect active connections:

```routeros
/ppp active print
```

For Hotspot clients, inspect active sessions:

```routeros
/ip hotspot active print
```

These service-specific tables help identify which clients are using dynamically assigned addresses.

# 🗁 IP Pool and DHCP Lease Relationship

An IP pool defines the available address range, while a DHCP lease records an address assignment to a particular client.

| Feature | IP pool | DHCP lease |
| --- | --- | --- |
| Purpose | Defines available addresses | Records a client assignment |
| Scope | Address allocation range | Individual client |
| Configuration | `/ip pool` | `/ip dhcp-server lease` |
| Static assignment | Not its primary purpose | Supported |
| Dynamic allocation | Supplies addresses | Tracks allocation |

For example, a DHCP server may allocate `192.168.10.105` from `pool-LAN` to a client.

The DHCP lease table records the assigned address and client information.

# 🗁 Address Reservation and Exclusion Planning

An IP pool should not overlap with manually assigned static addresses unless the allocation design explicitly accounts for them.

Example network:

```text
Network:              192.168.10.0/24
Gateway:              192.168.10.1
Infrastructure:       192.168.10.2-192.168.10.49
Reserved devices:     192.168.10.50-192.168.10.99
Dynamic DHCP pool:    192.168.10.100-192.168.10.200
Remaining addresses:  192.168.10.201-192.168.10.254
```

Create the dynamic pool:

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.100-192.168.10.200
```

For DHCP clients that require a fixed address, create a static DHCP lease rather than relying on an address being excluded from the dynamic pool by convention.

# 🗁 IP Pool Next-Pool Property

RouterOS IP pools support a `next-pool` property.

This property allows RouterOS to use another pool when the current pool has no available addresses.

Example:

```routeros
/ip pool
add name=pool-primary ranges=192.168.10.100-192.168.10.150 next-pool=pool-secondary

add name=pool-secondary ranges=192.168.10.151-192.168.10.200
```

In this example:

* RouterOS uses `pool-primary` first.
* When the primary pool is exhausted, the next pool can be used.
* The secondary pool provides additional addresses.

The referenced next pool must exist and must have a valid allocation range.

This feature can help expand an allocation range without changing the service's pool reference.

# 🗁 Troubleshooting IP Pool Problems

**DHCP clients are not receiving IP addresses**

Possible causes:

* The DHCP pool does not exist.
* The DHCP server references the wrong pool.
* The pool has no available addresses.
* The DHCP server is disabled.
* The DHCP server is attached to the wrong interface.
* The client is not reaching the DHCP server.

Check:

```routeros
/ip pool print detail
/ip dhcp-server print detail
/ip dhcp-server lease print
```

**IP pool is exhausted**

Possible causes:

* Too many active clients.
* Pool range is too small.
* Addresses are occupied by existing leases.
* The network has insufficient address capacity.

Check:

```routeros
/ip dhcp-server lease print
/ip pool print detail
```

Expand the pool if the subnet has unused addresses:

```routeros
/ip pool
set [find name="pool-LAN"] ranges=192.168.10.100-192.168.10.240
```

Ensure that the expanded range does not overlap with statically assigned addresses or another allocation pool.

**Duplicate IP address problems**

Possible causes:

* Static addresses overlap with dynamic pool ranges.
* Multiple DHCP servers allocate addresses in the same network.
* A device uses a manually configured address already assigned to another client.
* Address reservations are incorrectly configured.

Review the address plan and DHCP leases:

```routeros
/ip address print
/ip pool print detail
/ip dhcp-server lease print detail
/ip arp print
```

**PPPoE clients are not receiving addresses**

Possible causes:

* The PPP profile references an incorrect pool.
* The pool does not contain available addresses.
* The PPP server uses a different profile.
* The PPP configuration has an incorrect local or remote address.

Check:

```routeros
/ip pool print detail
/ppp profile print detail
/ppp active print detail
```

# 🗁 Practical IP Pool Configuration Lab

**Lab topology**

```text
                  MikroTik Router
                         |
                     bridge-LAN
                         |
                     LAN Switch
                    /          \
                  PC-1          PC-2
```

**Network requirements**

| Parameter | Value |
| --- | --- |
| LAN network | 192.168.10.0/24 |
| Router gateway | 192.168.10.1 |
| DHCP pool | 192.168.10.100–192.168.10.200 |
| DHCP server | dhcp-LAN |
| Interface | bridge-LAN |

**Create the bridge**

```routeros
/interface bridge
add name=bridge-LAN
```

**Add the Ethernet interface**

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
```

**Assign the gateway address**

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

**Create the IP pool**

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.100-192.168.10.200
```

**Create the DHCP server**

```routeros
/ip dhcp-server
add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN disabled=no
```

**Configure the DHCP network**

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=1.1.1.1,8.8.8.8
```

**Verify the configuration**

```routeros
/ip pool print detail
/ip dhcp-server print detail
/ip dhcp-server lease print
```

**Expected results**

* PC-1 receives an address from the configured pool.
* PC-2 receives a different address from the same pool.
* Both clients use `192.168.10.1` as their default gateway.
* DHCP leases appear in the lease table.
* The pool provides addresses within the configured range.

# 🗁 Best Practices

* Use meaningful names for IP pools.
* Maintain a documented IP addressing plan.
* Keep dynamic pools separate from infrastructure address ranges.
* Avoid overlapping pools and static address assignments.
* Use separate pools for different VLANs and services.
* Monitor DHCP leases and PPP active sessions.
* Ensure that pools have enough capacity for expected clients.
* Use static DHCP leases for clients that require predictable addresses.
* Configure `next-pool` only when the additional allocation range is intentional.
* Verify service-to-pool references before modifying or removing pools.
* Review pool utilization when troubleshooting address exhaustion.

# 🗁 Official Documentation

* MikroTik IP Pools: <https://help.mikrotik.com/docs/>
* MikroTik DHCP: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP>
* MikroTik PPP: <https://help.mikrotik.com/docs/>
