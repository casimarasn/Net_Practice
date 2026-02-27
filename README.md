*This project has been created as part of the 42 curriculum by msedeno-.*

---

<div align="center">

# 🌐 NetPractice

![42 Badge](https://img.shields.io/badge/42-NetPractice-00babc?style=for-the-badge&logo=42&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Language](https://img.shields.io/badge/Topic-Networking-blue?style=for-the-badge&logo=cisco&logoColor=white)

*A hands-on introduction to computer networking and TCP/IP addressing.*

🇬🇧 English | [🇪🇸 Español](README.es.md)

</div>

---

## 📋 Description

**NetPractice** is a practical exercise from the 42 curriculum designed to introduce students to the fundamentals of **computer networking**. Working through a browser-based training interface, you configure small-scale simulated networks by assigning correct IP addresses, subnet masks, and default gateways so that all devices can communicate as required.

The project consists of **10 levels** of increasing complexity. Each level presents a broken network diagram and one or more communication goals. The student must modify the editable fields until the network functions correctly, then export the configuration for submission.

> ⚠️ All networks in this project are **simulated** and have no connection to real-world infrastructure.

---

## 🧠 Networking Theory

Understanding the following concepts is **essential** to complete this project.

---

### 🔹 The OSI Model

The **OSI (Open Systems Interconnection)** model is a conceptual framework that divides network communication into **7 layers**:

| Layer | Name | Role |
|-------|------|------|
| 7 | Application | User-facing protocols (HTTP, FTP, DNS…) |
| 6 | Presentation | Data encoding, encryption, compression |
| 5 | Session | Session management between applications |
| 4 | Transport | End-to-end delivery (TCP, UDP) |
| 3 | Network | Logical addressing and routing (IP) |
| 2 | Data Link | MAC addressing, frame delivery (Ethernet) |
| 1 | Physical | Bits over cables, signals |

NetPractice primarily works at **Layer 3 (Network)** — IP addressing and routing.

---

### 🔹 TCP/IP Addressing

**IP (Internet Protocol)** is the addressing system used to identify every device on a network. In this project, we use **IPv4**, which represents addresses as four groups of numbers (octets) separated by dots:

```
192.168.1.10
```

Each octet ranges from `0` to `255`. An IPv4 address has **32 bits** total.

There are two parts to every IP address:
- **Network portion** — identifies the network the device belongs to.
- **Host portion** — identifies the specific device within that network.

The split between these two parts is determined by the **subnet mask**.

---

### 🔹 Subnet Mask

A **subnet mask** tells the network which bits of an IP address belong to the network and which belong to the host. It follows the same dotted-decimal notation as an IP address:

```
255.255.255.0
```

In binary, a subnet mask is always a sequence of `1`s followed by `0`s:
```
11111111.11111111.11111111.00000000
```

The `1` bits cover the **network portion**; the `0` bits cover the **host portion**.

#### CIDR Notation

Subnet masks are often written in **CIDR** (Classless Inter-Domain Routing) notation, which simply counts the number of `1` bits:

| Subnet Mask | CIDR | Available Hosts |
|-------------|------|----------------|
| 255.255.255.0 | /24 | 254 |
| 255.255.255.128 | /25 | 126 |
| 255.255.255.192 | /26 | 62 |
| 255.255.254.0 | /23 | 510 |
| 255.255.0.0 | /16 | 65,534 |

> 💡 Formula: **2ⁿ − 2** hosts per subnet, where *n* is the number of host bits. We subtract 2 for the **network address** (all host bits = 0) and the **broadcast address** (all host bits = 1).

#### Finding the Network Address

Apply a bitwise **AND** between the IP address and the subnet mask:

```
IP:   192.168.1.10  →  11000000.10101000.00000001.00001010
Mask: 255.255.255.0 →  11111111.11111111.11111111.00000000
AND: ─────────────────────────────────────────────────────
Net:  192.168.1.0   →  11000000.10101000.00000001.00000000
```

Two devices can communicate **directly** (without a router) only if they share the same **network address**.

---

### 🔹 Special IP Ranges

Certain address ranges are reserved and cannot be used as regular host addresses:

| Range | Purpose |
|-------|---------|
| `10.0.0.0/8` | Private network |
| `172.16.0.0/12` | Private network |
| `192.168.0.0/16` | Private network |
| `127.0.0.0/8` | Loopback (localhost) |
| `0.0.0.0/0` | Default route (catch-all) |

---

### 🔹 Default Gateway

When a device wants to communicate with a host **outside its own subnet**, it sends the packet to its **default gateway** — typically a router interface on the same subnet as the device.

```
Device A (192.168.1.10/24)  →  Gateway (192.168.1.1)  →  Outside network
```

- The gateway IP must be in the **same subnet** as the device.
- Without a configured gateway, a device cannot reach hosts on other networks.

---

### 🔹 Routers

A **router** operates at Layer 3 and forwards packets between different networks. It has multiple interfaces, each connected to a different subnet and assigned an IP address within that subnet.

Routers use a **routing table** to decide where to forward packets:
- Each entry has a **destination network** and a **next hop** (or exit interface).
- The entry `0.0.0.0/0` is the **default route** — it matches any destination not covered by a more specific rule, acting as a last-resort gateway.

---

### 🔹 Switches

A **switch** operates at Layer 2 (Data Link). It connects multiple devices within the **same network** and forwards frames using MAC addresses. Unlike a router, a switch does not create subnet boundaries — all ports share the same network.

In NetPractice, switches are transparent: they do not require IP configuration themselves.

---

## 🚀 Instructions

### Running the Training Interface

1. Download the project files from the 42 project page.
2. Extract them into any folder.
3. Open a terminal in that folder and run:

```bash
bash run.sh
```

This launches a local web server and opens the interface in your browser automatically.

> **If `run.sh` doesn't work**, you can start the server manually:
> ```bash
> python3 -m http.server 49242
> ```
> Then navigate to `http://localhost:49242` in your browser.

---

### Using the Interface

1. On the welcome screen, select the **Training** tab.
2. Enter your **42 intranet login** in the field provided — this is required so your configuration is personalised.
3. Click **Start!** to begin.

For each level:
- Read the **goal(s)** shown at the top.
- Modify only the **unshaded (editable)** fields.
- Click **[Check again]** to validate your configuration.
- Read the **logs at the bottom** for hints if something is wrong.
- Once the level passes, click **[Get my config]** to download your configuration file **before** clicking Next level.

> ⚠️ **Do not forget to export the config file for every level.** You cannot go back once you advance.

---

### Submitting Your Work

Place the **10 exported configuration files** (one per level) at the **root of your Git repository**, then push as usual. Only files present in your repository will be evaluated.

```
my_netpractice_repo/
├── level1.json
├── level2.json
├── ...
├── level10.json
└── README.md
```

---

### During the Defense

- You will be asked to solve **3 random levels** from scratch, under time pressure.
- External tools are **not allowed** — only a simple calculator (e.g., `bc`) is tolerated.
- Make sure you genuinely understand subnetting: practice until it feels natural.

---

## 📚 Resources

### Networking Concepts Studied

The following topics are required to complete NetPractice:

- **TCP/IP Addressing** — how IPv4 addresses identify devices on a network
- **Subnet Masks** — splitting addresses into network and host portions
- **CIDR Notation** — compact representation of subnet masks
- **Default Gateways** — routing traffic outside a local subnet
- **Routing Tables** — how routers decide where to forward packets
- **Routers** — devices that connect and route between different subnets
- **Switches** — devices that connect hosts within the same subnet
- **OSI Model** — the layered framework for network communication

### Reference Links

| Resource | Description |
|----------|-------------|
| [Cisco — What is an IP Address?](https://www.cisco.com/c/en/us/solutions/small-business/resource-center/networking/what-is-an-ip-address.html) | Official Cisco explanation of IP addressing |
| [Subnet Calculator](https://www.subnet-calculator.com/) | Visual subnet/mask calculator |
| [CIDR to Subnet Mask Converter](https://www.ipaddressguide.com/cidr) | Convert between CIDR and dotted-decimal |
| [Khan Academy — Computer Networks](https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:the-internet) | Beginner-friendly networking introduction |
| [RFC 791 — Internet Protocol](https://tools.ietf.org/html/rfc791) | Original IPv4 specification |
| [lpaube/NetPractice (reference)](https://github.com/lpaube/NetPractice) | Excellent community reference for this project |

### 🤖 AI Usage

**Claude (Anthropic)** was used during this project to:
- Understand and consolidate the **basic networking theory** (IP addressing, subnetting, gateways, OSI model).
- Clarify concepts encountered during exercises (e.g., how routing tables resolve default routes).

All AI-generated explanations were reviewed, tested against the actual exercises, and verified through peer discussion before being considered reliable. No AI tool was used to directly solve or bypass any of the 10 levels.

---

<div align="center">

Made with 🧠 and ☕ at **42**

</div>