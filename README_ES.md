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
