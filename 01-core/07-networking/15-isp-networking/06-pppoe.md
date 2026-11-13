# PPPoE (Point-to-Point Protocol over Ethernet)

PPPoE (Point-to-Point Protocol over Ethernet) is a network protocol that allows Point-to-Point Protocol (PPP) sessions to operate over Ethernet networks.

Internet Service Providers (ISPs) commonly use PPPoE to establish individual subscriber sessions, authenticate customers, assign IP addresses, enforce service policies, and monitor network usage.

PPPoE combines the session-management and authentication capabilities of PPP with the Ethernet connectivity used in modern access networks.

It is commonly deployed in broadband networks where customer premises equipment (CPE) connects to an ISP's Broadband Network Gateway (BNG) or Broadband Remote Access Server (BRAS).

# 🗁 Why ISPs Use PPPoE

PPPoE provides a structured way to manage individual broadband subscribers.

Common reasons for using PPPoE include:

* **Subscriber authentication:** Verify customers using usernames and passwords.
* **Session management:** Establish and maintain individual subscriber connections.
* **IP address assignment:** Assign addresses dynamically or through predefined subscriber configurations.
* **Bandwidth management:** Apply speed limits and traffic policies to individual subscribers.
* **Usage accounting:** Track session duration, connection status, and data usage.
* **Centralized management:** Integrate subscriber authentication and accounting with RADIUS.
* **Access control:** Control how many sessions a subscriber can establish and which services they can access.

PPPoE is particularly useful for ISPs that need centralized subscriber management across a large access network.

# 🗁 PPPoE Architecture

A typical ISP PPPoE architecture contains the following components:

| Component | Responsibility |
| --- | --- |
| CPE | Customer router or device that initiates the PPPoE connection |
| Access network | Ethernet, switches, OLTs, and aggregation infrastructure |
| PPPoE Access Concentrator (AC) | Receives discovery requests and establishes PPPoE sessions |
| BNG / BRAS | Terminates subscriber sessions and provides broadband network services |
| RADIUS server | Performs centralized authentication, authorization, and accounting |
| IP address pool | Provides subscriber IP addresses |
| Upstream router | Connects subscriber traffic to external networks and the internet |

A simplified connection path looks like this:

```text
Customer Network
       |
       |
  Customer Router
    PPPoE Client
       |
       |
   Access Network
 (Switch / OLT / VLAN)
       |
       |
 Aggregation Network
       |
       |
   BNG / BRAS
  PPPoE Server (AC)
       |
       +------------------+
       |                  |
       |                  |
   RADIUS Server      IP Address Pool
       |
       |
  ISP Core Network
       |
       |
   Upstream Network
       |
       |
    Internet
```

The exact architecture depends on the ISP's network design. The PPPoE Access Concentrator and BNG functions may be implemented on the same device.

# 🗁 PPPoE Components and Roles

**PPPoE Client**

The PPPoE client runs on the customer router or another subscriber device.

Its responsibilities include:

* Discovering an available PPPoE Access Concentrator.
* Requesting a PPPoE session.
* Authenticating with subscriber credentials.
* Negotiating PPP parameters.
* Establishing IP connectivity.

**PPPoE Access Concentrator (AC)**

The Access Concentrator is the network-side component that receives PPPoE discovery requests and establishes sessions with clients.

It typically operates on an ISP access router or BNG.

Its responsibilities include:

* Responding to discovery requests.
* Accepting or rejecting session requests.
* Establishing PPP sessions.
* Authenticating subscribers locally or through RADIUS.
* Applying subscriber-specific policies.

**BNG / BRAS**

A Broadband Network Gateway (BNG), historically also called a Broadband Remote Access Server (BRAS), terminates subscriber access sessions and connects them to the ISP's IP network.

Depending on the platform, it may provide:

* PPPoE termination.
* Subscriber authentication.
* IP address assignment.
* Routing.
* Bandwidth enforcement.
* Accounting and session monitoring.
* Subscriber policy enforcement.

# 🗁 PPPoE Operation

PPPoE operates through two major stages:

* Discovery Stage
* PPPoE Session Stage

After discovery establishes the session, PPP negotiation takes place inside that session.

### 🖆 Discovery Stage

During discovery, the PPPoE client discovers an available Access Concentrator and establishes a session identifier.

The discovery process uses five important packet types:

| Packet | Full Name | Purpose |
| --- | --- | --- |
| PADI | PPPoE Active Discovery Initiation | Client searches for an Access Concentrator |
| PADO | PPPoE Active Discovery Offer | Access Concentrator responds to the client |
| PADR | PPPoE Active Discovery Request | Client requests a session |
| PADS | PPPoE Active Discovery Session-confirmation | Access Concentrator confirms the session |
| PADT | PPPoE Active Discovery Terminate | Terminates an established PPPoE session |

