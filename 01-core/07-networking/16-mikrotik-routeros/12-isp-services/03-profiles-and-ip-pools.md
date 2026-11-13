# PPP Profiles and IP Pools

PPP profiles and IP pools are essential components of MikroTik RouterOS ISP infrastructure. They help administrators manage subscriber network settings, assign IP addresses, control bandwidth, and apply consistent connection policies.

In a PPPoE environment, profiles define connection parameters, while IP pools provide ranges of addresses that can be assigned dynamically to subscribers.

These features are commonly used with:

* PPPoE servers
* PPPoE clients
* PPTP and L2TP servers
* SSTP and OpenVPN servers
* Other PPP-based services

# 🗁 What Is a PPP Profile?

A PPP profile is a reusable configuration template that defines settings for PPP connections.

Instead of configuring every subscriber individually, an administrator can create profiles for different service packages and assign them to subscriber accounts.

For example:

| Profile | Download | Upload | Service |
| --- | ---: | ---: | --- |
| Basic | 10 Mbps | 5 Mbps | Basic package |
| Standard | 25 Mbps | 10 Mbps | Standard package |
| Premium | 50 Mbps | 25 Mbps | Premium package |
| Business | 100 Mbps | 50 Mbps | Business package |

Each subscriber receives the settings associated with their assigned profile, unless individual account settings override them.

# 🗁 View Existing PPP Profiles

Display all configured PPP profiles:

```routeros
/ppp profile print
```

Display detailed profile configuration:

```routeros
/ppp profile print detail
```

RouterOS includes a default PPP profile, which may be used by PPP services unless another profile is specified.

# 🗁 Create a PPP Profile

Create a profile for a basic subscriber package:

```routeros
/ppp profile
add name=basic-profile \
    local-address=10.10.0.1 \
    remote-address=pppoe-pool \
    rate-limit=5M/10M \
    comment="Basic subscriber package"
```

The profile defines:

| Property | Description |
| --- | --- |
| `name` | Profile name |
| `local-address` | IP address assigned to the router side of the PPP connection |
| `remote-address` | Address or pool used for the subscriber side |
| `rate-limit` | Bandwidth limitation for the PPP connection |
| `comment` | Administrative description |

The `local-address` is the router-side address of the PPP connection. The `remote-address` is assigned to the subscriber.

# 🗁 Understand Local and Remote Addresses

A PPP connection has two endpoint addresses:

* **Local address:** The IP address assigned to the MikroTik router's side of the PPP connection.
* **Remote address:** The IP address assigned to the connected subscriber.

Example:

```text
              MikroTik Router
              PPPoE Server
                   |
            Local Address
              10.10.0.1
                   |
              PPP Session
                   |
            Remote Address
              10.10.0.2
                   |
              Subscriber
```

For a PPP profile, the remote address can be a fixed IP address or an IP pool name.

Using an IP pool allows RouterOS to assign addresses dynamically to multiple subscribers.

# 🗁 What Is an IP Pool?

An IP pool is a configured range of IP addresses that RouterOS can allocate dynamically to network services.

In ISP environments, IP pools are commonly used for:

* PPPoE subscriber address assignment
* DHCP address assignment
* VPN client connections
* Hotspot users
* Other services that require dynamic address allocation

For example, an ISP can reserve the range `10.10.0.10-10.10.0.254` for PPPoE subscribers.

# 🗁 Create an IP Pool

Create a pool for PPPoE subscribers:

```routeros
/ip pool
add name=pppoe-pool ranges=10.10.0.10-10.10.0.254
```

This pool contains addresses that can be dynamically assigned to PPP sessions.

View configured pools:

```routeros
/ip pool print
```

Display detailed pool information:

```routeros
/ip pool print detail
```

# 🗁 Create Multiple IP Pools

An ISP may maintain separate pools for different subscriber groups.

Example:

```routeros
/ip pool
add name=basic-pool ranges=10.10.0.10-10.10.0.99
add name=standard-pool ranges=10.10.0.100-10.10.0.199
add name=premium-pool ranges=10.10.0.200-10.10.0.254
```

Possible allocation strategy:

| Pool | Address range | Subscriber group |
| --- | --- | --- |
| basic-pool | 10.10.0.10–10.10.0.99 | Basic |
| standard-pool | 10.10.0.100–10.10.0.199 | Standard |
| premium-pool | 10.10.0.200–10.10.0.254 | Premium |

Separate pools make address management and subscriber classification easier.

Ensure that the configured ranges do not overlap with other address allocations or infrastructure networks.

# 🗁 Assign an IP Pool to a PPP Profile

Create a profile that uses the PPPoE subscriber pool:

```routeros
/ppp profile
add name=basic-profile \
    local-address=10.10.0.1 \
    remote-address=basic-pool \
    rate-limit=5M/10M
```

When a subscriber connects using this profile, RouterOS assigns an available address from `basic-pool`.

