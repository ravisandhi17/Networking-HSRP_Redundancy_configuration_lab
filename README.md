**Enterprise Campus Network Lab**

**Project Overview**

This project demonstrates a fully redundant enterprise campus network built in Cisco Packet Tracer using enterprise-grade routing and switching technologies.

The topology simulates a real-world multi-building enterprise environment featuring:

- Dynamic routing using OSPF
- Gateway redundancy using HSRP
- WAN serial connectivity
- Hierarchical network architecture
- Redundant routing paths
- High availability design
- Distribution and access layer switching

This lab was designed to strengthen practical networking skills related to enterprise infrastructure, routing, redundancy, and network troubleshooting.

---

**Network Topology**

![Enterprise Topology](screenshots/topology/topology.png)

---

**Network Architecture**

**Building 1**

**Routers**
- CORE_R1
- EDGE_R1
- EDGE_R2

**Switches**
- DIST_SW1
- DIST_SW2
- ACCESS_SW1
- ACCESS_SW2

**LAN Network**
- 10.10.40.0/24

**HSRP Virtual Gateway**
- 10.10.40.100

---

**Building 2**

**Routers**
- CORE_R2
- EDGE_R3
- EDGE_R4

**Switches**
- DIST_SW3
- DIST_SW4
- ACCESS_SW3
- ACCESS_SW4

**LAN Network**
- 30.30.30.0/24

**HSRP Virtual Gateway**
- 30.30.30.100

---

**WAN Connectivity**

| Connection | Network |
|---|---|
| CORE_R1 ↔ CORE_R2 | 50.50.50.0/24 |

The WAN serial connection enables routing communication between both enterprise buildings.

---

**Technologies Implemented**

- Cisco IOS Configuration
- OSPF Dynamic Routing
- HSRP Redundancy
- Floating Static Routes
- PVST (Per VLAN Spanning Tree)
- Hierarchical Enterprise Design
- Redundant Layer 3 Paths
- WAN Serial Connectivity
- Equal Cost Multi-Path Routing (ECMP)
- High Availability Architecture

---

**IP Addressing Scheme**

**Building 1 Networks**

| Network | Purpose |
|---|---|
| 10.10.10.0/24 | CORE_R1 ↔ EDGE_R1 |
| 10.10.20.0/24 | CORE_R1 ↔ EDGE_R2 |
| 10.10.30.0/24 | EDGE_R1 ↔ EDGE_R2 |
| 10.10.40.0/24 | User LAN |
| 10.10.40.100 | HSRP VIP |

---

**Building 2 Networks**

| Network | Purpose |
|---|---|
| 20.20.20.0/24 | CORE_R2 ↔ EDGE_R3 |
| 20.20.10.0/24 | CORE_R2 ↔ EDGE_R4 |
| 40.40.40.0/24 | EDGE_R3 ↔ EDGE_R4 |
| 30.30.30.0/24 | User LAN |
| 30.30.30.100 | HSRP VIP |

---

**OSPF Configuration**

OSPF Area 0 is configured across all routers for dynamic route advertisement and automatic route learning.

**Advertised Networks**

- 10.10.10.0/24
- 10.10.20.0/24
- 10.10.30.0/24
- 10.10.40.0/24
- 20.20.10.0/24
- 20.20.20.0/24
- 30.30.30.0/24
- 40.40.40.0/24
- 50.50.50.0/24

---

**HSRP Configuration**

HSRP is configured to provide gateway redundancy for both buildings.

---

**Building 1 HSRP**

| Router | Priority | State | Virtual IP |
|---|---|---|---|
| EDGE_R1 | 150 | Active | 10.10.40.100 |
| EDGE_R2 | 90 | Standby | 10.10.40.100 |

**Verification**


**EDGE_R1#show standby brief**

Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    150 P Active   local           10.10.40.2     10.10.40.100


**EDGE_R2#show standby brief**

Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    90  P Standby  10.10.40.1     local           10.10.40.100


**Building 2 HSRP**
| Router  | Priority | State   | Virtual IP   |
| ------- | -------- | ------- | ------------ |
| EDGE_R3 | 150      | Active  | 30.30.30.100 |
| EDGE_R4 | 90       | Standby | 30.30.30.100 |

**Verification**

**EDGE_R3#show standby brief**

Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    150 P Active   local           30.30.30.2     30.30.30.100

**EDGE_R4#show standby brief**

Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    90  P Standby  30.30.30.1     local           30.30.30.100

**Spanning Tree Configuration**

All switches are configured with PVST.

spanning-tree mode pvst

This prevents Layer 2 loops while maintaining redundancy in the switching infrastructure.

**Routing Verification**

The routing table confirms successful OSPF route learning and WAN communication.

Example from CORE_R1:
O       30.30.30.0/24 [110/66] via 50.50.50.2
•	40.40.40.0/24 [110/66] via 50.50.50.2
This verifies successful dynamic routing between both buildings.

**Floating Static Routes**

Floating static routes were configured with higher administrative distance values to act as backup routes if OSPF fails.

Example:
ip route 10.10.40.0 255.255.255.0 10.10.20.2 120


**Useful Verification Commands**

OSPF Neighbors

show ip ospf neighbor

Routing Table

show ip route

HSRP Status

show standby brief

**Interface Status**

show ip interface brief

**Testing Goals**

The following tests can be performed to validate the topology:

OSPF neighbor establishment

Dynamic route learning

HSRP failover testing

End-to-end connectivity

WAN communication verification

Inter-building communication

Redundant path validation


**Author**

Ravi Kumar