**PADI — Active Discovery Initiation**

The client broadcasts a PADI packet to discover available PPPoE Access Concentrators.

The destination MAC address is normally the Ethernet broadcast address:

```text
FF:FF:FF:FF:FF:FF
```

**PADO — Active Discovery Offer**

An available Access Concentrator responds with a PADO packet.

The response identifies the Access Concentrator and may include service information.

**PADR — Active Discovery Request**

After receiving one or more PADO responses, the client selects an Access Concentrator and sends a PADR packet requesting a PPPoE session.

**PADS — Active Discovery Session-confirmation**

The Access Concentrator accepts the request and sends a PADS packet.

The PADS packet contains the assigned PPPoE Session ID.

At this point, the discovery process is complete and the PPPoE session is established.

**PADT — Active Discovery Terminate**

A PADT packet terminates an existing PPPoE session.

It can be sent by either the client or the Access Concentrator.

### 🖆 Discovery Sequence

```text
PPPoE Client                    Access Concentrator
     |                                  |
     |----------- PADI ---------------->|
     |                                  |
     |<---------- PADO -----------------|
     |                                  |
     |----------- PADR ---------------->|
     |                                  |
     |<---------- PADS -----------------|
     |                                  |
     |       Session Established        |
     |                                  |
     |<======= PPP Session ============>|
     |                                  |
     |----------- PADT ---------------->|
     |                                  |
     |        Session Terminated        |
```

### 🖆 Session Stage

After discovery, the client and Access Concentrator exchange PPP packets through the established PPPoE session.

The session stage carries PPP control and data traffic.

PPP negotiation generally follows these steps:

* Link Control Protocol (LCP) negotiation.
* Authentication using PAP or CHAP, if configured.
* Network Control Protocol (NCP) negotiation.
* IP communication.

# 🗁 PPP Negotiation

PPPoE establishes the Ethernet-based session, but PPP handles link configuration, authentication, and network-layer configuration.

**LCP — Link Control Protocol**

LCP establishes and configures the PPP link.

It negotiates parameters such as:

* Maximum Receive Unit (MRU).
* Authentication protocol.
* Link quality monitoring options.
* PPP link behavior.

LCP also provides mechanisms to terminate and monitor the PPP connection.

**Authentication**

After LCP negotiation, authentication is performed if required by the configured PPP profile.

Common authentication protocols include:

| Protocol | Description |
| --- | --- |
| PAP | Password Authentication Protocol |
| CHAP | Challenge Handshake Authentication Protocol |
| MS-CHAP | Microsoft Challenge Handshake Authentication Protocol variants |

CHAP uses a challenge-response mechanism instead of sending the password directly over the link.

PAP transmits credentials in a less protected form within the PPP exchange. It should only be used where the surrounding access network and security requirements make it acceptable.

**NCP — Network Control Protocol**

NCP negotiates network-layer parameters after the PPP link is established and authentication succeeds.

Common examples include:

| Protocol | Purpose |
| --- | --- |
| IPCP | Configures IPv4 parameters |
| IPv6CP | Configures IPv6 interface parameters |

IPCP commonly negotiates IPv4 address configuration.

IPv6CP configures IPv6-related PPP interface parameters. IPv6 prefix delegation, when required, is handled through additional mechanisms such as DHCPv6 Prefix Delegation.

# 🗁 PPPoE Encapsulation

PPPoE encapsulates PPP frames inside Ethernet frames.

The encapsulation structure is:

```text
+--------------------------+
|      Ethernet Header     |
+--------------------------+
|       PPPoE Header       |
+--------------------------+
|       PPP Protocol       |
+--------------------------+
|       PPP Information    |
+--------------------------+
|       Ethernet FCS       |
+--------------------------+
```

The PPPoE header contains:

| Field | Size | Description |
| --- | ---: | --- |
| Version | 4 bits | PPPoE version |
| Type | 4 bits | PPPoE type |
| Code | 1 byte | Packet type or session-stage code |
| Session ID | 2 bytes | Identifies the PPPoE session |
| Length | 2 bytes | Length of the PPPoE payload |

The PPP protocol field identifies the protocol carried inside PPP.

The PPPoE header is 6 bytes, and the PPP protocol field adds another 2 bytes.

# 🗁 PPPoE Ethernet Types

PPPoE uses two Ethernet EtherTypes:

| EtherType | Hexadecimal | Purpose |
| --- | --- | --- |
| Discovery | `0x8863` | PPPoE discovery packets |
| Session | `0x8864` | PPPoE session packets |

These EtherTypes allow Ethernet devices to distinguish PPPoE discovery traffic from established PPPoE session traffic.