The same pool can be referenced by multiple profiles when they are intended to share an address range.

# 🗁 Configure Bandwidth Limits in PPP Profiles

RouterOS supports bandwidth limitation through the `rate-limit` property of a PPP profile.

Example:

```routeros
/ppp profile
add name=standard-profile \
    local-address=10.10.0.1 \
    remote-address=standard-pool \
    rate-limit=10M/25M
```

The rate-limit format is:

```text
upload/download
```

From the router's perspective:

* The first value limits the subscriber's upload traffic.
* The second value limits the subscriber's download traffic.

For example:

```text
rate-limit=10M/25M
```

This provides:

| Direction | Bandwidth |
| --- | ---: |
| Subscriber upload | 10 Mbps |
| Subscriber download | 25 Mbps |

The actual shaping behavior depends on the generated queue configuration and traffic path.

# 🗁 Create Different Service Profiles

An ISP can create multiple profiles for different subscription packages.

```routeros
/ppp profile
add name=basic-profile \
    local-address=10.10.0.1 \
    remote-address=basic-pool \
    rate-limit=5M/10M

add name=standard-profile \
    local-address=10.10.0.1 \
    remote-address=standard-pool \
    rate-limit=10M/25M

add name=premium-profile \
    local-address=10.10.0.1 \
    remote-address=premium-pool \
    rate-limit=25M/50M

add name=business-profile \
    local-address=10.10.0.1 \
    remote-address=business-pool \
    rate-limit=50M/100M
```

These profiles can be assigned to subscriber accounts according to their purchased packages.

Create the corresponding `business-pool` before assigning it to the business profile.

# 🗁 Assign a Profile to a PPPoE Subscriber

PPP subscriber accounts are configured through `/ppp secret`.

Create a subscriber account and assign a profile:

```routeros
/ppp secret
add name=customer001 \
    password=StrongPassword001 \
    service=pppoe \
    profile=basic-profile
```

The subscriber receives the settings defined by `basic-profile` when the PPPoE session is established.

Assign a different profile to an existing subscriber:

```routeros
/ppp secret
set [find name=customer001] profile=premium-profile
```

The new profile settings generally take effect when the subscriber reconnects. An active session may need to be disconnected before the new settings are applied.

# 🗁 Override Profile Settings for Individual Subscribers

A subscriber account can define certain settings independently of its assigned profile.

For example, assign a fixed remote IP address:

```routeros
/ppp secret
add name=customer002 \
    password=StrongPassword002 \
    service=pppoe \
    profile=standard-profile \
    remote-address=10.10.0.50
```

This subscriber receives the configured fixed remote address instead of an address dynamically selected from the profile's pool.

Individual account settings are useful when a subscriber requires special configuration.

Avoid assigning the same fixed address to multiple active subscribers.

# 🗁 Configure Profile DNS Settings

A PPP profile can provide DNS server addresses to connected subscribers.

Example:

```routeros
/ppp profile
set basic-profile dns-server=1.1.1.1,8.8.8.8
```

You can also define DNS servers while creating a profile:

```routeros
/ppp profile
add name=standard-profile \
    local-address=10.10.0.1 \
    remote-address=standard-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=10M/25M
```

The PPP client receives these DNS settings through the PPP connection.

# 🗁 Configure Session Management Options

PPP profiles provide additional properties for managing subscriber sessions.

For example, configure a profile with a session timeout:

```routeros
/ppp profile
add name=temporary-profile \
    local-address=10.10.0.1 \
    remote-address=basic-pool \
    session-timeout=1d
```

A session timeout limits the duration of a PPP session.

Other useful profile properties include:

| Property | Purpose |
| --- | --- |
| `idle-timeout` | Disconnects an idle PPP session after the configured period |
| `session-timeout` | Limits the total duration of a session |
| `only-one` | Controls whether multiple sessions are allowed for the same account |
| `change-tcp-mss` | Adjusts TCP MSS where needed for PPP traffic |
| `use-encryption` | Controls PPP encryption behavior where supported |
| `on-up` | Executes a configured script when a session connects |
| `on-down` | Executes a configured script when a session disconnects |

Example:

```routeros
/ppp profile
set basic-profile only-one=yes
```

Use session controls according to the ISP's subscriber policy.

# 🗁 Understand the Relationship Between Profiles, Pools, and Secrets

These components work together to establish subscriber connections.

```text
             PPPoE Server
                  |
             PPP Secret
                  |
          Subscriber Account
                  |
             PPP Profile
                  |
          +-------+-------+
          |               |
      IP Pool         Rate Limit
          |
    Remote Address
          |
      PPP Session
          |
       Subscriber
```

Their responsibilities are different:

| Component | Responsibility |
| --- | --- |
| PPP secret | Stores subscriber credentials and account-specific settings |
| PPP profile | Defines reusable connection settings |
| IP pool | Supplies dynamically assigned IP addresses |
| PPPoE server | Accepts and manages incoming PPPoE connections |

