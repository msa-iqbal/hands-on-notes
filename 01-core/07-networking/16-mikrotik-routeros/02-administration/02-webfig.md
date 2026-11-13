# WebFig

WebFig is the web-based graphical configuration interface for MikroTik RouterOS. It allows administrators to manage routers through a web browser without installing WinBox or using a command-line terminal.

WebFig provides access to many RouterOS configuration features, including interfaces, IP addressing, DHCP, firewall, routing, wireless, queues, monitoring, and system administration.

It is particularly useful when managing routers from devices where WinBox is unavailable or when browser-based administration is preferred.

# 🗁 What Is WebFig?

WebFig is an administration interface built into RouterOS. It runs as a web service on the MikroTik router and provides a graphical interface for configuring and monitoring the device.

Administrators can use WebFig to:

* View router status and resource information.
* Configure network interfaces and bridges.
* Manage IP addresses and routes.
* Configure DHCP servers and clients.
* Create and manage firewall rules.
* Configure wireless interfaces where supported.
* Monitor traffic and system resources.
* Manage RouterOS users and services.
* Access the RouterOS terminal through the browser where supported.

WebFig does not require a separate application installation. A compatible web browser and network connectivity to the router are sufficient.

# 🗁 WebFig vs WinBox vs CLI

RouterOS provides several administration methods. Each has different strengths.

| Feature | WebFig | WinBox | CLI |
| --- | --- | --- | --- |
| Browser-based access | Yes | No | No |
| Graphical interface | Yes | Yes | No |
| Command execution | Terminal feature | Terminal feature | Native |
| Installation required | No | Yes | Depends on access method |
| Remote administration | Yes | Yes | Yes |
| Automation | Limited | Limited | Strong |
| Script execution | Through terminal | Through terminal | Native |
| Configuration visibility | Graphical | Graphical | Command-based |

**When to use WebFig:**

* When WinBox is not installed.
* When administering a router from another operating system.
* When a browser is the preferred management interface.
* When performing routine configuration through a graphical interface.

**When to use CLI:**

* When managing multiple routers.
* When automating configuration.
* When troubleshooting complex network problems.
* When applying repeatable configurations through scripts.

# 🗁 Accessing WebFig

WebFig is accessed through the router's IP address or a hostname that resolves to the router.

For example, if the router has the management IP address `192.168.88.1`, open the following address in a browser:

```text
http://192.168.88.1
```

For HTTPS access:

```text
https://192.168.88.1
```

The default RouterOS web services are:

| Service | Default Port | Protocol |
| --- | ---: | --- |
| www | 80 | HTTP |
| www-ssl | 443 | HTTPS |

The ports can be changed through RouterOS service configuration.

**Important:** HTTPS is preferable for administration because it encrypts communication between the browser and router. HTTP does not provide transport encryption.

# 🗁 Requirements for Access

Before accessing WebFig, ensure that:

* The router is powered on and running RouterOS.
* Your computer can reach the router's management IP address.
* The WebFig service is enabled.
* Firewall rules permit access to the web service.
* You have valid RouterOS login credentials.
* Your browser supports the connection and TLS configuration.

You can verify IP connectivity from Linux:

```bash
ping -c 4 192.168.88.1
```

You can also check whether the web service is reachable:

```bash
curl -I http://192.168.88.1
```

For HTTPS:

```bash
curl -k -I https://192.168.88.1
```

The `-k` option allows curl to connect without validating the server certificate. It is useful for testing a self-signed certificate, but it should not be treated as certificate validation.

# 🗁 Logging In to WebFig

After opening the router's address:

* The WebFig login page appears.
* Enter your RouterOS username.
* Enter the corresponding password.
* Submit the login form.

After successful authentication, the WebFig interface loads.

The available configuration menus depend on the RouterOS version, installed packages, hardware capabilities, and user permissions.

For a new router, the default management address is commonly `192.168.88.1`, but this can vary depending on the device and its initial configuration.

# 🗁 Understanding the WebFig Interface

WebFig organizes RouterOS configuration into menus and configuration pages.

Common sections include:

| Section | Purpose |
| --- | --- |
| Quick Set | Basic router configuration |
| Interfaces | Ethernet, wireless, VLAN, bridge, and other interfaces |
| Bridge | Layer 2 bridging and bridge ports |
| IP | IP addressing, DHCP, DNS, firewall, and routing-related configuration |
| IPv6 | IPv6 addressing and routing configuration |
| Routing | Routing tables, protocols, and route management |
| Wireless | Wireless configuration where supported |
| Queues | Bandwidth management and QoS |
| System | Identity, users, packages, services, clock, and administration |
| Tools | Network utilities and diagnostic tools |
| Files | Router file management |
| Terminal | RouterOS command-line access |