# 🗁 PPPoE Session ID

A PPPoE Session ID identifies an established session between a client and an Access Concentrator.

Important characteristics:

* The Session ID is assigned during the discovery stage.
* It is a 16-bit field.
* It is used in PPPoE session packets.
* It is associated with the client and Access Concentrator MAC addresses.
* The Session ID is meaningful within the context of the corresponding PPPoE connection.

A Session ID is not the same as a subscriber username or IP address.

# 🗁 PPPoE MTU and MSS

One important consideration when deploying PPPoE is Ethernet frame overhead.

Standard Ethernet supports an IP MTU of 1500 bytes.

PPPoE adds:

| Header | Size |
| --- | ---: |
| PPPoE header | 6 bytes |
| PPP protocol field | 2 bytes |
| Total additional overhead | 8 bytes |

Therefore, a common PPPoE IP MTU is:

```text
1500 - 8 = 1492 bytes
```

This is why many PPPoE deployments use an MTU of 1492 instead of 1500.

**MTU**

The Maximum Transmission Unit defines the largest IP packet that can be transmitted without fragmentation at a particular network layer.

If the path cannot support the required packet size, communication may experience fragmentation or packet loss.

**MSS**

The Maximum Segment Size defines the maximum TCP payload size in a TCP segment.

For a typical IPv4 connection using a 1492-byte IP MTU:

```text
1492 - 20 (IPv4 header) - 20 (TCP header) = 1452 bytes
```

For a typical IPv6 connection with a 40-byte IPv6 header and a 20-byte TCP header:

```text
1492 - 40 (IPv6 header) - 20 (TCP header) = 1432 bytes
```

These calculations assume no additional IP or TCP header options.

**MSS Clamping**

MSS clamping adjusts TCP MSS values to avoid sending TCP segments that exceed the supported path MTU.

It can help when path MTU discovery is not working correctly or when traffic crosses networks with smaller MTUs.

MSS clamping should be configured according to the actual network path rather than applied blindly to every connection.

# 🗁 Subscriber Authentication and RADIUS

PPPoE supports subscriber authentication through locally configured credentials or centralized authentication services.

**Local Authentication**

An ISP can configure subscriber usernames and passwords directly on the PPPoE Access Concentrator.

This approach is suitable for small deployments and testing environments.

**RADIUS Authentication**

RADIUS provides centralized Authentication, Authorization, and Accounting (AAA).

A typical authentication flow is:

```text
PPPoE Client
     |
     | Connection Request
     v
PPPoE Access Concentrator
     |
     | Access-Request
     v
 RADIUS Server
     |
     | Access-Accept / Access-Reject
     v
PPPoE Access Concentrator
     |
     | Session Configuration
     v
PPPoE Client
```

RADIUS can provide subscriber attributes such as:

* Username and authentication result.
* Assigned IP address.
* Rate-limit or bandwidth policy.
* Session timeout.
* Idle timeout.
* Simultaneous session limits.
* Accounting information.

The exact attributes and policy behavior depend on the RADIUS server and Access Concentrator implementation.

# 🗁 Subscriber IP Address Assignment

After successful PPP negotiation, the subscriber receives network-layer configuration.

For IPv4, IPCP is commonly used to negotiate the subscriber's IP address.

An ISP may assign addresses through:

* Dynamic IP pools.
* Static IP configuration.
* RADIUS-provided attributes.
* Subscriber-specific profiles.

Example:

```text
Subscriber Username: customer001
Assigned IPv4:      10.10.10.100
Gateway:            PPP interface peer
Session Type:       PPPoE
```

The example address is private and intended for illustration.

For IPv6, the ISP may configure IPv6CP and provide prefixes through mechanisms such as DHCPv6 Prefix Delegation.

# 🗁 VLANs in PPPoE Networks

VLANs are commonly used to organize and isolate subscriber traffic across ISP access networks.

A typical access path may look like:

```text
Customer Router
      |
      |
 Access Switch
      |
      | VLAN 100
      |
 Aggregation Switch
      |
      |
    BNG / BRAS
```

VLANs can be used to:

* Separate residential and business services.
* Isolate different access areas.
* Transport subscriber traffic across aggregation networks.
* Separate management and customer traffic.
* Apply service-specific policies.

A dedicated VLAN for every subscriber is not mandatory. ISPs may use shared VLANs, per-service VLANs, per-area VLANs, or other segmentation designs.

# 🗁 Advantages of PPPoE

* Provides individual subscriber sessions.
* Supports username and password authentication.
* Integrates with RADIUS for centralized AAA.
* Supports subscriber-specific IP address assignment.
* Enables per-subscriber bandwidth policies.
* Provides session accounting and monitoring.
* Works over common Ethernet-based access networks.
* Supports established ISP broadband management workflows.

