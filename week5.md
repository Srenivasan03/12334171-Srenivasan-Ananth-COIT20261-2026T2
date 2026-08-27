# Week 05 Portfolio – COIT20261 Network Services and Automation

**Name:** Srenivasan Ananth  
**Student ID:** 12334171  
**Unit:** COIT20261 – Network Services and Automation  

---

# Task 1: Viewing Routing Tables

## Aim

The aim of this task was to build a small routed network with two IPv4 subnets, inspect routing tables, enable IP forwarding on the router, and verify communication between hosts on different subnets.

## Project

`View-Routes-12334171`

## Network Topology

The topology contained:

- Host1
- Host2
- Host3
- Router1
- Switch1

Host1 and Host2 were connected to Switch1. Switch1 was connected to Router1 `eth0`. Router1 `eth1` was connected directly to Host3.

## IP Addressing

| Device | Interface | IP Address | Default Gateway |
|---|---|---|---|
| Host1 | eth0 | 10.5.1.10/24 | 10.5.1.1 |
| Host2 | eth0 | 10.5.1.11/24 | 10.5.1.1 |
| Router1 | eth0 | 10.5.1.1/24 | — |
| Router1 | eth1 | 10.5.2.1/24 | — |
| Host3 | eth0 | 10.5.2.10/24 | 10.5.2.1 |

## Router Configuration

```text
auto eth0
iface eth0 inet static
    address 10.5.1.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1

auto eth1
iface eth1 inet static
    address 10.5.2.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1
```

## Routing Table Checks

On each node:

```bash
ip address show
ip route show
```

On Router1:

```bash
sysctl net.ipv4.ip_forward
```

The router showed:

```text
net.ipv4.ip_forward = 1
```

No additional manual static route was required because Router1 was directly connected to both `10.5.1.0/24` and `10.5.2.0/24`.

## Connectivity Test

From Host1:

```bash
ping -c 3 10.5.2.10
```

The ping was successful with 0% packet loss.

## Task 1 Outputs

- `View-Routes-12334171.gns3project`
- `View-Routes-12334171-network.png`
- `View-Routes-12334171-routing-tables.png`
- `View-Routes-12334171-ping.png`


## Screenshots

### Figure 1 – View Routes Topology

<img width="1188" height="536" alt="Screenshot 2026-08-27 103721" src="https://github.com/user-attachments/assets/7c1026a7-74c4-47a6-95b9-2e9d77aa2bf6" />


### Figure 2 – Host1 / Host2 / Router1 / Host3 IP and Routing Tables

<img width="560" height="371" alt="Screenshot 2026-08-27 103545" src="https://github.com/user-attachments/assets/a318b537-5c5a-4005-ba5e-1b45cdeacbd8" />


### Figure 3 – Successful Cross-Subnet Ping

<img width="591" height="209" alt="Screenshot 2026-08-27 103535" src="https://github.com/user-attachments/assets/d36f4cf6-3aca-41f9-9a76-8a9b72fc377f" />



---

# Task 2: OSPF Basics and Route Failover

## Aim

The aim of this task was to inspect OSPF neighbour and routing information in an existing FRRouting topology, trace the active route between two hosts, simulate a path failure, and observe OSPF reconvergence to an alternate route.

## Project

`OSPF-Basics-12334171`

The supplied OSPF template was used. The IP addressing and OSPF configuration were already present in the template.

## Topology

The OSPF topology contained:

- Host1
- Host2
- FRR-1
- FRR-2
- FRR-3
- FRR-4
- NETem1
- NETem2

The labelled networks were:

| Network | Subnet |
|---|---|
| A | 10.10.1.0/24 |
| B | 10.10.2.0/24 |
| C | 10.10.3.0/24 |
| D | 10.10.4.0/24 |
| E | 10.10.5.0/24 |
| F | 10.10.6.0/24 |

Host2 was verified as:

```text
10.10.6.102/24
```

## OSPF Verification

On FRR-1:

```text
show ip ospf neighbor
show ip ospf route
show ip route
```

Routing information was also checked on another FRR router.

## Traceroute Before Failure

From Host1:

```bash
traceroute 10.10.6.102
```

The original traceroute showed:

```text
1  10.10.1.1
2  10.10.2.2
3  10.10.4.4
4  10.10.6.102
```

This represented the upper route:

```text
Host1 → FRR-1 → FRR-2 → NETem1 → FRR-4 → Host2
```

## Simulated Path Failure

`NETem1` was stopped to break the upper OSPF path.

After waiting for OSPF to reconverge, traceroute was run again from Host1:

```bash
traceroute 10.10.6.102
```

## Traceroute After Failure

The new traceroute showed:

```text
1  10.10.1.1
2  10.10.3.3
3  10.10.5.4
4  10.10.6.102
```

This represented the alternative lower route:

```text
Host1 → FRR-1 → FRR-3 → NETem2 → FRR-4 → Host2
```

The successful change in path demonstrated OSPF reconvergence after a path failure.

## Task 2 Outputs

- `OSPF-Basics-12334171.gns3project`
- `OSPF-Basics-12334171-network.png`
- `OSPF-Basics-12334171-neighbors.png`
- `OSPF-Basics-12334171-FRR1-routes.png`
- `OSPF-Basics-12334171-FRR4-routes.png`
- `OSPF-Basics-12334171-traceroute-before.png`
- `OSPF-Basics-12334171-traceroute-after.png`


## Screenshots

### Figure 4 – OSPF Network Topology

<img width="1161" height="298" alt="image" src="https://github.com/user-attachments/assets/a4a054ba-0946-44ab-b640-1a9f7fc29793" />


### Figure 5 – FRR-1 OSPF Neighbours

<img width="549" height="359" alt="Screenshot 2026-08-27 104750" src="https://github.com/user-attachments/assets/af207e2b-9f01-44b1-a591-e5c16bd467db" />


### Figure 6 – FRR-1 Routing Table

<img width="549" height="359" alt="Screenshot 2026-08-27 104750" src="https://github.com/user-attachments/assets/4c4bb296-af43-4165-955f-ebfb88cd64f2" />

### Figure 7 – Second FRR Router Routing Table

<img width="543" height="359" alt="image" src="https://github.com/user-attachments/assets/0a646a14-9871-4795-b130-a63c6da7cc52" />

### Figure 8 – Traceroute before NETem1 Failure

<img width="527" height="379" alt="image" src="https://github.com/user-attachments/assets/ea6a4da2-7a43-4dca-9afe-b59923fdcb88" />


### Figure 9 – Traceroute After NETem1 Failure

<img width="2559" height="1305" alt="image" src="https://github.com/user-attachments/assets/40542130-63f7-45c3-80e6-02c996d774e2" />




## Learnings and Observations

- A router automatically learns routes to networks directly connected to its interfaces.
- End hosts use their default gateway to reach networks outside their local subnet.
- IPv4 forwarding must be enabled on a Linux router for it to forward packets between networks.
- OSPF dynamically exchanges routing information between routers.
- `show ip ospf neighbor` can be used to inspect OSPF neighbour relationships.
- `show ip ospf route` and `show ip route` can be used to inspect learned routes.
- `traceroute` shows the path packets take through the routed network.
- When NETem1 was stopped, OSPF recalculated the route and traffic automatically moved to the alternate path through FRR-3 and NETem2.
