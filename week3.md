# Week 03 Portfolio – COIT20261 Network Services and Automation

**Name:** Srenivasan Ananth  
**Student ID:** 12334171  
**Unit:** COIT20261 – Network Services and Automation  

---

# Task 1: Simple Application Communications with Netcat

## Aim

The aim of this task was to use Netcat (`nc`) to test simple application-level communication between Linux hosts.

## Network Used

- **Project:** `Setting-IP-12334171`
- **Network:** `10.1.1.0/24`
- **Host1:** `10.1.1.1/24`
- **Host2:** `10.1.1.2/24`
- **Host3:** `10.1.1.3/24`
- **Topology:** Four Linux hosts connected to one Ethernet switch

## Activities Completed

1. Reused the Week 2 `Setting-IP-12334171` project.
2. Started a Netcat server on Host1 using port `4444`.
3. Connected Host2 to the Netcat server on Host1.
4. Sent the name `Srenivasan Ananth` from Host2 to Host1.
5. Sent the student ID `12334171` from Host1 to Host2.
6. Confirmed that messages were visible on both consoles.

## Commands Used

### Host1 – Netcat Server

```bash
nc -l -p 4444
```

### Host2 – Netcat Client

```bash
nc 10.1.1.1 4444
```

## Output

- `Netcat-Basics-12334171-client-server.png`

## Screenshot

### Figure 1 – Netcat Client and Server Communication

<img width="2416" height="1198" alt="image" src="https://github.com/user-attachments/assets/0edc7988-add9-4912-a49c-7ad90506139f" />


The screenshot shows the Netcat server and client communicating successfully. The name and student ID were exchanged between the two hosts.

---

# Task 2: Capturing Packets

## Aim

The aim of this task was to capture network packets on a GNS3 link while generating ICMP and Netcat traffic.

## Activities Completed

1. Used the same `Setting-IP-12334171` project.
2. Started packet capture on the link between Host1 and the Ethernet switch.
3. Generated three ICMP echo requests from Host1 to Host2.
4. Started a Netcat server on Host3 using port `4444`.
5. Connected Host1 to Host3 using Netcat.
6. Sent the name `Srenivasan Ananth` from Host1 to Host3.
7. Stopped the packet capture.
8. Saved the capture as a `.pcap` file.

## Commands Used

### Ping from Host1 to Host2

```bash
ping -c 3 10.1.1.2
```

### Host3 – Netcat Server

```bash
nc -l -p 4444
```

### Host1 – Netcat Client

```bash
nc 10.1.1.3 4444
```

Message sent:

```text
Srenivasan Ananth
```

## Output

- `Capture-Basics-12334171-ping-netcat.pcap`
<img width="2203" height="1168" alt="image" src="https://github.com/user-attachments/assets/1e959fd3-1516-4e68-bc5e-73d7ffad943b" />

---

## Learnings and Observations

- Netcat can be used to test TCP application communication between two devices.
- A Netcat server listens on a specified port and a client connects using the server IP address and port.
- `ping` uses ICMP, while Netcat can use TCP for application-level communication.
- GNS3 can capture packets directly from a network link.
- The `.pcap` file can be opened in Wireshark for packet analysis.
