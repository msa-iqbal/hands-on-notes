# Netwatch

Netwatch is a network monitoring feature in MikroTik RouterOS that continuously checks the availability of remote hosts and executes configured actions when their status changes.

It is commonly used to monitor gateways, servers, DNS resolvers, remote routers, and other network devices.

Netwatch can help administrators detect connectivity failures, monitor service availability, trigger notifications, and automate basic network recovery operations.

# 🗁 Understanding Netwatch

Netwatch monitors a specified target using a configured probe method.

Depending on the RouterOS version and configuration, supported probe methods include:

* ICMP: Checks whether a target responds to ICMP echo requests.
* TCP connection: Checks whether a TCP connection can be established.
* HTTP GET: Checks an HTTP endpoint.
* HTTPS GET: Checks an HTTPS endpoint.
* DNS: Checks DNS resolution.

Netwatch maintains a monitoring status and can execute scripts when a target changes from an up state to a down state or vice versa.

Example monitoring architecture:

```text
              MikroTik Router
                     |
              Netwatch Monitor
                     |
              Target Host
                     |
             Availability Check
                     |
             +-------+-------+
             |               |
            UP              DOWN
             |               |
       on-up Script     on-down Script
             |               |
       Log Event        Alert / Action
```

# 🗁 Viewing Netwatch Configuration

Display configured Netwatch entries:

```routeros
/tool/netwatch/print
```

For detailed information:

```routeros
/tool/netwatch/print detail
```

Each entry can contain:

* Host address or endpoint.
* Probe type.
* Monitoring interval.
* Timeout.
* Current status.
* Up and down scripts.
* Other probe-specific settings.

# 🗁 Monitoring a Host with ICMP

ICMP monitoring checks whether a target responds to ping requests.

Add a Netwatch entry:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s
```

This configuration checks `8.8.8.8` every 30 seconds with a two-second timeout.

Inspect the entry:

```routeros
/tool/netwatch/print detail
```

Netwatch updates the target's availability status based on the configured probe.

ICMP monitoring is useful for checking basic IP reachability, but a successful ping does not necessarily mean that an application's service is working.

# 🗁 Monitoring a Remote Router

You can monitor another MikroTik router using its reachable IP address.

For example, assume:

| Device | IP Address |
| --- | --- |
| Local router | `192.168.10.1` |
| Remote router | `192.168.20.1` |

Add the monitoring entry:

```routeros
/tool/netwatch/add host=192.168.20.1 type=icmp interval=30s timeout=2s
```

Netwatch checks whether the remote router responds.

This can help detect:

* Remote router shutdown.
* WAN connectivity failure.
* Routing problems.
* Remote network interruptions.

Make sure the target allows ICMP requests and that a valid route exists between the routers.

# 🗁 Configuring on-up and on-down Scripts

Netwatch supports scripts that execute when a monitored target changes state.

The `up-script` runs when the target becomes available.

The `down-script` runs when the target becomes unavailable.

Example:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s \
    up-script="/log/info \"Internet connectivity restored\"" \
    down-script="/log/warning \"Internet connectivity lost\""
```

The scripts write messages to the RouterOS log.

Inspect the log:

```routeros
/log/print where topics~"netwatch"
```

The actual log topics depend on the script commands and configured logging rules.

Netwatch scripts should be designed carefully. Repeated state changes can cause repeated script executions.

# 🗁 Monitoring Multiple Hosts

Netwatch can monitor multiple independent targets.

For example, monitor two public DNS resolvers:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s
```

```routeros
/tool/netwatch/add host=1.1.1.1 type=icmp interval=30s timeout=2s
```

Monitor an internal server:

```routeros
/tool/netwatch/add host=192.168.10.100 type=icmp interval=30s timeout=2s
```

List all monitoring entries:

```routeros
/tool/netwatch/print
```

Monitoring multiple independent targets helps distinguish between a single-host failure and a broader connectivity problem.

# 🗁 Monitoring TCP Services

ICMP monitoring only checks IP-level reachability.

A host may respond to ping while its application service is unavailable.

TCP monitoring can check whether a particular service port accepts connections.

For example, monitor an HTTPS service:

```routeros
/tool/netwatch/add host=192.168.10.100 type=tcp-connect port=443 interval=30s timeout=3s
```

Monitor an SSH service:

```routeros
/tool/netwatch/add host=192.168.10.100 type=tcp-connect port=22 interval=30s timeout=3s
```

This checks whether a TCP connection can be established to the specified port.

A successful TCP connection does not prove that the application is functioning correctly. It only verifies that the TCP connection test succeeds.

# 🗁 Monitoring HTTP and HTTPS Endpoints

RouterOS versions supporting HTTP GET and HTTPS GET probes can monitor application endpoints.

For example, an HTTP endpoint:

```routeros
/tool/netwatch/add host=http://192.168.10.100/health type=http-get interval=30s timeout=5s
```

An HTTPS endpoint:

```routeros
/tool/netwatch/add host=https://example.com/health type=https-get interval=30s timeout=5s
```

The target endpoint should return the expected response for the monitoring test.

Application-level monitoring is useful when basic IP connectivity is insufficient to determine service availability.

The exact HTTP and HTTPS probe options depend on the RouterOS release. Check the available properties using:

```routeros
/tool/netwatch/add ?
```

# 🗁 Monitoring DNS Resolution

DNS monitoring can be used to check whether a configured DNS name can be resolved.

For example:

```routeros
/tool/netwatch/add host=example.com type=dns interval=30s timeout=5s
```

This helps monitor DNS resolution availability rather than only checking whether a DNS server responds to ping.

DNS monitoring is useful for detecting:

* DNS server failures.
* Resolution problems.
* Upstream DNS connectivity issues.
* Incorrect DNS configuration.

A successful DNS lookup does not guarantee that the destination application is reachable.

# 🗁 Netwatch for Internet Connectivity Monitoring

A basic Internet monitoring configuration can use multiple external targets.

For example:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s
```