# 🗁 Limitations of PPPoE

* Adds encapsulation overhead.
* Commonly reduces the usable IP MTU to 1492 bytes.
* Requires careful MTU and MSS planning.
* Creates session-management overhead on the Access Concentrator.
* Requires adequate CPU, memory, and session capacity.
* Can introduce operational complexity in large subscriber networks.
* Requires suitable IPv6 configuration for dual-stack deployments.

PPPoE capacity planning should account for concurrent sessions, authentication request rates, accounting traffic, packet throughput, and hardware forwarding capabilities.

# 🗁 PPPoE vs IPoE

IPoE (IP over Ethernet) is another common broadband access model.

Unlike PPPoE, IPoE carries IP traffic directly over Ethernet without establishing a PPP session.

| Feature | PPPoE | IPoE |
| --- | --- | --- |
| Encapsulation | PPP over Ethernet | IP over Ethernet |
| Subscriber session | PPPoE session | Platform-dependent subscriber session |
| Authentication | PAP, CHAP, or RADIUS integration | Commonly DHCP, Option 82, or RADIUS integration |
| IP assignment | IPCP for IPv4; additional IPv6 mechanisms | Commonly DHCP or static configuration |
| MTU overhead | 8 bytes of PPPoE and PPP overhead | No PPPoE overhead |
| Session management | PPP session-based | Implementation-dependent |
| Bandwidth policies | Subscriber-specific | Subscriber-specific |
| Common deployment | PPPoE broadband | DHCP/IPoE broadband |

Both approaches can provide centralized subscriber management. The choice depends on ISP architecture, access equipment, operational requirements, and subscriber-management capabilities.

# 🗁 Common PPPoE Troubleshooting

PPPoE problems can occur during discovery, authentication, PPP negotiation, or normal session operation.

| Symptom | Possible Cause | Troubleshooting |
| --- | --- | --- |
| No PADO response | No reachable Access Concentrator | Check server status, VLANs, and Ethernet connectivity |
| PADI repeated without response | Discovery packets are not reaching the server | Check switching, VLAN membership, filtering, and interface configuration |
| PADO received but no PADS | Session request rejected or not processed | Check service name, Access Concentrator configuration, and session limits |
| Authentication failure | Incorrect credentials or rejected subscriber | Verify username, password, and RADIUS response |
| LCP negotiation failure | Incompatible PPP parameters | Check authentication and link configuration |
| IPCP failure | IPv4 configuration problem | Check IP pools and address assignment |
| Session disconnects frequently | Link instability, timeout, or server issue | Check physical links, logs, timers, and device resources |
| Websites partially load | MTU or MSS problem | Check path MTU and TCP MSS handling |
| Subscriber receives no internet | Routing, NAT, firewall, or upstream problem | Check subscriber routes and internet connectivity |

**Useful diagnostic checks include:**

* Verify that the customer router is connected to the correct access VLAN.
* Confirm that discovery packets reach the Access Concentrator.
* Check PPPoE server and client status.
* Verify subscriber credentials.
* Inspect RADIUS authentication and accounting logs.
* Check available IP addresses in the subscriber pool.
* Review active session counts and device resource utilization.
* Test MTU and path MTU discovery.
* Verify routing, firewall policies, and upstream connectivity.

# 🗁 Practical Notes

* PPPoE discovery and PPP negotiation are separate processes.
* PADI, PADO, PADR, and PADS establish the PPPoE session.
* LCP, authentication, and NCP configure the PPP connection.
* A successful PPPoE session does not automatically guarantee internet connectivity.
* PPPoE Session IDs identify sessions, not subscribers.
* RADIUS can centralize authentication, authorization, and accounting.
* A standard 1500-byte Ethernet IP MTU commonly results in a 1492-byte PPPoE IP MTU.
* VLAN segmentation and PPPoE session management solve different network-design problems.
* PPPoE is not mandatory for every ISP; IPoE is a widely used alternative.

# 🗁 References

* [RFC 2516 — A Method for Transmitting PPP Over Ethernet (PPPoE)](https://www.rfc-editor.org/rfc/rfc2516)
* [RFC 1661 — The Point-to-Point Protocol (PPP)](https://www.rfc-editor.org/rfc/rfc1661)
* [RFC 1332 — The PPP Internet Protocol Control Protocol (IPCP)](https://www.rfc-editor.org/rfc/rfc1332)
* [RFC 4638 — Accommodating a Maximum Transit Unit / Maximum Receive Unit Greater Than 1492 in PPPoE](https://www.rfc-editor.org/rfc/rfc4638)
* [RFC 2865 — Remote Authentication Dial In User Service (RADIUS)](https://www.rfc-editor.org/rfc/rfc2865)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
