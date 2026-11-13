# RADIUS and User Management

RADIUS (Remote Authentication Dial-In User Service) is a networking protocol used to provide centralized Authentication, Authorization, and Accounting (AAA) for users accessing network services.

Internet Service Providers (ISPs) use RADIUS to manage subscriber credentials, control network access, assign service policies, and monitor customer sessions.

Instead of maintaining subscriber accounts separately on every access router, an ISP can use a centralized RADIUS server to manage users across multiple network devices.

RADIUS commonly integrates with:

* PPPoE servers.
* Hotspot gateways.
* Wireless access points and controllers.
* VPN servers.
* Network Access Servers (NAS).
* Broadband Network Gateways (BNG).

# 🗁 Understanding AAA

AAA represents three important functions of network access management.

| Function | Meaning | Responsibility |
| --- | --- | --- |
| Authentication | Who are you? | Verifies user identity |
| Authorization | What can you access? | Determines permitted services and policies |
| Accounting | What did you use? | Records session activity and usage |

**Authentication**

Authentication verifies whether a subscriber's credentials are valid.

For example, an ISP subscriber enters:

```text
Username: customer001
Password: StrongPassword123
```

The access router sends an authentication request to the RADIUS server.

The server validates the credentials and returns an appropriate response.

**Authorization**

Authorization determines what an authenticated subscriber is allowed to use.

An ISP may assign different policies based on the subscriber's package.

For example:

| Subscriber | Package | Download | Upload |
| --- | --- | ---: | ---: |
| customer001 | Basic | 5 Mbps | 5 Mbps |
| customer002 | Standard | 10 Mbps | 10 Mbps |
| customer003 | Premium | 20 Mbps | 20 Mbps |
| customer004 | Business | 50 Mbps | 50 Mbps |

Authorization can also control:

* IP address assignment.
* Session duration.
* Idle timeout.
* Simultaneous sessions.
* VLAN assignment.
* Access restrictions.
* Service-specific permissions.

**Accounting**

Accounting records subscriber session activity.

Typical accounting information includes:

* Username.
* Session start and stop times.
* Assigned IP address.
* NAS identity.
* Session duration.
* Uploaded data.
* Downloaded data.
* Session termination reason.

ISPs use accounting information for usage monitoring, customer support, auditing, and billing systems.

# 🗁 RADIUS Architecture

A typical RADIUS architecture contains three main components:

| Component | Responsibility |
| --- | --- |
| RADIUS Client | Sends authentication and accounting requests |
| RADIUS Server | Processes requests and applies subscriber policies |
| User Database | Stores subscriber credentials and service information |

In an ISP environment, the RADIUS client is commonly a MikroTik router, BNG, or another Network Access Server.

Example architecture:

```text
             Subscribers
                  |
                  |
          PPPoE / Hotspot
                  |
                  |
           Access Network
                  |
                  |
          MikroTik Router
             (NAS)
                  |
                  |
             RADIUS
              Server
                  |
          +-------+-------+
          |               |
          |               |
      User Database   Policy Database
          |               |
          +-------+-------+
                  |
                  |
          Authentication
          Authorization
             Accounting
```

The RADIUS server centralizes subscriber management while the NAS enforces the returned policies.

# 🗁 RADIUS Communication Process

A typical RADIUS authentication process works as follows:

* A subscriber attempts to connect to an ISP service.
* The NAS receives the connection request.
* The NAS creates a RADIUS Access-Request.
* The RADIUS server validates the subscriber credentials.
* The server returns an Access-Accept or Access-Reject response.
* If accepted, the NAS applies the authorized subscriber policies.
* The NAS sends accounting information to the RADIUS server.

Example authentication flow:

```text
Subscriber          NAS             RADIUS Server
    |                |                     |
    | Login Request  |                     |
    |--------------->|                     |
    |                |                     |
    |                |  Access-Request     |
    |                |-------------------->|
    |                |                     |
    |                |  Access-Accept      |
    |                |<--------------------|
    |                |                     |
    | Access Granted |                     |
    |<---------------|                     |
    |                |                     |
    |                | Accounting-Start    |
    |                |-------------------->|
    |                |                     |
    |                | Accounting-Update  |
    |                |-------------------->|
    |                |                     |
    |                | Accounting-Stop     |
    |                |-------------------->|
```

# 🗁 RADIUS Packet Types

RADIUS uses different packet types for authentication, authorization, and accounting operations.

