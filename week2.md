# Week 02 Portfolio – COIT20261 Network Services and Automation

**Name:** Srenivasan Ananth  
**Student ID:** 12334171  
**Unit:** COIT20261 – Network Services and Automation  

---

# Task 1: Setting Static IP Addresses

## Aim

The aim of this task was to configure static IPv4 addresses on Linux hosts in GNS3 using different configuration methods.

## Network Used

- **Project:** `Setting-IP-12334171`
- **Network:** `10.1.1.0/24`
- **Topology:** 4 Linux Hosts connected to 1 Ethernet Switch

## IP Address Assignments

| Host | Interface | IP Address | Configuration Method |
|---|---|---|---|
| Host1 | eth0 | 10.1.1.1/24 | GNS3 Configure menu |
| Host2 | eth0 | 10.1.1.2/24 | GNS3 Configure menu |
| Host3 | eth0 | 10.1.1.3/24 | `/etc/network/interfaces` |
| Host4 | eth0 | 10.1.1.4/24 | `ip address add` command |

---

## Activities Completed

1. Created the GNS3 project `Setting-IP-12334171`.
2. Added four Linux Host nodes and one Ethernet Switch.
3. Connected all four Linux hosts to the switch to form one LAN.
4. Configured Host1 and Host2 through the GNS3 network configuration menu.
5. Configured Host3 by editing `/etc/network/interfaces`.
6. Configured Host4 at runtime using the `ip address add` command.
7. Used `ip address show` to verify the configured addresses.

---

## Method 1 – GNS3 Configure Menu

### Host1

```text
auto eth0
iface eth0 inet static
    address 10.1.1.1
    netmask 255.255.255.0
```

### Host2

```text
auto eth0
iface eth0 inet static
    address 10.1.1.2
    netmask 255.255.255.0
```

---

## Method 2 – Editing `/etc/network/interfaces`

Host3 was configured from its Linux console.

```bash
nano /etc/network/interfaces
```

The following settings were entered:

```text
auto eth0
iface eth0 inet static
    address 10.1.1.3
    netmask 255.255.255.0
```

The interface configuration was then reloaded:

```bash
ifdown eth0
ifup eth0
```

The address was checked using:

```bash
ip address show
```

---

## Method 3 – `ip address add`

Host4 was left without a persistent static IP configuration in `/etc/network/interfaces`.

The address was added directly from the console using:

```bash
ip address add 10.1.1.4/24 dev eth0
```

The configured address was verified using:

```bash
ip address show
```

---

## Task 1 Outputs

- `Setting-IP-12334171.gns3project`
- `Setting-IP-12334171-network.png`
- `Setting-IP-12334171-host1.png`
- `Setting-IP-12334171-host2.png`
- `Setting-IP-12334171-host3.png`
- `Setting-IP-12334171-host4.png`

---

## Screenshots

### Figure 1 – GNS3 Network Topology

<img width="2547" height="1233" alt="image" src="https://github.com/user-attachments/assets/eb43ed9d-8fea-44fc-990a-57e2b0785d17" />



The topology consists of four Linux hosts connected to a single Ethernet switch.

### Figure 2 – Host1 Static IP Configuration

<img width="1633" height="1024" alt="image" src="https://github.com/user-attachments/assets/d72c54f2-e8d0-4a33-a6dc-cce31da02f35" />

Host1 was configured with the static address `10.1.1.1/24`.

### Figure 3 – Host2 Static IP Configuration

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/93709349-08a4-4b44-a222-0e296fb57f26" />



Host2 was configured with the static address `10.1.1.2/24`.

### Figure 4 – Host3 Static IP Configuration

<img width="1939" height="1231" alt="image" src="https://github.com/user-attachments/assets/c8111c82-2097-46c3-92b7-903ef2f9dbbf" />



Host3 was configured with the static address `10.1.1.3/24`.

### Figure 5 – Host4 IP Address

<img width="1737" height="1017" alt="image" src="https://github.com/user-attachments/assets/01529104-5e42-4959-8683-b54423897ed7" />


Host4 was configured at runtime with `10.1.1.4/24` using the `ip address add` command.

---

# Task 2: Testing Network Connectivity and Delay with Ping

## Aim

The aim of this task was to use `ping` to test whether hosts were reachable and observe packet delay and packet loss.

## Basic Ping

From Host1, Host2 was tested using:

```bash
ping 10.1.1.2
```

After at least five successful responses, the command was stopped using `Ctrl+C`.

## Ping to a Non-Existing Address

An unused address on the LAN was tested using:

```bash
ping 10.1.1.99
```

The ping was allowed to run for approximately 10 seconds and was then stopped using `Ctrl+C`.

## Ping with Options

The following command was used to modify the packet count, interval and data size:

```bash
ping -c 3 -i 2 -s 100 10.1.1.2
```

Where:

- `-c 3` limits the test to three packets.
- `-i 2` sets a two-second interval between packets.
- `-s 100` sets the data size to 100 bytes.

---

## Task 2 Outputs

- `Ping-Basics-12334171-simple.png`
- `Ping-Basics-12334171-error.png`
- `Ping-Basics-12334171-options.png`

---

## Screenshots

### Figure 6 – Basic Ping Test

<img width="674" height="393" alt="image" src="https://github.com/user-attachments/assets/acc8098b-15e6-4b53-86ad-7b4535b6440e" />

The successful ping responses confirmed connectivity between Host1 and Host2.

### Figure 7 – Ping to an Unused Address

<img width="563" height="181" alt="image" src="https://github.com/user-attachments/assets/b14f0af0-15c9-49c8-a06b-606d335f4783" />


The test to the unused destination demonstrated packet loss when no device responded.

### Figure 8 – Ping with Command-Line Options

<img width="497" height="143" alt="image" src="https://github.com/user-attachments/assets/e53a327e-3a11-4a4a-a051-31098f7e9fff" />


The final test demonstrated the use of count, interval and packet-size options.

---

## Learnings and Observations

- Static IPv4 addresses can be configured using several Linux networking methods.
- Addresses configured through `/etc/network/interfaces` can remain after a restart.
- The `ip address add` method applies the address immediately but does not persist after reboot.
- `ip address show` can be used to verify the current network configuration.
- `ping` can be used to test reachability and observe round-trip time.
- Packet loss occurs when the destination address does not respond.
