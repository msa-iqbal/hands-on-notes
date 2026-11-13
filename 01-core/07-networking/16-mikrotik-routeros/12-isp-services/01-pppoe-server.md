# PPPoE Server

PPPoE (Point-to-Point Protocol over Ethernet) is a network protocol that allows an ISP or network administrator to provide authenticated internet access over an Ethernet-based network.

It combines PPP authentication and session management with Ethernet connectivity.

MikroTik RouterOS supports PPPoE server functionality, allowing a router to authenticate users, assign IP addresses, apply bandwidth limits, and manage subscriber sessions.

PPPoE is widely used by ISPs to provide broadband internet access to residential and business customers.

# 🗁 What Is PPPoE?

PPPoE stands for Point-to-Point Protocol over Ethernet.

It creates a logical point-to-point connection between a PPPoE client and a PPPoE server over an Ethernet network.

The client establishes a session with the server, authenticates using configured credentials, and receives network configuration.

A typical ISP deployment looks like this:

```text
                  Internet
                     |
                +----------+
                |  Router  |
                | MikroTik |
                +----------+
                     |
                ISP Network
                     |
              +--------------+
              | PPPoE Server |
              +--------------+
                     |
                Ethernet LAN
                     |
          +----------+----------+
          |          |          |
       Client A   Client B   Client C
       PPPoE      PPPoE      PPPoE
       Client     Client     Client
```

Each subscriber establishes an independent PPPoE session with the server.

# 🗁 How PPPoE Works

PPPoE connection establishment involves two main phases.

**Discovery phase**

The client discovers an available PPPoE server and negotiates a session.

The discovery process uses these messages:

| Message | Purpose |
| --- | --- |
| PADI | PPPoE Active Discovery Initiation |
| PADO | PPPoE Active Discovery Offer |
| PADR | PPPoE Active Discovery Request |
| PADS | PPPoE Active Discovery Session-confirmation |
| PADT | PPPoE Active Discovery Termination |

**Session phase**

After discovery, the client and server establish a PPP session.

The session typically includes:

* Link configuration using LCP.
* User authentication using PAP or CHAP.
* Network-layer configuration using IPCP for IPv4.
* IP address assignment.
* Data transmission through the established session.

When the connection is terminated, the PPPoE session is closed.

# 🗁 PPPoE Server Requirements

Before configuring a PPPoE server, prepare the following:

* A MikroTik router running RouterOS.
* An Ethernet interface or bridge for subscriber connections.
* A configured WAN connection with internet access.
* A local IP address pool for PPPoE subscribers.
* A PPP profile.
* Subscriber usernames and passwords.
* Appropriate firewall and NAT configuration.

Example network plan:

| Component | Configuration |
| --- | --- |
| WAN interface | ether1 |
| Subscriber interface | ether2 |
| Subscriber network | PPPoE sessions |
| PPP local address | 10.10.10.1 |
| PPP address pool | 10.10.10.100-10.10.10.200 |
| Authentication | CHAP and PAP |
| DNS server | 1.1.1.1, 8.8.8.8 |

The example assumes that `ether2` is dedicated to PPPoE subscribers. If subscribers connect through a bridge or VLAN, use that logical interface instead.

# 🗁 Prepare the Subscriber Interface

The PPPoE server should listen on the interface where subscriber devices connect.

Inspect available interfaces:

```routeros
/interface print
```

If the subscriber network uses a dedicated Ethernet interface, no bridge is required.

If multiple physical ports belong to the same subscriber network, create a bridge and add the appropriate ports:

```routeros
/interface bridge
add name=bridge-subscribers

/interface bridge port
add bridge=bridge-subscribers interface=ether2
add bridge=bridge-subscribers interface=ether3
```

Use the bridge as the PPPoE server interface when subscribers connect through it.

Do not assign ordinary LAN gateway addresses to the subscriber access interface unless your design specifically requires them.

# 🗁 Create an IP Pool

An IP pool defines the addresses available for PPPoE clients.

Create a pool:

```routeros
/ip pool
add name=pppoe-pool ranges=10.10.10.100-10.10.10.200
```

The pool contains addresses that can be assigned to authenticated subscribers.

Inspect the pool:

```routeros
/ip pool print detail
```

Avoid overlapping the pool with other networks or manually assigned addresses.

# 🗁 Create a PPP Profile

A PPP profile defines common settings applied to authenticated PPP sessions.

Create a profile:

```routeros
/ppp profile
add name=pppoe-profile \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    change-tcp-mss=yes
```

Important profile properties:

| Property | Description |
| --- | --- |
| `name` | Profile name |
| `local-address` | IP address used by the server side of the PPP connection |
| `remote-address` | Pool or address assigned to the subscriber |
| `dns-server` | DNS servers provided to clients |
| `change-tcp-mss` | Adjusts TCP MSS when appropriate |
| `rate-limit` | Defines upload and download rate limits |
| `only-one` | Controls whether a user may have multiple simultaneous sessions |

