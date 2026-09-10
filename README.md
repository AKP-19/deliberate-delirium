# Network Discovery & Traffic Analysis Lab

## Overview

This project simulates the initial discovery, documentation, and troubleshooting
work required when supporting technology within an existing site network.

The objective was to establish an accurate inventory of connected technology,
map the network's logical dependencies, and use packet-level analysis to
investigate endpoint connectivity and network behavior.

Rather than treating connectivity as a single condition, testing was performed
across multiple network layers to distinguish endpoint, addressing, gateway,
DNS, and protocol-related issues.

## Operational Scenario

A technology support associate may be responsible for a mixed environment of
workstations, mobile devices, printers, displays, instructional technology,
IoT devices, and network infrastructure.

Effective troubleshooting requires knowing:

- What equipment exists?
- How is each asset connected?
- What network configuration has the endpoint received?
- Can the endpoint reach required network resources?
- Is name resolution functioning?
- Where does a connectivity failure occur?
- What technical evidence should be documented or escalated?

This lab was designed around answering those questions.

## Environment

| Component | Operational Role |
|---|---|
| Windows Workstation | Support and analysis workstation |
| TP-Link Router/AP | Gateway, DHCP, NAT, firewall and wireless access |
| Cable Modem | Upstream ISP connectivity |
| PCs/Laptops | User endpoints |
| Mobile Devices | Wireless user endpoints |
| TV / IoT Equipment | Shared/site technology |
| Wireshark | Network troubleshooting and packet analysis |
| PowerShell | Endpoint configuration and network inspection |

Network identifiers have been sanitized in public documentation.

## Asset Inventory

An inventory was created to associate physical/logical assets with their
observed network identities.

Recorded information included:

- Asset type
- Connection method
- IPv4 address
- MAC/interface information
- Operational role

This provided a baseline for distinguishing known infrastructure, user
endpoints, and shared/IoT technology during troubleshooting.

## Network Documentation

The network's logical topology was documented from the ISP connection through
the local network:

Internet
   |
ISP Infrastructure
   |
DOCSIS
   |
Cable Modem
   |
WAN
   |
Router / Wireless AP
   |
Local WLAN
   |
User and Shared Endpoints

This map establishes the dependencies that must function for an endpoint to
reach local and Internet-hosted resources.

## Troubleshooting Methodology

Connectivity was evaluated as a sequence of dependencies rather than a single
"Internet works / Internet doesn't work" condition.

Endpoint
   |
   |-- Network configuration assigned?
   |        DHCP
   |
   |-- Local gateway identifiable?
   |        ARP
   |
   |-- Network path reachable?
   |        ICMP
   |
   |-- Hostnames resolving?
   |        DNS
   |
   |-- IPv6 infrastructure operating?
            ICMPv6 / NDP

Wireshark captures were used to validate each behavior at the packet level.

## Validation Tests

### NET-01 — ARP / Local Gateway Resolution

**Support Question:** Can the endpoint resolve its IPv4 gateway to the
appropriate Layer-2 address?

**Method:** Captured and analyzed ARP request/reply traffic.

**Result:** Gateway IPv4-to-MAC resolution confirmed. Helps distinguish Layer-2/local connectivity problems
from failures occurring farther upstream.

### NET-02 — DHCP / Endpoint Configuration

**Support Question:** Is the endpoint receiving appropriate network
configuration?

**Method:** Analyzed DHCP traffic and DHCP ACK configuration.

Validated:

- IPv4 assignment
- Subnet mask
- Default gateway
- DNS server
- DHCP server
- Lease information

**Result:** Dynamic endpoint configuration confirmed. Provides a repeatable method for investigating endpoints
that connect to a network but cannot properly reach network resources.

### NET-03 — DNS / Application Dependency

**Support Question:** Is the endpoint generating and receiving DNS traffic
required to locate network services?

**Method:** Captured DNS queries and responses and identified requested
hostnames and the responding resolver.

**Result:** DNS activity confirmed. Helps separate name-resolution failures from broader
network-connectivity or application failures.

### NET-04 — ICMP / Network Reachability

**Support Question:** Is an expected IPv4 network path reachable?

**Method:** Generated controlled ICMP traffic and analyzed packet results.

**Result:** Reachability behavior documented for the tested path. Provides evidence for determining whether an incident
should be investigated at the endpoint, local network, gateway, or farther
upstream.

### NET-05 — ICMPv6 / IPv6 Discovery

**Support Question:** What IPv6 network-discovery activity is occurring on the
local network?

**Method:** Analyzed ICMPv6 and Neighbor Discovery traffic and correlated
IPv6 sources with Layer-2 information.

**Result:** IPv6 control-plane behavior identified and documented.

## Findings

### Endpoint Discovery

Google Hub Device is still capable of communicating on the IPv6 network, potential Thread mesh capabilities allowed.

Packet analysis identified network activity associated with endpoints that
were not immediately apparent from the initial inventory. (A printer was discovered through ARP packet capture.)

IP addressing, MAC information, vendor information, and observed protocol
behavior were correlated before classifying devices.

This demonstrated how passive network evidence can supplement an existing
hardware inventory.

### Monitoring Limitation

Endpoint-based Wireshark capture did not provide complete visibility into
unicast communications between other WLAN clients and the gateway.

Investigation determined that the analysis workstation was neither inline with
those communications nor receiving mirrored traffic.

Broader monitoring would require an appropriate endpoint, gateway, inline, or
mirrored capture point.

This distinction is important when determining whether the absence of captured
traffic represents a connectivity failure or simply a limitation of the
monitoring location.

## Support Workflow Demonstrated

The project produced a repeatable troubleshooting workflow:

1. Identify the affected asset.
2. Verify physical/wireless connectivity.
3. Inspect endpoint network configuration.
4. Verify local gateway resolution.
5. Test network reachability.
6. Verify DNS resolution.
7. Capture relevant network traffic when required.
8. Identify the failing dependency.
9. Document findings and evidence.
10. Escalate with reproducible technical information when the issue falls
    outside local support scope.

## Deliverables

- Hardware/network asset inventory
- Logical network topology
- Network validation test matrix
- ARP capture evidence
- DHCP capture evidence
- DNS capture evidence
- ICMP capture evidence
- ICMPv6/NDP capture evidence
- Documented findings and monitoring limitations

## Skills Demonstrated

- Hardware/network asset inventory
- Site network documentation
- Windows network troubleshooting
- Wireshark packet analysis
- DHCP and endpoint configuration analysis
- DNS troubleshooting
- IPv4/IPv6 troubleshooting
- ARP and Neighbor Discovery
- Network-path isolation
- Technical documentation
- Evidence-based incident escalation