| Packet Type | Code | Purpose |
| --- | ---: | --- |
| Access-Request | 1 | Requests authentication and authorization |
| Access-Accept | 2 | Accepts the authentication request |
| Access-Reject | 3 | Rejects the authentication request |
| Accounting-Request | 4 | Sends accounting information |
| Accounting-Response | 5 | Acknowledges an accounting request |
| Access-Challenge | 11 | Requests additional authentication information |
| Status-Server | 12 | Supports server status checks when implemented |
| Disconnect-Request | 40 | Requests termination of a user session |
| Disconnect-ACK | 41 | Confirms a disconnect request |
| Disconnect-NAK | 42 | Rejects a disconnect request |
| CoA-Request | 43 | Requests modification of an active session |
| CoA-ACK | 44 | Confirms a Change of Authorization request |
| CoA-NAK | 45 | Rejects a Change of Authorization request |

The original RADIUS authentication and accounting packet types are defined in RFC 2865 and RFC 2866. Disconnect and Change of Authorization operations are defined in RFC 5176.

# 🗁 RADIUS Ports and Transport

Traditional RADIUS uses UDP for communication.

| Service | Default Port | Transport |
| --- | ---: | --- |
| Authentication | 1812 | UDP |
| Accounting | 1813 | UDP |
| Legacy Authentication | 1645 | UDP |
| Legacy Accounting | 1646 | UDP |

The standard ports are 1812 and 1813.

Legacy ports may appear in older deployments and should only be used when required by the existing infrastructure.

RADIUS communication should be restricted to trusted network paths and protected according to the security requirements of the deployment.

# 🗁 Important RADIUS Attributes

RADIUS uses attributes to carry subscriber information and network policies.

Common attributes include:

| Attribute | Attribute Number | Purpose |
| --- | ---: | --- |
| User-Name | 1 | Subscriber username |
| User-Password | 2 | Password attribute in supported authentication exchanges |
| NAS-IP-Address | 4 | IP address of the Network Access Server |
| NAS-Port | 5 | NAS port identifier |
| Service-Type | 6 | Type of requested service |
| Framed-IP-Address | 8 | IPv4 address assigned to the subscriber |
| Framed-IP-Netmask | 9 | Subscriber network mask |
| Filter-Id | 11 | Identifies an access filter or policy |
| Session-Timeout | 27 | Maximum session duration |
| Idle-Timeout | 28 | Maximum idle period |
| Calling-Station-Id | 31 | Identifier of the calling station, commonly a MAC address |
| Acct-Status-Type | 40 | Accounting event type |
| Acct-Input-Octets | 42 | Received octet counter |
| Acct-Output-Octets | 43 | Transmitted octet counter |
| Acct-Session-Id | 44 | Session identifier |
| Acct-Session-Time | 46 | Session duration |
| Acct-Input-Gigawords | 52 | High-order input octet counter |
| Acct-Output-Gigawords | 53 | High-order output octet counter |

RADIUS attributes allow the server and NAS to exchange authentication results, subscriber configuration, and accounting information.

# 🗁 Authentication and Authorization Responses

The RADIUS server commonly returns one of three authentication responses.

**Access-Accept**

Indicates that the authentication request was accepted.

The response may include subscriber policy attributes.

**Access-Reject**

Indicates that the authentication request was rejected.

Possible reasons include:

* Incorrect username.
* Incorrect password.
* Disabled subscriber account.
* Expired subscription.
* Access restrictions.
* Invalid authentication conditions.

**Access-Challenge**

Requests additional information from the user or NAS before authentication can be completed.

This can support challenge-response authentication workflows.

# 🗁 RADIUS Accounting

RADIUS accounting records subscriber session activity.

Common accounting events include:

| Event | Description |
| --- | --- |
| Accounting-Start | Subscriber session begins |
| Accounting-Interim-Update | Periodic session information update |
| Accounting-Stop | Subscriber session ends |

A typical accounting record may contain:

```text
Username:          customer001
NAS-IP-Address:    10.20.20.1
Framed-IP-Address: 10.10.10.100
Acct-Session-Id:   00000001
Acct-Session-Time: 3600
Input-Octets:      104857600
Output-Octets:     524288000
Acct-Status-Type:  Stop
```

The values are illustrative.

Accounting data can be used to calculate subscriber usage and support billing systems.

Interim updates are particularly useful when an accounting stop record is not received because of a router failure or unexpected disconnection.

# 🗁 RADIUS for PPPoE

PPPoE servers can use RADIUS to authenticate broadband subscribers.

A typical PPPoE authentication flow is:

