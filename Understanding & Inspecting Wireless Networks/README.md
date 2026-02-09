# Understanding & Inspecting Wireless Networks

As I start working with wireless networks, I realize they are very different from wired ones. Wireless traffic is broadcast through the air, which makes it convenient — but also easier to observe and abuse if it’s not secured properly. In this section, I break down how wireless networks work, how to inspect them, and why visibility is critical for both troubleshooting and security.

---

## How Wireless Networks Work

Wireless networks use radio waves to transmit data.

Key components:
- Access Point (AP) → provides network access
- Client → device connecting to the AP
- SSID → network name
- Channel → radio frequency used

Anyone within range can see wireless traffic.

---

## Wireless Modes

Wireless interfaces can operate in different modes.

Managed mode:
- Normal client mode
- Connects to an access point

Monitor mode:
- Listens to all wireless traffic
- Does not associate with an AP

Monitor mode is essential for inspection and analysis.

---

## Identifying Wireless Interfaces

To see wireless interfaces:
iwconfig

Or:
ip link

Wireless interfaces often start with:
- wlan0
- wlan1

---

## Checking Wireless Network Information

To scan for nearby networks:
iw dev wlan0 scan

This reveals:
- SSIDs
- Signal strength
- Channels
- Encryption type

Root privileges are usually required.

---

## Encryption Types

Wireless security depends on encryption.

Common types:
- Open → no encryption
- WEP → broken and insecure
- WPA → improved but outdated
- WPA2 → strong when configured properly
- WPA3 → modern and most secure

Weak encryption equals weak security.

---

## MAC Addresses in Wireless Networks

Every wireless device has a MAC address.

Uses:
- Identify devices
- Filter access
- Track clients

MAC addresses can be spoofed and should not be trusted alone.

---

## Wireless Traffic Is Broadcast

Unlike wired networks:
- Packets are sent through the air
- Anyone in range can capture traffic
- Encryption protects the content, not the existence

Visibility is unavoidable in wireless environments.

---

## Inspecting Wireless Traffic

Wireless inspection focuses on:
- Network discovery
- Client activity
- Signal strength
- Encryption methods

This is useful for:
- Troubleshooting connectivity
- Security assessment
- Environment awareness

---

## Channels and Interference

Wireless networks share channels.

Problems caused by:
- Channel overlap
- Congested frequencies
- Physical obstacles

Choosing the right channel improves reliability and security.

---

## Hidden SSIDs

Some networks hide their SSID.

Important points:
- Hidden does not mean secure
- SSID still exists in traffic
- Clients reveal it when connecting

Security should never rely on hiding.

---

## Regulatory Domain

Wireless behavior depends on location.

The regulatory domain controls:
- Allowed channels
- Transmission power

Misconfiguration can limit performance or visibility.

---

## Practical Wireless Awareness

Here’s how I approach wireless inspection:

If I can’t connect:
iwconfig
iw dev wlan0 scan

If the signal is weak:
Check channel and distance

If security matters:
Confirm encryption type

If performance is bad:
Look for interference

---

## Wireless Security Mindset

Key questions to ask:
- Who can see this network?
- What encryption is in use?
- Are clients protected?
- Is traffic encrypted end-to-end?

Wireless always assumes an exposed environment.

---

## Common Mistakes

- Trusting MAC filtering
- Using outdated encryption
- Assuming hidden SSIDs are secure
- Ignoring interference

Wireless networks fail quietly when misconfigured.

---

## Good Wireless Practices

- Use WPA2 or WPA3
- Monitor nearby networks
- Choose clean channels
- Limit signal leakage
- Regularly review configurations

---

## Key Takeaway

Wireless networks are convenient but inherently exposed. Once I understand how wireless traffic works and how to inspect it, I gain visibility, control, and the ability to identify weaknesses before they become real problems.