The exact menu organization can change between RouterOS releases and device configurations.

# 🗁 Working with Configuration Pages

Most WebFig configuration pages follow a similar workflow.

A typical workflow includes:

* Open the relevant menu.
* Select an existing configuration entry or create a new one.
* Enter the required configuration values.
* Apply or save the changes.
* Verify that the configuration behaves as expected.

Common actions include:

| Action | Purpose |
| --- | --- |
| Add | Create a new configuration entry |
| Edit | Modify an existing entry |
| Remove | Delete an entry |
| Enable | Activate a disabled entry |
| Disable | Deactivate an active entry |
| Apply | Apply changes while keeping the editing window open |
| OK | Save changes and close the editing window |
| Cancel | Close without applying pending changes |

The exact buttons and labels depend on the configuration page.

**Important:** Applying a configuration change does not guarantee that the resulting network behavior is correct. Verify the relevant interfaces, routes, firewall rules, and connectivity after making changes.

# 🗁 Configuring Interfaces Through WebFig

Interfaces are the foundation of RouterOS networking. WebFig provides graphical access to interface configuration.

To inspect interfaces:

* Open **Interfaces**.
* Review the available Ethernet, wireless, bridge, VLAN, and other interfaces.
* Select an interface to inspect or modify its properties.

Common interface properties include:

* Interface name.
* MAC address.
* MTU.
* Running status.
* Disabled status.
* Link status and traffic statistics.

You can also rename interfaces to make their purpose easier to understand.

For example:

```text
ether1 → WAN
ether2 → LAN
ether3 → OFFICE
ether4 → SERVER
```

Always verify that the selected physical interface corresponds to the intended network connection before modifying it.

# 🗁 Configuring IP Addresses Through WebFig

WebFig provides a graphical method for assigning IP addresses to router interfaces.

For example, to configure a LAN address:

* Open **IP → Addresses**.
* Select **Add New**.
* Enter the address `192.168.10.1/24`.
* Select the LAN interface.
* Apply the configuration.

The resulting configuration is conceptually equivalent to:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

To verify the configuration through the terminal:

```routeros
/ip address print
```

An IP address must be assigned to the correct interface. Incorrect interface selection can cause connectivity problems or unexpected network behavior.

# 🗁 Configuring DHCP Through WebFig

WebFig can be used to configure DHCP clients, DHCP servers, address pools, and network parameters.

A typical LAN DHCP server configuration involves:

* Assigning an IP address to the LAN interface.
* Creating an IP address pool.
* Configuring the DHCP network.
* Creating and enabling the DHCP server.
* Verifying that clients receive valid network settings.

Example network configuration:

| Parameter | Value |
| --- | --- |
| Router IP | `192.168.10.1` |
| Subnet | `192.168.10.0/24` |
| Address Pool | `192.168.10.100-192.168.10.200` |
| Gateway | `192.168.10.1` |
| DNS Server | `192.168.10.1` |

WebFig provides graphical fields for these settings. The equivalent RouterOS commands can be used to verify the resulting configuration.

```routeros
/ip pool print
/ip dhcp-server print
/ip dhcp-server network print
/ip dhcp-server lease print
```

# 🗁 Configuring Firewall Through WebFig

WebFig provides access to RouterOS firewall configuration.

Open **IP → Firewall** to manage firewall components such as:

* Filter rules.
* NAT rules.
* Mangle rules.
* Address lists.
* Connection tracking settings.
* Raw rules.

Firewall rules are processed according to their respective chains and rule order.

For example, the `input` chain controls traffic destined for the router itself, including management access.

The `forward` chain controls traffic passing through the router.

The `output` chain controls traffic generated by the router.

Before changing firewall rules through WebFig, understand their order and effect. An incorrectly configured `input` rule can block your own management connection.

# 🗁 Configuring Routing Through WebFig

WebFig provides graphical access to RouterOS routing configuration.

Depending on the RouterOS version and installed features, you can manage:

* Connected routes.
* Static routes.
* Default routes.
* Routing tables.
* Routing rules.
* OSPF.
* BGP.
* Other supported routing features.

For example, a static default route can be created through the routing configuration pages.