The `local-address` and `remote-address` values form the point-to-point addressing for each PPP session.

# 🗁 Configure the PPPoE Server

Create the PPPoE server on the subscriber-facing interface.

For a dedicated Ethernet interface:

```routeros
/interface pppoe-server server
add interface=ether2 \
    service-name=ISP-PPPoE \
    default-profile=pppoe-profile \
    authentication=chap,mschap2 \
    one-session-per-host=yes \
    disabled=no
```

If subscribers connect through the bridge created earlier, use:

```routeros
/interface pppoe-server server
add interface=bridge-subscribers \
    service-name=ISP-PPPoE \
    default-profile=pppoe-profile \
    authentication=chap,mschap2 \
    one-session-per-host=yes \
    disabled=no
```

Important settings:

| Property | Description |
| --- | --- |
| `interface` | Interface where the server listens |
| `service-name` | PPPoE service identifier |
| `default-profile` | Default PPP profile |
| `authentication` | Permitted authentication methods |
| `one-session-per-host` | Restricts multiple sessions from the same host |
| `disabled` | Enables or disables the server |

The service name can help clients identify the intended PPPoE service. It is not a replacement for authentication.

# 🗁 Create PPPoE Subscriber Accounts

Subscriber accounts are created under `/ppp secret`.

Create a subscriber:

```routeros
/ppp secret
add name=customer001 \
    password=StrongPassword001 \
    service=pppoe \
    profile=pppoe-profile
```

Create another subscriber:

```routeros
/ppp secret
add name=customer002 \
    password=StrongPassword002 \
    service=pppoe \
    profile=pppoe-profile
```

You can assign a specific IP address to a subscriber:

```routeros
/ppp secret
add name=customer003 \
    password=StrongPassword003 \
    service=pppoe \
    profile=pppoe-profile \
    remote-address=10.10.10.150
```

A subscriber-specific `remote-address` overrides the address supplied by the profile.

Important subscriber properties:

| Property | Description |
| --- | --- |
| `name` | Subscriber username |
| `password` | Authentication password |
| `service` | Service type, such as PPPoE |
| `profile` | Assigned PPP profile |
| `local-address` | Optional subscriber-specific local address |
| `remote-address` | Optional subscriber-specific client address |
| `disabled` | Enables or disables the account |

Use unique credentials for every subscriber.

# 🗁 Configure Bandwidth Limits

You can apply bandwidth limits through a PPP profile or an individual subscriber account.

To apply a shared speed limit to all subscribers using a profile:

```routeros
/ppp profile
set pppoe-profile rate-limit=10M/20M
```

The rate-limit format is:

```text
upload/download
```

Therefore, `10M/20M` represents:

* Upload: 10 Mbps.
* Download: 20 Mbps.

For an individual subscriber, configure a different profile:

```routeros
/ppp profile
add name=pppoe-20m \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    rate-limit=20M/50M \
    dns-server=1.1.1.1,8.8.8.8
```

Assign the profile to a subscriber:

```routeros
/ppp secret
set [find name=customer001] profile=pppoe-20m
```

The effective rate limit depends on the subscriber's assigned profile and any applicable individual settings.

# 🗁 Configure Firewall and NAT

PPPoE subscribers generally need permission to access the internet through the router.

Assuming the WAN interface belongs to an interface list named `WAN`, configure source NAT:

```routeros
/ip firewall nat
add chain=srcnat \
    out-interface-list=WAN \
    action=masquerade \
    comment="NAT for internet access"
```

If your router already has a suitable masquerade rule, do not add a duplicate unnecessarily.

Review the forward-chain firewall policy to ensure that authenticated subscriber traffic can reach the internet while unwanted traffic remains restricted.

For example, a basic policy can allow established connections and new connections from the subscriber interface toward the WAN:

```routeros
/ip firewall filter
add chain=forward \
    action=accept \
    connection-state=established,related \
    comment="Allow established and related traffic"

add chain=forward \
    action=accept \
    in-interface=ether2 \
    out-interface-list=WAN \
    connection-state=new \
    comment="Allow PPPoE subscribers to WAN"
```

If the PPPoE server listens on a bridge or VLAN, replace `ether2` with the correct interface or use suitable PPP interface matching.

Place these rules before conflicting drop rules and retain the rest of your firewall policy.

# 🗁 Configure MTU and MRU

PPPoE introduces additional encapsulation overhead.

Standard Ethernet has an MTU of 1500 bytes. Traditional PPPoE commonly uses an IP MTU of 1492 bytes because PPPoE and PPP headers consume 8 bytes.

Common PPPoE values:

| Property | Typical value |
| --- | --- |
| Ethernet MTU | 1500 |
| PPPoE overhead | 8 bytes |
| PPPoE IP MTU | 1492 |
| PPPoE MRU | 1492 |

Inspect the PPPoE server configuration:

```routeros
/interface pppoe-server server
print detail
```

