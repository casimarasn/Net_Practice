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


*Este proyecto ha sido creado como parte del currículo de 42 por msedeno-.*

---

<div align="center">

# 🌐 NetPractice

![42 Badge](https://img.shields.io/badge/42-NetPractice-00babc?style=for-the-badge&logo=42&logoColor=white)
![Status](https://img.shields.io/badge/Estado-Completado-brightgreen?style=for-the-badge)
![Language](https://img.shields.io/badge/Tema-Redes-blue?style=for-the-badge&logo=cisco&logoColor=white)

*Una introducción práctica a las redes informáticas y el direccionamiento TCP/IP.*

[🇬🇧 English](README.md) | 🇪🇸 Español

</div>

---

## 📋 Descripción

**NetPractice** es un ejercicio práctico del currículo de 42 diseñado para introducir a los estudiantes en los fundamentos de las **redes informáticas**. A través de una interfaz de entrenamiento en el navegador, se configuran redes simuladas a pequeña escala asignando correctamente direcciones IP, máscaras de subred y puertas de enlace predeterminadas para que todos los dispositivos puedan comunicarse según los objetivos establecidos.

El proyecto consta de **10 niveles** de complejidad creciente. Cada nivel muestra un diagrama de red defectuoso y uno o más objetivos de comunicación. El estudiante debe modificar los campos editables hasta que la red funcione correctamente, y luego exportar la configuración para su entrega.

> ⚠️ Todas las redes de este proyecto son **simuladas** y no tienen conexión con infraestructura real.

---

## 🧠 Teoría de Redes

Comprender los siguientes conceptos es **esencial** para completar este proyecto.

---

### 🔹 El Modelo OSI

El **modelo OSI (Open Systems Interconnection)** es un marco conceptual que divide la comunicación en red en **7 capas**:

| Capa | Nombre | Función |
|------|--------|---------|
| 7 | Aplicación | Protocolos de usuario (HTTP, FTP, DNS…) |
| 6 | Presentación | Codificación, cifrado, compresión |
| 5 | Sesión | Gestión de sesiones entre aplicaciones |
| 4 | Transporte | Entrega extremo a extremo (TCP, UDP) |
| 3 | Red | Direccionamiento lógico y enrutamiento (IP) |
| 2 | Enlace de datos | Direcciones MAC, entrega de tramas (Ethernet) |
| 1 | Física | Bits por cables y señales |

NetPractice trabaja principalmente en la **Capa 3 (Red)** — direccionamiento IP y enrutamiento.

---

### 🔹 Direccionamiento TCP/IP

El **protocolo IP (Internet Protocol)** es el sistema de direccionamiento que identifica cada dispositivo en una red. En este proyecto se usa **IPv4**, que representa las direcciones como cuatro grupos de números (octetos) separados por puntos:

```
192.168.1.10
```

Cada octeto va de `0` a `255`. Una dirección IPv4 tiene **32 bits** en total.

Toda dirección IP tiene dos partes:
- **Parte de red** — identifica la red a la que pertenece el dispositivo.
- **Parte de host** — identifica el dispositivo concreto dentro de esa red.

La división entre ambas partes la determina la **máscara de subred**.

---

### 🔹 Máscara de Subred

Una **máscara de subred** indica qué bits de una dirección IP pertenecen a la red y cuáles al host. Usa la misma notación decimal con puntos que una dirección IP:

```
255.255.255.0
```

En binario, una máscara de subred es siempre una secuencia de `1`s seguida de `0`s:
```
11111111.11111111.11111111.00000000
```

Los bits `1` cubren la **parte de red**; los bits `0` cubren la **parte de host**.

#### Notación CIDR

Las máscaras de subred también se expresan en notación **CIDR** (Classless Inter-Domain Routing), que simplemente cuenta los bits `1`:

| Máscara de subred | CIDR | Hosts disponibles |
|-------------------|------|-------------------|
| 255.255.255.0 | /24 | 254 |
| 255.255.255.128 | /25 | 126 |
| 255.255.255.192 | /26 | 62 |
| 255.255.254.0 | /23 | 510 |
| 255.255.0.0 | /16 | 65.534 |

> 💡 Fórmula: **2ⁿ − 2** hosts por subred, donde *n* es el número de bits de host. Se restan 2 por la **dirección de red** (todos los bits de host = 0) y la **dirección de broadcast** (todos los bits de host = 1).

#### Calcular la Dirección de Red

Se aplica una operación **AND bit a bit** entre la dirección IP y la máscara de subred:

```
IP:    192.168.1.10  →  11000000.10101000.00000001.00001010
Másc:  255.255.255.0 →  11111111.11111111.11111111.00000000
AND:  ────────────────────────────────────────────────────
Red:   192.168.1.0   →  11000000.10101000.00000001.00000000
```

Dos dispositivos pueden comunicarse **directamente** (sin router) solo si comparten la misma **dirección de red**.

---

### 🔹 Rangos de IP Especiales

Ciertos rangos de direcciones están reservados y no pueden usarse como direcciones de host normales:

| Rango | Uso |
|-------|-----|
| `10.0.0.0/8` | Red privada |
| `172.16.0.0/12` | Red privada |
| `192.168.0.0/16` | Red privada |
| `127.0.0.0/8` | Loopback (localhost) |
| `0.0.0.0/0` | Ruta por defecto (catch-all) |

---

### 🔹 Puerta de Enlace Predeterminada (Default Gateway)

Cuando un dispositivo quiere comunicarse con un host **fuera de su propia subred**, envía el paquete a su **puerta de enlace predeterminada** — normalmente una interfaz del router que está en la misma subred que el dispositivo.

```
Dispositivo A (192.168.1.10/24)  →  Gateway (192.168.1.1)  →  Red exterior
```

- La IP del gateway debe estar en la **misma subred** que el dispositivo.
- Sin un gateway configurado, un dispositivo no puede alcanzar hosts en otras redes.

---

### 🔹 Routers (Enrutadores)

Un **router** opera en la Capa 3 y reenvía paquetes entre distintas redes. Tiene múltiples interfaces, cada una conectada a una subred diferente y con una IP asignada dentro de esa subred.

Los routers usan una **tabla de enrutamiento** para decidir adónde reenviar los paquetes:
- Cada entrada tiene una **red de destino** y un **siguiente salto** (o interfaz de salida).
- La entrada `0.0.0.0/0` es la **ruta por defecto** — coincide con cualquier destino no cubierto por una regla más específica, actuando como última opción.

---

### 🔹 Switches (Conmutadores)

Un **switch** opera en la Capa 2 (Enlace de datos). Conecta múltiples dispositivos dentro de la **misma red** y reenvía tramas usando direcciones MAC. A diferencia de un router, un switch no crea fronteras entre subredes — todos sus puertos comparten la misma red.

En NetPractice, los switches son transparentes: no requieren configuración IP propia.

---

## 🚀 Instrucciones

### Ejecutar la Interfaz de Entrenamiento

1. Descarga los archivos del proyecto desde la página del proyecto en 42.
2. Extrae los archivos en cualquier carpeta.
3. Abre una terminal en esa carpeta y ejecuta:

```bash
bash run.sh
```

Esto lanza un servidor web local y abre la interfaz en tu navegador automáticamente.

> **Si `run.sh` no funciona**, puedes iniciar el servidor manualmente:
> ```bash
> python3 -m http.server 49242
> ```
> Luego navega a `http://localhost:49242` en tu navegador.

---

### Usar la Interfaz

1. En la pantalla de bienvenida, selecciona la pestaña **Training**.
2. Introduce tu **login de la intranet de 42** en el campo — esto es obligatorio para que tu configuración sea personalizada.
3. Haz clic en **Start!** para comenzar.

Para cada nivel:
- Lee el/los **objetivo(s)** que aparecen arriba.
- Modifica únicamente los campos **sin sombrear (editables)**.
- Haz clic en **[Check again]** para validar tu configuración.
- Lee los **registros (logs) en la parte inferior** para pistas si algo falla.
- Una vez que el nivel se apruebe, haz clic en **[Get my config]** para descargar el archivo de configuración **antes** de pasar al siguiente nivel.

> ⚠️ **No olvides exportar el archivo de configuración de cada nivel.** Una vez que avances no podrás volver atrás.

---

### Entrega del Proyecto

Coloca los **10 archivos de configuración exportados** (uno por nivel) en la **raíz de tu repositorio Git** y haz push como de costumbre. Solo los archivos presentes en tu repositorio serán evaluados.

```
mi_repositorio_netpractice/
├── level1.json
├── level2.json
├── ...
├── level10.json
└── README.md
```

---

### Durante la Defensa

- Se te pedirá resolver **3 niveles aleatorios** desde cero, con límite de tiempo.
- Las herramientas externas **no están permitidas** — solo se tolera una calculadora simple (p. ej., `bc`).
- Asegúrate de entender genuinamente el subnetting: practica hasta que te resulte natural.

---

## 📚 Recursos

### Conceptos de Redes Estudiados

Los siguientes temas son necesarios para completar NetPractice:

- **Direccionamiento TCP/IP** — cómo las direcciones IPv4 identifican dispositivos en una red
- **Máscaras de subred** — división de direcciones en parte de red y parte de host
- **Notación CIDR** — representación compacta de las máscaras de subred
- **Puertas de enlace predeterminadas** — enrutar el tráfico fuera de la subred local
- **Tablas de enrutamiento** — cómo los routers deciden adónde reenviar paquetes
- **Routers** — dispositivos que conectan y enrutan entre diferentes subredes
- **Switches** — dispositivos que conectan hosts dentro de la misma subred
- **Modelo OSI** — el marco por capas para la comunicación en red

### Enlaces de Referencia

| Recurso | Descripción |
|---------|-------------|
| [Cisco — ¿Qué es una dirección IP?](https://www.cisco.com/c/en/us/solutions/small-business/resource-center/networking/what-is-an-ip-address.html) | Explicación oficial de Cisco sobre el direccionamiento IP |
| [Calculadora de Subnets](https://www.subnet-calculator.com/) | Calculadora visual de subredes y máscaras |
| [Conversor CIDR a Máscara](https://www.ipaddressguide.com/cidr) | Convierte entre CIDR y decimal con puntos |
| [Khan Academy — Redes](https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:the-internet) | Introducción a redes apta para principiantes |
| [RFC 791 — Internet Protocol](https://tools.ietf.org/html/rfc791) | Especificación original de IPv4 |
| [lpaube/NetPractice (referencia)](https://github.com/lpaube/NetPractice) | Excelente referencia de la comunidad para este proyecto |

### 🤖 Uso de IA

Se utilizó **Claude (Anthropic)** durante este proyecto para:
- Entender y consolidar la **teoría básica de redes** (direccionamiento IP, subnetting, puertas de enlace, modelo OSI).
- Aclarar conceptos encontrados durante los ejercicios (p. ej., cómo las tablas de enrutamiento resuelven rutas por defecto).

Todas las explicaciones generadas por IA fueron revisadas, contrastadas con los ejercicios reales y verificadas mediante discusión con compañeros antes de considerarse fiables. Ninguna herramienta de IA fue utilizada para resolver o eludir directamente ninguno de los 10 niveles.

---

<div align="center">

Hecho con 🧠 y ☕ en **42**

</div>
