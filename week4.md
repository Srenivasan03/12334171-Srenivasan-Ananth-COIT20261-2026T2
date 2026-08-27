# Week 04 Portfolio – COIT20261 Network Services and Automation

**Name:** Srenivasan Ananth  
**Student ID:** 12334171  
**Unit:** COIT20261 – Network Services and Automation  

---

# Task 1: HTTP Client with GUI

## Aim

The aim of this task was to build a three-subnet routed network in GNS3, access a web server from a Firefox client, and capture the HTTP traffic passing through Subnet B.

## Project

`HTTPClient-GUI-12334171`

## Network Topology

- Firefox Host on Subnet A
- Switch1 on Subnet A
- Router1 connecting Subnets A and B
- Switch2 on Subnet B
- Router2 connecting Subnets B and C
- Switch3 on Subnet C
- Linux Server on Subnet C

## IP Addressing

| Device | Interface | IP Address | Default Gateway |
|---|---|---|---|
| Firefox Host | eth0 | 192.168.1.10/24 | 192.168.1.1 |
| Router1 | eth0 | 192.168.1.1/24 | — |
| Router1 | eth1 | 192.168.2.1/24 | — |
| Router2 | eth0 | 192.168.2.2/24 | — |
| Router2 | eth1 | 192.168.3.1/24 | — |
| Linux Server | eth0 | 192.168.3.10/24 | 192.168.3.1 |

## Router Configuration

### Router1

```text
auto eth0
iface eth0 inet static
    address 192.168.1.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1

auto eth1
iface eth1 inet static
    address 192.168.2.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1
```

Static route:

```bash
ip route add 192.168.3.0/24 via 192.168.2.2
```

### Router2

```text
auto eth0
iface eth0 inet static
    address 192.168.2.2
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1

auto eth1
iface eth1 inet static
    address 192.168.3.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=1
```

Static route:

```bash
ip route add 192.168.1.0/24 via 192.168.2.1
```

## Linux Server Configuration

```text
auto eth0
iface eth0 inet static
    address 192.168.3.10
    netmask 255.255.255.0
    gateway 192.168.3.1
```

The Linux Server was running Nginx on TCP port 80.

Verification:

```bash
ss -ltnp | grep ':80'
```

## Connectivity Testing

From Router1:

```bash
ping -c 3 192.168.3.10
```

The server responded successfully.

## Packet Capture and HTTP Access

1. Started packet capture on the Router1–Switch2 link in Subnet B.
2. Opened the Firefox Host through VNC.
3. Browsed to:

```text
http://192.168.3.10
```

4. The Networkers' Toolkit page loaded successfully.
5. Stopped the packet capture.

## Task 1 Outputs

- `HTTPClient-GUI-12334171.gns3project`
- `HTTPClient-GUI-12334171-network.png`
- `HTTPClient-GUI-12334171-subnetB.pcap`
- `HTTPClient-GUI-12334171-firefox.png`

## Screenshots

### Figure 1 – GUI Network Topology

<img width="1196" height="563" alt="Screenshot 2026-08-26 220345" src="https://github.com/user-attachments/assets/0148d50f-0600-45e8-aedb-764d7fe6a584" />


### Figure 2 – Firefox HTTP Access

<img width="1072" height="551" alt="Screenshot 2026-08-26 220213" src="https://github.com/user-attachments/assets/99d5d4d0-83de-4f5c-aa51-8eea87d8e968" />

Firefox successfully accessed the Linux Server at `http://192.168.3.10`.

---

# Task 2: HTTP Client with CLI

## Aim

The aim of this task was to replace the Firefox client with a Linux Host and access the same HTTP server using command-line HTTP tools.

## Project

`HTTPClient-CLI-12334171`

The Task 1 project was duplicated and the Firefox Host was replaced with a Linux Host.

## Linux Client Configuration

```text
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
```

## Routing

Router1:

```bash
ip route add 192.168.3.0/24 via 192.168.2.2
```

Router2:

```bash
ip route add 192.168.1.0/24 via 192.168.2.1
```

## Connectivity Test

```bash
ping -c 3 192.168.3.10
```

The ping was successful.

## HTTP Access with wget

Started packet capture again on the Router1–Switch2 link and ran:

```bash
wget http://192.168.3.10/
```

The request completed successfully. The packet capture was then stopped.

## HTTP Access with curl

```bash
curl http://192.168.3.10/
```

The server response was displayed directly in the terminal.

## Task 2 Outputs

- `HTTPClient-CLI-12334171.gns3project`
- `HTTPClient-CLI-12334171-network.png`
- `HTTPClient-CLI-12334171-subnetB.pcap`
- `HTTPClient-CLI-12334171-wget-curl.png`

## Screenshots

### Figure 3 – CLI Network Topology

<img width="1125" height="240" alt="image" src="https://github.com/user-attachments/assets/b2d13e6a-e9e6-407b-a5ac-2a82b5ebaf54" />


### Figure 4 – wget and curl

<img width="1093" height="631" alt="image" src="https://github.com/user-attachments/assets/9f04a267-f92e-41ad-a83d-a715b1dcab83" />


---

## Learnings and Observations

- IPv4 forwarding is required on routers to pass traffic between subnets.
- Static routes allow each router to reach remote networks.
- Routes added with `ip route add` are temporary and need to be re-added after a restart unless configured persistently.
- Clients require the correct default gateway to communicate outside their local subnet.
- Packet capture on Subnet B records the HTTP traffic passing between the client and server.
- Firefox, `wget`, and `curl` can all access the same HTTP server using different interfaces.