MTU and MRU settings should be consistent with the capabilities of the access network and subscriber devices.

Some networks support larger frames through baby jumbo frames, but this requires end-to-end support.

# 🗁 Monitor Active PPPoE Sessions

View active PPP sessions:

```routeros
/ppp active print
```

Display detailed session information:

```routeros
/ppp active print detail
```

View configured subscriber accounts:

```routeros
/ppp secret print
```

Inspect the PPPoE server:

```routeros
/interface pppoe-server server print detail
```

Monitor interface traffic:

```routeros
/interface monitor-traffic ether2
```

Check active connections and assigned addresses to identify subscriber connectivity problems.

# 🗁 Disconnect a Subscriber

To disconnect a specific active PPP session, identify its active session entry:

```routeros
/ppp active print
```

Remove the session using its username:

```routeros
/ppp active remove [find name=customer001]
```

The subscriber can reconnect if the account remains enabled and the client continues attempting to establish a session.

# 🗁 Disable or Enable a Subscriber Account

Disable an account:

```routeros
/ppp secret
disable [find name=customer001]
```

Enable it again:

```routeros
/ppp secret
enable [find name=customer001]
```

Disabling the account prevents new authentication using that secret. Disconnect an existing active session separately if immediate termination is required.

# 🗁 Troubleshooting PPPoE Server

| Problem | Possible cause | What to check |
| --- | --- | --- |
| Client cannot discover server | Wrong server interface | PPPoE server interface and subscriber connectivity |
| No PADO response | Server disabled or discovery traffic blocked | Server status, bridge, VLAN, and Layer 2 path |
| Authentication failure | Incorrect credentials | PPP secret username and password |
| Client authenticates but gets no IP | Incorrect profile or pool | Local address, remote address, and pool |
| Client connects but has no internet | NAT or routing problem | Default route, NAT, and forward-chain rules |
| Only some subscribers connect | Pool exhaustion or account restrictions | Available pool addresses and session settings |
| Slow browsing or some sites fail | MTU or MSS issue | PPPoE MTU, MRU, and path MTU |
| Subscriber cannot reconnect | Existing session or account disabled | Active sessions and PPP secret status |

Enable PPP-related logging for troubleshooting:

```routeros
/system logging
add topics=ppp,debug action=memory
```

Inspect the log:

```routeros
/log print where topics~"ppp"
```

Remove or disable verbose debugging after troubleshooting to avoid unnecessary log volume.

# 🗁 Security Best Practices

* Use strong and unique subscriber passwords.
* Prefer CHAP or MS-CHAPv2 where supported by the client environment.
* Avoid PAP when stronger authentication methods are available.
* Restrict access to router management services.
* Keep RouterOS updated.
* Use firewall rules to isolate subscriber traffic appropriately.
* Monitor active sessions and account usage.
* Avoid exposing subscriber management interfaces to the public internet.
* Use RADIUS for centralized authentication and accounting in larger ISP deployments.
* Keep regular backups of PPP secrets and server configuration.

# 🗁 Practical Lab

Build a small PPPoE ISP environment using two MikroTik routers or one MikroTik router and a PPPoE client.

**Topology**

```text
             Internet / WAN
                   |
             +-----------+
             | Router A  |
             | MikroTik  |
             | PPPoE     |
             | Server    |
             +-----------+
                   |
              ether2
                   |
             +-----------+
             | Router B  |
             | PPPoE     |
             | Client    |
             +-----------+
                   |
                LAN PC
```

**Router A configuration requirements**

* Configure WAN internet access.
* Create a subscriber-facing interface.
* Create an IP pool.
* Create a PPP profile.
* Configure the PPPoE server.
* Create a subscriber account.
* Configure NAT and firewall rules.

**Router B configuration requirements**

* Configure a PPPoE client interface.
* Connect it to Router A's subscriber-facing interface.
* Enter the subscriber username and password.
* Verify that the client receives an IP address.
* Test internet connectivity.

**Verification tasks**

* Confirm that the PPPoE session becomes active.
* Verify the assigned IP address.
* Check the active session on Router A.
* Test connectivity to the internet.
* Apply a bandwidth limit.
* Disable the subscriber account and test authentication failure.
* Re-enable the account and verify reconnection.

# 🗁 Key Takeaways

* PPPoE provides authenticated point-to-point connectivity over Ethernet.
* PPPoE uses discovery and session phases to establish connections.
* RouterOS provides PPPoE server functionality through `/interface pppoe-server server`.
* IP pools define the available subscriber addresses.
* PPP profiles define shared session settings.
* PPP secrets store local subscriber credentials.
* Bandwidth limits can be applied through profiles or subscriber-specific configuration.
* NAT and routing are required for internet access when using private subscriber addresses.
* PPPoE commonly uses an MTU of 1492 bytes over standard 1500-byte Ethernet.
* Active subscriber sessions can be monitored and disconnected through `/ppp active`.