```text
Customer Router
       |
       | PPPoE Discovery
       v
PPPoE Access Concentrator
       |
       | PPP Authentication
       v
RADIUS Server
       |
       | Access-Accept
       v
PPPoE Access Concentrator
       |
       | Apply Subscriber Policy
       v
PPP Session Established
```

RADIUS can return attributes that influence subscriber configuration, depending on NAS support.

Examples include:

* Assigned IP address.
* Session timeout.
* Idle timeout.
* Rate-limit policy.
* Address pool or service profile selection.
* Accounting session information.

# 🗁 RADIUS for Hotspot

RADIUS can also authenticate Hotspot users.

A typical workflow is:

* User connects to the Hotspot network.
* DHCP assigns an IP address.
* The captive portal requests subscriber credentials.
* The Hotspot gateway sends a RADIUS Access-Request.
* The RADIUS server validates the account.
* The gateway grants or rejects access.
* Accounting records are sent to the RADIUS server.

RADIUS allows an ISP to use a common subscriber database for both PPPoE and Hotspot services when the policies and service types are configured appropriately.

# 🗁 MikroTik RouterOS RADIUS Configuration

MikroTik RouterOS supports RADIUS integration for services such as PPP and Hotspot.

Example parameters:

| Parameter | Value |
| --- | --- |
| RADIUS Server | `10.20.20.10` |
| MikroTik NAS | `10.20.20.1` |
| Authentication Port | `1812` |
| Accounting Port | `1813` |
| Shared Secret | `RadiusSharedSecret` |

Add a RADIUS server:

```routeros
/radius add \
    service=ppp,hotspot \
    address=10.20.20.10 \
    secret=RadiusSharedSecret \
    authentication-port=1812 \
    accounting-port=1813
```

Enable RADIUS for PPP:

```routeros
/ppp aaa set \
    use-radius=yes \
    accounting=yes \
    interim-update=5m
```

Enable RADIUS for Hotspot:

```routeros
/ip hotspot profile set \
    [find name=hsprof1] \
    use-radius=yes
```

Inspect RADIUS configuration:

```routeros
/radius print detail
```

Monitor the RADIUS server:

```routeros
/radius monitor [find]
```

The RADIUS server must also be configured to recognize the MikroTik router as a client.

The shared secret must match on both sides.

# 🗁 Centralized Subscriber User Management

An ISP may have thousands of subscribers.

Managing each account separately on every router creates operational complexity.

A centralized user-management system provides a single place to manage subscriber information.

A typical subscriber record may contain:

| Field | Example |
| --- | --- |
| Subscriber ID | ISP-10001 |
| Username | customer001 |
| Password | Stored securely |
| Full Name | Customer One |
| Service Type | PPPoE |
| Package | Standard |
| Download Speed | 10 Mbps |
| Upload Speed | 10 Mbps |
| Assigned IP | Dynamic |
| Account Status | Active |
| Expiry Date | Defined by subscription |
| NAS Group | Area-01 |

The subscriber database should be the authoritative source for account status and service entitlement.

The RADIUS server uses this information to determine whether a subscriber can connect and which policies should be applied.

# 🗁 Subscriber Account Lifecycle

An ISP user-management system should support the complete subscriber lifecycle.

**Account Creation**

Create a subscriber account with:

* Unique username.
* Authentication credentials.
* Service package.
* Account validity.
* Access restrictions.

**Account Activation**

Enable the subscriber after registration, payment, or service provisioning.

**Package Upgrade or Downgrade**

Change the subscriber's service profile when the customer changes packages.

**Account Suspension**

Temporarily restrict access because of unpaid bills, policy violations, or administrative requirements.

**Account Renewal**

Extend the subscription validity after payment.

**Account Termination**

Disable the account when the service is permanently cancelled.

These operations should be reflected consistently in the subscriber database and RADIUS authorization policy.

# 🗁 Bandwidth and Service Profiles

A centralized user-management system can assign different service packages to subscribers.

Example:

| Package | Download | Upload | Concurrent Sessions |
| --- | ---: | ---: | ---: |
| Basic | 5 Mbps | 5 Mbps | 1 |
| Standard | 10 Mbps | 10 Mbps | 1 |
| Premium | 20 Mbps | 20 Mbps | 1 |
| Business | 50 Mbps | 50 Mbps | 2 |

The RADIUS server can return supported policy attributes or vendor-specific attributes that the NAS interprets.

For MikroTik deployments, bandwidth policies can be returned using supported MikroTik RADIUS attributes, such as `Mikrotik-Rate-Limit`.

