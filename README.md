# Hardened Linux & Docker Home Server

> Repurposing legacy 2012 x86 hardware into an energy-efficient home network security and container host.

---

## Project Overview

This project documents the transformation of an old Mid-2012 Unibody MacBook Pro into a headless, secure home server. It hosts core network services in isolated Docker containers while implementing host-level security controls, DNS ad-blocking, and local infrastructure monitoring.

### Key Objectives
* **Infrastructure Repurposing:** Maximize legacy hardware utility while maintaining low thermal and power footprints.
* **Network Security:** Filter unauthorized DNS queries network-wide using Pi-hole.
* **System Hardening:** Implement firewall rules (`Uncomplicated Firewall`), disable root login, and enforce SSH key authentication.
* **Containerization:** Deploy modular network services using Docker & Docker Compose.

---

## Hardware Specifications & Architecture

| Component | Specification |
| :--- | :--- |
| **Host System** | MacBook Pro (13-inch, Mid 2012) |
| **CPU** | Intel Core i5-3210M (2.5GHz Dual-Core) |
| **RAM** | 8GB DDR3 (1600MHz) |
| **Network** | Native 10/100/1000BASE-T Gigabit Ethernet |
| **Integrated UPS** | Internal Lithium-ion Battery (acts as power drop failover) |
| **Optical Drive** | Built-in 8x SuperDrive (DVD/CD) |

### Network Topology

```mermaid
graph TD
    Router[Home Router / Gateway] -->|Gigabit Ethernet| MacBook[MacBook Pro Server - 192.168.1.150]
    
    Clients[Network Clients / Smart Devices] -->|DNS Requests :53| PiHole
    
    subgraph MacBook Host Environment
        MacBook --> UFW[UFW Firewall / SSH Hardened]
        UFW --> Docker[Docker Engine]
        
        subgraph Docker Containers
            Docker --> PiHole[Pi-hole - DNS Sinkhole]
            Docker --> Portainer[Portainer - Container Admin]
        end
    end
