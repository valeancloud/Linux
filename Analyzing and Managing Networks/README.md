# Analyzing and Managing Networks

As I move deeper into Linux, networking becomes one of the most important areas to understand. Almost everything we do on a system involves communication—whether it’s reaching a website, connecting to another machine, or troubleshooting why something isn’t responding. In this section, I walk through the core tools I use to inspect, test, and understand network behavior directly from the terminal.

## Understanding Your Network Environment

Before analyzing anything, I like to get a quick snapshot of the system’s network configuration. The modern command for this is:
ip addr

This shows all network interfaces, their IP addresses, and their status. It’s the first place I look when I’m trying to understand how a machine is connected.

To see routing information—how the system decides where to send traffic—I use:
ip route

This tells me the default gateway, local routes, and how packets leave the system.

## Testing Connectivity

One of the simplest but most useful tools is ping. I use it to check whether a host is reachable:
ping google.com

This sends ICMP echo requests and shows how long each response takes. If I want to limit the number of packets:
ping -c 4 google.com

If ping fails, it usually means one of three things:
- The host is down
- The network path is blocked
- ICMP is disabled (common on servers)

## Inspecting Active Connections

To see what connections are currently open on the system, I use:
ss -tuln

This shows listening ports and active connections. The flags break down like this:
- **t**: TCP  
- **u**: UDP  
- **l**: listening sockets  
- **n**: show numeric addresses (faster and cleaner)

If I want to see which processes are tied to which ports:
ss -tulnp

This is extremely helpful when something is already using a port I need.

## Checking DNS Resolution

When a system can reach an IP but not a domain name, DNS is usually the issue. To test DNS resolution, I use:
nslookup example.com

Or for more detailed output:
dig example.com

These tools show which DNS server responded and what IP address was returned.

## Tracing Network Paths

If I can reach a host but the connection is slow or inconsistent, I trace the route:
traceroute google.com

This shows each hop between my machine and the destination. It’s useful for spotting delays, routing loops, or unreachable segments.

## Inspecting Network Traffic

When I want to see what’s happening on the wire, I use:
tcpdump

A simple capture on a specific interface looks like:
sudo tcpdump -i eth0

To filter for specific traffic, like HTTP:
sudo tcpdump -i eth0 port 80

Or to capture packets and save them for later analysis:
sudo tcpdump -i eth0 -w capture.pcap

I can then open the `.pcap` file in Wireshark for deeper inspection.

## Checking Host Information

To see the system’s hostname:
hostname

To see the mapping of hostnames to IPs on the local machine:
cat /etc/hosts

This file is often used for overrides or local testing.

## Practical Workflow

Here’s how I typically approach a network issue from start to finish.

First, I check the interface:
ip addr

Then I confirm the route:
ip route

Next, I test connectivity:
ping -c 4 target-host

If DNS might be the issue:
nslookup target-host

If a service isn’t responding, I check whether something is listening:
ss -tulnp

If the path looks suspicious or slow:
traceroute target-host

And if I need to see the actual packets:
sudo tcpdump -i eth0

By combining these tools, I can quickly narrow down whether the problem is local, network-related, DNS-related, or service-related.

## Building Good Networking Habits

A few habits make network analysis much easier:

- Always check IP configuration before troubleshooting anything else.
- Use ping to confirm basic reachability.
- Use ss to verify whether services are actually listening.
- Use dig or nslookup to confirm DNS behavior.
- Use traceroute to understand the path traffic takes.
- Use tcpdump when you need to see what’s really happening on the wire.

Networking can seem complex at first, but once you understand how to inspect each layer—interfaces, routes, DNS, services, and traffic—it becomes much easier to diagnose and fix issues with confidence.
