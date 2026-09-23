# Network Segmentation Design with OPNsense

A clean, production-oriented network architecture designed for security, isolation, and clear traffic control.

---

## 1. Network Segments

| Network | Role | Real-World Example |
|---------|------|--------------------|
| **WAN** | OPNsense connection to the outside / Internet | Company Internet connection |
| **LAN** | Internal employee network | Corporate office network |
| **DMZ** | Servers that need to be accessible from outside | Public-facing company website |
| **MGMT** | Dedicated management network (only authorized systems such as OPNsense and SIEM) | IT / Security team network |

---

## 2. IP Addressing Plan

We use RFC 1918 private address ranges.

| Network | Subnet | Gateway | Purpose |
|---------|--------|---------|---------|
| **WAN** | DHCP / ISP-assigned | ISP Gateway | External / Internet connectivity |
| **LAN** | `10.10.10.0/24` | `10.10.10.1` | Internal clients and workstations |
| **DMZ** | `10.10.20.0/24` | `10.10.20.1` | Public-facing servers |
| **MGMT** | `10.10.30.0/24` | `10.10.30.1` | Management and security infrastructure |

---

## 3. Network Topology

                         INTERNET
                            |
                           WAN
                            |
                       [ OPNsense ]
                       /     |     \
                      /      |      \
                   LAN      DMZ      MGMT
                    |        |         |
              User PCs    Web Server  SIEM
              Workstations             |
                                   Management
                                   Interfaces

---

## 4. Traffic Policy

| Source | Destination | Policy | Reason |
|--------|-------------|--------|--------|
| **LAN** | Internet | **ALLOW** | Employees need outbound Internet access |
| **LAN** | DMZ | **DENY** | Clients should not have direct access to public-facing servers |
| **DMZ** | Internet | **ALLOW – LIMITED** | Only response traffic to requested connections |
| **Internet** | DMZ | **ALLOW – LIMITED** | Only required ports (e.g. 80/443) to the web server |
| **Internet** | LAN | **DENY** | No direct access to internal clients from the Internet |
| **MGMT** | All | **ALLOW – MANAGEMENT ONLY** | Management network has access only to required management interfaces and services |
| **LAN / DMZ** | MGMT | **DENY** | No traffic from LAN or DMZ into the management network |

---

## Design Principles

- **Least Privilege**: Only necessary traffic is permitted.
- **Isolation**: Management network is strictly separated from user and public networks.
- **Controlled Exposure**: DMZ hosts only the minimum required services.
- **Clear Boundaries**: Every segment has a defined purpose and strict policy.

---

*Designed for clarity, security, and real-world applicability.*