The attribute's direction and rate-limit syntax must follow the MikroTik implementation.

# 🗁 Change of Authorization (CoA)

Change of Authorization allows a RADIUS server to modify the authorization state of an active subscriber session.

For example, an ISP may need to:

* Change a subscriber's bandwidth package.
* Apply a new access policy.
* Update session authorization.
* Disconnect a subscriber.

RADIUS CoA and Disconnect Messages are defined in RFC 5176.

The NAS must support the required functionality, and the CoA client and server must be configured with compatible security settings.

A package change does not automatically affect every active session. The NAS may need a supported CoA operation or session reconnection before the new policy takes effect.

# 🗁 RADIUS Security

RADIUS carries sensitive authentication and authorization information.

Important security practices include:

* Use strong, unique shared secrets for every NAS.
* Restrict RADIUS access to trusted network devices.
* Filter UDP ports 1812 and 1813 at network boundaries.
* Protect management access to RADIUS servers.
* Monitor authentication failures.
* Maintain accurate accounting records.
* Use secure storage for subscriber credentials.
* Protect backups and database exports.
* Use secure network paths between NAS devices and RADIUS servers.
* Consider protected RADIUS transport such as RadSec where supported.

Traditional RADIUS over UDP does not encrypt the entire packet. The shared secret and protocol mechanisms provide limited protections, not full transport confidentiality.

RadSec transports RADIUS over TLS and is specified in RFC 6614.

# 🗁 RADIUS Troubleshooting

| Problem | Possible Cause | Troubleshooting |
| --- | --- | --- |
| Access-Request receives no response | RADIUS server unreachable | Check routing, firewall, and server status |
| Access-Reject received | Invalid credentials or denied policy | Inspect server authentication logs |
| Access-Accept received but access fails | NAS policy or unsupported attributes | Inspect returned attributes and NAS logs |
| Accounting records missing | Accounting disabled or packets blocked | Check UDP 1813 and accounting configuration |
| Subscriber gets the wrong IP | Incorrect address attribute or NAS policy | Verify Framed-IP-Address and local pool configuration |
| Incorrect bandwidth applied | Wrong profile or vendor-specific attribute | Inspect the returned rate-limit attributes |
| Authentication works intermittently | Server overload or network instability | Check server resources, latency, and packet loss |
| Duplicate session problems | Stale sessions or incorrect session policy | Inspect active sessions and accounting records |
| CoA request fails | NAS does not support it or request is rejected | Verify CoA support, client configuration, and logs |
| Shared-secret errors | NAS and server secrets do not match | Verify both configurations |

Useful MikroTik diagnostic commands:

```routeros
/radius print detail

/radius monitor [find]

/ppp active print detail

/ip hotspot active print detail

/log print where topics~"radius"
```

Useful server-side checks include:

* Inspect authentication logs.
* Verify NAS client definitions.
* Confirm shared secrets.
* Check authentication and accounting ports.
* Verify user database availability.
* Inspect returned RADIUS attributes.
* Monitor accounting update intervals.
* Confirm that the NAS can reach the RADIUS server.

# 🗁 Practical Notes

* RADIUS centralizes authentication, authorization, and accounting.
* The NAS acts as the RADIUS client.
* Access-Accept grants access subject to the returned policies.
* Access-Reject denies the request.
* Accounting-Start, Interim-Update, and Accounting-Stop provide session records.
* PPPoE and Hotspot can use the same RADIUS infrastructure.
* Vendor-specific attributes may be required for bandwidth and platform-specific policies.
* CoA can change active session authorization when supported by the NAS.
* Traditional RADIUS does not encrypt the entire packet.
* Centralized user management improves operational consistency across ISP access networks.

# 🗁 References

* [RFC 2865 — Remote Authentication Dial In User Service (RADIUS)](https://www.rfc-editor.org/rfc/rfc2865)
* [RFC 2866 — RADIUS Accounting](https://www.rfc-editor.org/rfc/rfc2866)
* [RFC 5176 — Dynamic Authorization Extensions to RADIUS](https://www.rfc-editor.org/rfc/rfc5176)
* [RFC 6614 — Transport Layer Security (TLS) Encryption for RADIUS](https://www.rfc-editor.org/rfc/rfc6614)
* [RFC 6929 — Remote Authentication Dial-In User Service (RADIUS) Protocol Extensions](https://www.rfc-editor.org/rfc/rfc6929)
* [MikroTik RouterOS — RADIUS](https://help.mikrotik.com/docs/spaces/ROS/pages/328097/RADIUS)