```routeros
/tool/netwatch/add host=1.1.1.1 type=icmp interval=30s timeout=2s
```

Monitoring more than one target helps avoid interpreting a single destination's failure as a complete Internet outage.

For more reliable monitoring, consider:

* Independent targets hosted by different providers.
* Appropriate monitoring intervals.
* Suitable timeout values.
* Routing through the intended WAN.
* Application-level probes where required.

A successful ICMP response only confirms reachability to the selected target through the path used by that probe.

# 🗁 Netwatch for WAN Monitoring

Netwatch can be used to monitor WAN connectivity.

Assume:

| Interface | Description |
| --- | --- |
| `ether1` | Primary WAN |
| `ether2` | Backup WAN |
| `8.8.8.8` | Primary WAN monitoring target |

A basic monitoring entry:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s
```

When the target becomes unavailable, Netwatch can execute a script.

However, this basic configuration does not automatically guarantee that the probe uses `ether1`.

If the router has multiple WAN interfaces, the monitoring traffic must be routed through the intended WAN. Use appropriate routing tables, routing rules, or other policy-routing mechanisms.

For production failover, recursive gateway checks and routing-based failover are generally more suitable for controlling route selection. Netwatch can complement them with notifications and additional automation.

# 🗁 Configuring Netwatch Notifications Through Logging

Netwatch scripts can generate log messages when connectivity changes.

Example:

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s \
    down-script="/log/warning \"Primary monitoring target unavailable\"" \
    up-script="/log/info \"Primary monitoring target restored\""
```

Review the messages:

```routeros
/log/print
```

If remote Syslog logging is configured for the relevant topics, these events can also be forwarded to a centralized logging server.

# 🗁 Managing Netwatch Entries

List all entries:

```routeros
/tool/netwatch/print
```

Inspect detailed configuration:

```routeros
/tool/netwatch/print detail
```

Disable a monitoring entry:

```routeros
/tool/netwatch/disable [find host=8.8.8.8]
```

Enable it again:

```routeros
/tool/netwatch/enable [find host=8.8.8.8]
```

Remove an entry:

```routeros
/tool/netwatch/remove [find host=8.8.8.8]
```

When multiple entries monitor the same host, use the entry number or another unique selector to avoid modifying the wrong entry.

# 🗁 Netwatch Limitations

Netwatch is useful for basic availability monitoring and event-driven automation, but it has limitations.

* ICMP monitoring does not verify application availability.
* A target may block ICMP while its services remain operational.
* Routing changes can cause probes to use an unexpected WAN.
* Network congestion can produce temporary probe failures.
* Monitoring one external host is insufficient to prove complete Internet availability.
* Repeated state changes can trigger repeated scripts.
* Incorrect script permissions or configuration can prevent intended actions.
* Probe capabilities and options depend on the RouterOS version.

For critical infrastructure, combine Netwatch with routing-based health checks, centralized monitoring, and application-level monitoring.

# 🗁 Practical Netwatch Monitoring Example

The following example monitors an external target and records availability changes.

**Create the monitoring entry**

```routeros
/tool/netwatch/add host=8.8.8.8 type=icmp interval=30s timeout=2s \
    down-script="/log/warning \"Internet monitoring target is DOWN\"" \
    up-script="/log/info \"Internet monitoring target is UP\""
```

**Verify the configuration**

```routeros
/tool/netwatch/print detail
```

**Review system logs**

```routeros
/log/print
```

**Add a second independent monitoring target**

```routeros
/tool/netwatch/add host=1.1.1.1 type=icmp interval=30s timeout=2s
```

**Inspect all monitoring entries**

```routeros
/tool/netwatch/print
```

This configuration provides basic reachability monitoring and records status transitions in the router logs.

# 🗁 Best Practices for Netwatch

* Monitor more than one independent external target.
* Choose monitoring intervals appropriate for the network.
* Use reasonable timeout values.
* Use TCP or HTTP probes when application availability matters.
* Ensure monitoring traffic follows the intended routing path.
* Keep scripts simple and predictable.
* Avoid unnecessary configuration changes from monitoring scripts.
* Forward important monitoring events to centralized Syslog.
* Correlate Netwatch results with interface and routing statistics.
* Use recursive routing checks for production WAN failover.
* Test both recovery and failure scenarios before relying on automation.

Netwatch provides a practical way to monitor network availability and execute automated actions when monitored hosts change state.
