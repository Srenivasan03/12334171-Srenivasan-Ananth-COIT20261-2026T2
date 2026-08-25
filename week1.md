# Week 01 Portfolio – COIT20261 Network Services and Automation

**Name:** Srenivasan Ananth
**Student ID:** 12334171  
**Unit:** COIT20261 – Network Services and Automation  
**Week:** 01  

---

## Section A – Unit Setup

### GitHub Repository

A private GitHub repository was created for this unit using the required naming format:

`12334171-Srenivasan-Ananth-COIT20261-2026T2`

The repository was shared with the tutor and will be used to store weekly portfolio files and supporting evidence.

### Software Setup

The following software was checked and prepared for the unit:

- VirtualBox


  
- GNS3
  

  
- Web browser for accessing the GNS3 interface

---
<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/5fee2810-ccff-4c66-b549-87bca945966e" />


## Section B – Task 1: Introduction to GNS3 Basics

### Aim

The aim of this task was to become familiar with basic GNS3 operations, including creating a project, adding a Linux Host, assigning a static IP address, starting the node, opening the console, and checking the configured IP address.

### Project Details

**Project name:** `12334171`  
**Node:** Linux Host  
**Interface:** `eth0`  
**IP address:** `10.10.1.1`  
**Subnet mask:** `255.255.255.0`  

### Activities Completed

1. Created a new GNS3 project named `GNS3-Intro-[12334171]`.
2. Added one Linux Host node to the project.
3. Added text annotations showing the project title, name, student ID and date.
4. Added an annotation beside the Linux Host showing its IP address.
5. Configured a static IPv4 address on `eth0` before starting the node.
6. Disabled IPv4 forwarding because the node is being used as a host rather than a router.
7. Started the Linux Host.
8. Opened the web console.
9. Ran `ip address show` to verify the configured address.
10. Saved the required screenshots and exported the GNS3 project.

### Network Configuration

The following configuration was entered in `/etc/network/interfaces`:

```text
auto eth0
iface eth0 inet static
    address 10.10.1.1
    netmask 255.255.255.0
    up sysctl net.ipv4.ip_forward=0
```

### Command Used

```bash
ip address show
```

The output confirmed that `eth0` was configured with `10.10.1.1/24`.

---

## Outputs

The following evidence was produced:

- `GNS3-Intro-[YOUR STUDENT ID].gns3project`
- `GNS3-Intro-[YOUR STUDENT ID]-network.png`
- `GNS3-Intro-[YOUR STUDENT ID]-ipaddress.png`

---

## Screenshots

### Figure 1 – GNS3 Network

<img width="1091" height="581" alt="image" src="https://github.com/user-attachments/assets/2400a1b1-8e88-4dd3-b3fb-bd9a1ab14eee" />

```markdown
![GNS3 Week 1 Network](images/GNS3-Intro-[YOUR STUDENT ID]-network.png)
```

The screenshot should show the Linux Host together with the project annotation and the configured IP address.

### Figure 2 – IP Address Verification

<img width="553" height="364" alt="image" src="https://github.com/user-attachments/assets/5a47663f-9aa8-42a7-aa2c-0ee91ac526a0" />


```markdown
![Linux Host IP Address](images/GNS3-Intro-[YOUR STUDENT ID]-ipaddress.png)
```

The console screenshot should clearly show the `ip address show` command and the `10.10.1.1/24` address on `eth0`.

---

## Learnings and Observations

- A static IP address can be configured through the Linux network interface configuration before the node starts.
- The `ip address show` command displays the IP addresses assigned to network interfaces.
- IPv4 forwarding can be disabled on a normal host using `net.ipv4.ip_forward=0`.
- GNS3 allows the project topology and console output to be documented using screenshots and exported project files.