The equivalent RouterOS command is:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1
```

Verify the resulting routes:

```routeros
/ip route print
```

When configuring routing remotely, ensure that the management path remains reachable.

# 🗁 Using the WebFig Terminal

WebFig includes a terminal feature that provides access to the RouterOS command-line interface through the browser.

The terminal is useful when:

* A particular configuration option is easier to manage through CLI.
* You need to execute diagnostic commands.
* You want to inspect configuration details.
* You need to run RouterOS scripts or commands.

Example commands:

```routeros
/system resource print
/system identity print
/interface print
/ip address print
/ip route print
```

The terminal provides the same RouterOS command environment used through other CLI access methods, subject to the logged-in user's permissions.

# 🗁 Managing Files Through WebFig

WebFig provides access to the router's file storage through its file management interface.

Depending on RouterOS version and permissions, administrators can use it to:

* View stored files.
* Upload files.
* Download files.
* Remove unnecessary files.
* Manage backup and export files.

Common file types include:

| File Type | Purpose |
| --- | --- |
| `.backup` | Binary RouterOS configuration backup |
| `.rsc` | RouterOS configuration export or script |
| `.npk` | RouterOS package file |
| `.pem` | Certificate or private-key-related file |
| `.crt` | Certificate file |

Example CLI commands for inspecting files:

```routeros
/file print
```

Creating a binary backup:

```routeros
/system backup save name=before-webfig-changes
```

Exporting the configuration:

```routeros
/export file=router-config
```

A binary backup and a text export serve different purposes. Keep backups in a secure location and verify that they are available before making significant changes.

# 🗁 Managing WebFig Services

WebFig access is controlled through RouterOS IP services.

Inspect available services:

```routeros
/ip service print
```

Common web services include:

| Service | Description |
| --- | --- |
| www | HTTP-based WebFig access |
| www-ssl | HTTPS-based WebFig access |

To disable HTTP:

```routeros
/ip service disable www
```

To enable HTTPS:

```routeros
/ip service enable www-ssl
```

To configure the HTTPS service port:

```routeros
/ip service set www-ssl port=443
```

To restrict HTTPS access to a trusted management subnet:

```routeros
/ip service set www-ssl address=192.168.10.0/24
```

To inspect the updated service configuration:

```routeros
/ip service print detail
```

The `address` property restricts which source IP addresses can access the service. It is an additional access-control layer, not a replacement for firewall filtering.

# 🗁 Configuring HTTPS Certificates

HTTPS encrypts the communication between the administrator's browser and the router.

For production administration, use a certificate whose identity matches the hostname used to access WebFig and whose issuing chain is trusted by the administrator's browser.

A typical certificate workflow involves:

* Obtaining or generating an appropriate certificate.
* Importing the certificate and its required private key into RouterOS.
* Assigning the certificate to the `www-ssl` service.
* Verifying that the browser can establish a trusted HTTPS connection.

Inspect available certificates:

```routeros
/certificate print
```

Assign a certificate to WebFig HTTPS:

```routeros
/ip service set www-ssl certificate=webfig-certificate
```

Replace `webfig-certificate` with the actual certificate name in the router.

A self-signed certificate can encrypt the connection, but browsers will not automatically trust it unless the certificate or its issuing authority is explicitly trusted.

A trusted certificate helps establish both encrypted communication and server identity.

# 🗁 Restricting WebFig Access with Firewall Rules

RouterOS firewall rules can restrict WebFig access to trusted management networks.

Suppose the management subnet is:

```text
192.168.10.0/24
```

A suitable policy is to allow HTTPS management traffic from that subnet and block other sources from reaching the router's HTTPS service.

Example:

```routeros
/ip firewall filter
add chain=input action=accept protocol=tcp dst-port=443 src-address=192.168.10.0/24 comment="Allow WebFig HTTPS from management LAN"
add chain=input action=drop protocol=tcp dst-port=443 comment="Block WebFig HTTPS from other sources"
```

These are example rules, not a complete firewall configuration.

**Rule placement matters:** RouterOS processes firewall filter rules from top to bottom. Ensure that established and related traffic, required management access, and other legitimate input traffic are handled correctly by the existing firewall policy.

If the router uses a custom HTTPS port, update the firewall rules to match it.

Before applying firewall changes remotely:

* Confirm that the management subnet is correct.
* Check the existing input-chain rules.
* Ensure an appropriate management allow rule is placed before the blocking rule.
* Maintain an alternative recovery method where possible.

# 🗁 WebFig Security Best Practices

WebFig provides direct access to router administration. Its exposure should be controlled carefully.

Recommended practices include:

* Disable HTTP when it is not required.
* Use HTTPS for browser-based administration.
* Use strong, unique RouterOS administrator credentials.
* Create separate user accounts with appropriate permissions.
* Restrict WebFig access to trusted management IP addresses.
* Apply firewall filtering to management services.
* Prefer VPN access for remote administration.
* Keep RouterOS and device packages updated.
* Avoid exposing WebFig directly to the public internet.
* Use trusted certificates for HTTPS.
* Back up configurations before major changes.
* Review enabled IP services regularly.

Inspect enabled services:

```routeros
/ip service print
```

Inspect RouterOS users:

```routeros
/user print
```

Inspect input-chain firewall rules:

```routeros
/ip firewall filter print
```

A well-designed management network should separate administrative access from ordinary client traffic.

# 🗁 Troubleshooting WebFig

WebFig connectivity problems can originate from IP addressing, routing, service configuration, firewall rules, browser behavior, or TLS configuration.

| Problem | Possible Cause | What to Check |
| --- | --- | --- |
| Page does not load | Incorrect router IP | Verify the management address |
| Connection refused | Web service disabled | Inspect `/ip service` |
| Connection timeout | Firewall blocking traffic | Inspect input-chain rules |
| HTTP works but HTTPS fails | HTTPS service or certificate problem | Check `www-ssl` configuration |
| Browser certificate warning | Untrusted or mismatched certificate | Verify certificate identity and trust |
| Login rejected | Invalid credentials or insufficient permissions | Check user account configuration |
| WebFig becomes inaccessible after changes | Management IP, route, or firewall changed | Verify connectivity through another management method |
| Some menus are unavailable | User permissions or package limitations | Inspect permissions and installed packages |

Useful diagnostic commands:

```routeros
/ip service print detail
/ip address print
/ip route print
/ip firewall filter print
/user print
/system resource print
/log print
```

From a Linux workstation, test the HTTP and HTTPS ports:

```bash
nc -vz 192.168.88.1 80
nc -vz 192.168.88.1 443
```

Inspect the HTTPS certificate:

```bash
openssl s_client -connect 192.168.88.1:443 -servername router.example.com
```

Replace the IP address and hostname with the actual management endpoint.

# 🗁 Practical WebFig Administration Workflow

A safe administration workflow helps prevent accidental configuration loss and management lockouts.

**Before making changes:**

* Confirm the router's identity and management IP.
* Check the current configuration.
* Create a backup.
* Identify the management interface and subnet.
* Review relevant firewall and routing rules.

**While making changes:**

* Change one logical configuration area at a time.
* Apply changes carefully.
* Verify the configuration after each important step.
* Keep an existing management session open when practical.
* Avoid modifying the active management path without a recovery plan.

**After making changes:**

* Verify interface status.
* Confirm IP addressing and routing.
* Test client connectivity.
* Check firewall behavior.
* Confirm WebFig remains accessible.
* Save a fresh backup after successful changes.

# 🗁 Practical Lab: Enable Secure WebFig Access

This lab demonstrates how to enable HTTPS-based WebFig access and restrict it to a management subnet.

**Lab topology:**

| Component | Configuration |
| --- | --- |
| Router | MikroTik RouterOS v7 |
| Management interface | `ether2` |
| Management IP | `192.168.10.1/24` |
| Management subnet | `192.168.10.0/24` |
| HTTPS port | `443` |
| Administration method | WebFig over HTTPS |

**Configure the management IP:**

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

**Inspect web services:**

```routeros
/ip service print
```

**Disable HTTP:**

```routeros
/ip service disable www
```

**Configure HTTPS access:**

```routeros
/ip service set www-ssl disabled=no port=443 address=192.168.10.0/24
```

Ensure an appropriate certificate is configured for `www-ssl` before relying on HTTPS for administration.

**Verify the service configuration:**

```routeros
/ip service print detail
```

**Test connectivity from the management workstation:**

```bash
ping -c 4 192.168.10.1
```

```bash
nc -vz 192.168.10.1 443
```

**Open WebFig:**

```text
https://192.168.10.1
```

Log in with a RouterOS account that has the required management permissions.

**Verify the result:**

* HTTPS WebFig is reachable from the management subnet.
* HTTP access is disabled.
* The router accepts management connections only from the configured source subnet.
* The administrator can navigate the interface and inspect router configuration.

# 🗁 Official Documentation

* MikroTik RouterOS documentation: <https://help.mikrotik.com/docs/>
* MikroTik IP Services: <https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services>
* MikroTik WebFig: <https://help.mikrotik.com/docs/>
* MikroTik Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/48660574/Filter>

WebFig is a convenient browser-based administration interface, but secure management still depends on proper service configuration, firewall policies, user permissions, and network segmentation.
