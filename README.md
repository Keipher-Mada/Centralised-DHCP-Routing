# Enterprise Network Simulation: Centralised DHCP & Routing

## 📌 Project Overview
This project simulates a real-world enterprise network environment connecting a headquarters server to three remote branch offices. The primary objective of this lab is to demonstrate the configuration of a centralised DHCP server and the implementation of DHCP Relay (`ip helper-address`) across different subnets and Variable Length Subnet Masks (VLSM) using the packet tracer simulation. 

##  Network Topology


## Network Topology Diagram:
<img width="1425" height="666" alt="Screenshot 2026-04-07 104250" src="https://github.com/user-attachments/assets/e4ee344e-4b88-42df-86b3-94c38e302eb3" />

## 🛠️ Technologies & Concepts Demonstrated
* **Cisco IOS CLI:** Router interface configuration and basic network management.
* **DHCP Relay Agents:** Bypassing router broadcast boundaries using `ip helper-address`.
* **Layer 3 Routing:** Connecting multiple distinct broadcast domains.
* **IPv4 Subnetting:** Managing wildly different network sizes (`/8`, `/16`, and `/24`) within the same routing environment.

## ⚙️ Configuration Highlights

### 1. Centralized DHCP Server (Headquarters)
Instead of placing a DHCP server at every branch, a single server at HQ (`10.0.0.2`) manages multiple IP pools. 
<img width="1904" height="969" alt="Screenshot 2026-04-07 104607" src="https://github.com/user-attachments/assets/bea8b899-dd56-4553-861b-c150b0a1834d" />

* **Branch 1 Pool:** `172.16.0.0 /16` (Gateway: `172.16.0.1`)
* **Branch 2 Pool:** `200.168.1.0 /24` (Gateway: `200.168.1.1`)
* **Branch 3 Pool:** `33.16.1.0 /8` (Gateway: `33.16.1.1`)

### 2. Router DHCP Relay Configuration
To allow branch clients to reach the central DHCP server, the router was configured as a Relay Agent. Below is an example snippet of the Cisco configuration used on the router's interface facing Branch 1:

```text
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/1
Router(config-if)# description Link to Branch 1
Router(config-if)# ip address 172.16.0.1 255.255.0.0
Router(config-if)# ip helper-address 10.0.0.2
Router(config-if)# no shutdown
```
This configuration was repeated with the appropriate IP addresses and subnet masks for the Branch 2 and Branch 3 interfaces as well.

✅ Testing & Verification

To verify the configuration is working as intended:

    Open the command prompt on any client PC in Branch 1, 2, or 3.

    Run ipconfig /renew.

    The PC will successfully pull an IP address, subnet mask, and default gateway specific to its physical branch location, proving the router is successfully tagging and relaying the DHCP broadcasts to the HQ Server.

This lab was built using Cisco Packet Tracer. The .pkt file is included in this repository for reference.