A subscriber account selects a profile. The profile can reference an IP pool, and the PPPoE server uses these settings when establishing the session.

# 🗁 Monitor Active PPP Sessions

View active PPP connections:

```routeros
/ppp active print
```

Display detailed session information:

```routeros
/ppp active print detail
```

The active session list helps administrators inspect:

* Connected subscriber usernames
* Assigned remote addresses
* Service types
* Session uptime
* Active connection details

View a specific subscriber:

```routeros
/ppp active print where name=customer001
```

# 🗁 Disconnect a Subscriber

Disconnect an active PPP session:

```routeros
/ppp active
remove [find name=customer001]
```

The subscriber's PPP session is terminated.

If the account remains enabled and the client continues attempting to connect, it may establish another session.

Disable a subscriber account to prevent future authentication:

```routeros
/ppp secret
disable [find name=customer001]
```

Enable it again:

```routeros
/ppp secret
enable [find name=customer001]
```

# 🗁 Troubleshooting Profiles and IP Pools

| Problem | Possible cause | What to check |
| --- | --- | --- |
| Subscriber receives no IP address | Incorrect remote-address setting | Verify the profile and pool |
| PPPoE connection fails | Incorrect credentials or disabled account | Check the PPP secret |
| Subscribers receive unexpected addresses | Incorrect pool assignment | Inspect profile and secret overrides |
| Two subscribers use the same fixed IP | Duplicate static assignment | Review individual remote addresses |
| Bandwidth limit is not applied | Incorrect profile or rate-limit | Inspect the active session and generated queues |
| Subscriber receives incorrect DNS | Incorrect profile DNS settings | Check `dns-server` |
| New profile settings do not apply | Existing session still active | Disconnect and reconnect the subscriber |
| Pool runs out of addresses | Insufficient address range | Inspect pool capacity and active sessions |

Inspect the configured profiles:

```routeros
/ppp profile print detail
```

Inspect IP pools:

```routeros
/ip pool print detail
```

Inspect subscriber accounts:

```routeros
/ppp secret print
```

Inspect active sessions:

```routeros
/ppp active print detail
```

Inspect generated queues when bandwidth limitations are involved:

```routeros
/queue simple print detail
```

# 🗁 Best Practices for ISP Networks

* Create separate profiles for different service packages.
* Use clearly named IP pools for subscriber groups.
* Keep subscriber address ranges separate from infrastructure networks.
* Avoid overlapping IP pools.
* Use fixed remote addresses only when required.
* Apply consistent bandwidth policies through profiles.
* Use individual account overrides only for special subscriber requirements.
* Monitor pool utilization and active PPP sessions.
* Document profile names, address ranges, and service limits.
* Back up configurations before making major changes.
* Restrict access to subscriber credentials and administrative configuration.

# 🗁 Practical Lab

Build a small ISP subscriber management environment using MikroTik RouterOS v7.

**Topology**

```text
                ISP Router
               PPPoE Server
                    |
                 ether2
                    |
                 ether1
              Customer Router
                    |
               PPPoE Client
                    |
                LAN Bridge
                    |
              LAN Computer
```

**Configuration requirements**

Create three IP pools:

| Pool | Address range |
| --- | --- |
| basic-pool | 10.10.0.10-10.10.0.99 |
| standard-pool | 10.10.0.100-10.10.0.199 |
| premium-pool | 10.10.0.200-10.10.0.254 |

Create three PPP profiles:

| Profile | Pool | Upload | Download |
| --- | --- | ---: | ---: |
| basic-profile | basic-pool | 5 Mbps | 10 Mbps |
| standard-profile | standard-pool | 10 Mbps | 25 Mbps |
| premium-profile | premium-pool | 25 Mbps | 50 Mbps |

Create subscriber accounts:

| Username | Profile |
| --- | --- |
| customer001 | basic-profile |
| customer002 | standard-profile |
| customer003 | premium-profile |

**Verification tasks**

* Verify that all three IP pools exist.
* Verify that each PPP profile references the correct pool.
* Connect subscribers using their respective credentials.
* Confirm that each subscriber receives an address from the correct pool.
* Verify upload and download limitations.
* Inspect active PPP sessions.
* Change one subscriber's profile.
* Disconnect and reconnect that subscriber.
* Confirm that the new profile settings are applied.
* Disable and re-enable a subscriber account.

# 🗁 Key Takeaways

* PPP profiles provide reusable configuration templates for PPP connections.
* IP pools provide dynamic address allocation.
* The local address represents the router side of a PPP connection.
* The remote address represents the subscriber side.
* PPP profiles can define address assignment, DNS, bandwidth limits, and session behavior.
* PPP secrets store subscriber credentials and can override selected profile settings.
* Multiple service packages can be managed through separate profiles and pools.
* Active PPP sessions can be monitored and disconnected through `/ppp active`.
* Proper profile and pool design makes ISP subscriber management more consistent and scalable.
