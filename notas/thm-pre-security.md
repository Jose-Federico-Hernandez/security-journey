# THM — Pre Security

Notas del path Pre Security de TryHackMe.
Combinadas con el módulo de Sistemas Informáticos (iLERNA DAW).

---

## 1. Network Fundamentals

### ¿Qué es una red?
- Una red es dos o más dispositivos conectados que pueden comunicarse entre sí.
- Internet es una red gigante de redes interconectadas.
- Las redes pueden ser privadas (LAN en casa o empresa) o públicas (Internet).

### Tipos de red
| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| LAN | Red de área local, corto alcance | Red de casa, oficina |
| WAN | Red de área amplia, largo alcance | Internet |
| WLAN | LAN inalámbrica | WiFi |
| VPN | Red privada virtual sobre Internet | Acceso remoto seguro |

### Direcciones IP
- Una IP identifica un dispositivo en una red.
- **IPv4:** 4 bloques de 0–255 separados por puntos → `192.168.1.1`
- **IPv6:** 128 bits en hexadecimal → `2001:0db8:85a3::8a2e:0370:7334`
- **IP pública:** visible desde Internet, asignada por el ISP.
- **IP privada:** solo dentro de la red local (rangos reservados):
  - `10.0.0.0 – 10.255.255.255`
  - `172.16.0.0 – 172.31.255.255`
  - `192.168.0.0 – 192.168.255.255`

### Subnetting básico
- Una subred divide una red grande en redes más pequeñas.
- La máscara de subred indica qué parte de la IP es red y qué parte es host.
  - `255.255.255.0` → `/24` → 254 hosts disponibles
  - `255.255.0.0` → `/16` → 65534 hosts disponibles
- **CIDR:** notación compacta → `192.168.1.0/24`

### MAC Address
- Identificador físico único de cada tarjeta de red.
- 48 bits en hexadecimal → `a4:c3:f0:85:ac:2d`
- Los primeros 3 pares identifican al fabricante (OUI).
- Opera en la capa 2 (Enlace de datos) del modelo OSI.
- **No es enrutable** — solo funciona dentro de la misma red local.

### ARP (Address Resolution Protocol)
- Traduce una IP conocida a su MAC address correspondiente.
- Proceso:
  1. El dispositivo hace un broadcast: "¿Quién tiene la IP `192.168.1.5`?"
  2. El dispositivo con esa IP responde con su MAC.
  3. La asociación IP→MAC se guarda en la tabla ARP local.
- Ver tabla ARP en Windows: `arp -a`
- Ver tabla ARP en Linux: `arp -n`

### Modelo OSI (7 capas)
| Capa | Nombre | Función principal | Ejemplo |
|------|--------|-------------------|---------|
| 7 | Aplicación | Interfaz con el usuario | HTTP, DNS, FTP |
| 6 | Presentación | Formato, cifrado | SSL/TLS, JPEG |
| 5 | Sesión | Gestión de sesiones | NetBIOS |
| 4 | Transporte | Entrega extremo a extremo | TCP, UDP |
| 3 | Red | Enrutamiento | IP, ICMP |
| 2 | Enlace de datos | Acceso al medio físico | Ethernet, MAC |
| 1 | Física | Bits por el cable | Cable, señal |

> Truco para recordar las capas (de 7 a 1): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

### Modelo TCP/IP (4 capas)
| Capa TCP/IP | Equivalente OSI | Protocolos |
|-------------|-----------------|------------|
| Aplicación | 5, 6, 7 | HTTP, DNS, FTP, SMTP |
| Transporte | 4 | TCP, UDP |
| Internet | 3 | IP, ICMP, ARP |
| Acceso a red | 1, 2 | Ethernet, WiFi |

### TCP vs UDP
| | TCP | UDP |
|-|-----|-----|
| Conexión | Orientado a conexión (handshake) | Sin conexión |
| Fiabilidad | Garantiza entrega y orden | No garantiza nada |
| Velocidad | Más lento | Más rápido |
| Uso | Web, email, SSH | Streaming, DNS, VoIP |

**Three-way handshake TCP:**
1. Cliente → Servidor: `SYN`
2. Servidor → Cliente: `SYN-ACK`
3. Cliente → Servidor: `ACK`

### Puertos importantes
| Puerto | Protocolo | Servicio |
|--------|-----------|---------|
| 21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |

### Packets y Frames
- **Frame:** unidad de datos en capa 2 (incluye MAC origen/destino).
- **Packet:** unidad de datos en capa 3 (incluye IP origen/destino).
- Los datos se encapsulan añadiendo cabeceras en cada capa al bajar.
- Se desencapsulan quitando cabeceras en cada capa al subir.

### Extending Your Network
- **Router:** conecta redes distintas, enruta paquetes por IP.
- **Switch:** conecta dispositivos dentro de la misma red, trabaja con MACs.
- **Firewall:** controla el tráfico según reglas (entrada/salida).
- **NAT:** traduce IPs privadas a una IP pública para salir a Internet.
- **DHCP:** asigna IPs automáticamente a los dispositivos de la red.

---

## 2. How The Web Works

### DNS (Domain Name System)
- Traduce nombres de dominio (`google.com`) a IPs (`142.250.200.46`).
- Jerarquía DNS:
  1. **Root DNS** (`.`)
  2. **TLD** (`.com`, `.es`, `.org`)
  3. **Authoritative nameserver** (el del dominio en cuestión)

**Proceso de resolución:**
1. El navegador comprueba su caché local.
2. Si no tiene, pregunta al resolver del ISP.
3. El resolver pregunta al root → TLD → authoritative.
4. Recibe la IP y la devuelve al navegador.
5. La respuesta se cachea por el tiempo indicado (TTL).

**Tipos de registros DNS:**
| Registro | Función |
|----------|---------|
| A | Dominio → IPv4 |
| AAAA | Dominio → IPv6 |
| CNAME | Alias de otro dominio |
| MX | Servidor de correo |
| TXT | Texto libre (verificación, SPF) |

### HTTP e HTTPS
- **HTTP** (HyperText Transfer Protocol): protocolo de comunicación web, puerto 80.
- **HTTPS**: HTTP con cifrado TLS/SSL, puerto 443.

**Métodos HTTP:**
| Método | Uso |
|--------|-----|
| GET | Solicitar un recurso |
| POST | Enviar datos al servidor |
| PUT | Actualizar un recurso completo |
| DELETE | Eliminar un recurso |
| PATCH | Actualizar parte de un recurso |

**Códigos de respuesta HTTP:**
| Rango | Significado | Ejemplos |
|-------|-------------|---------|
| 2xx | Éxito | 200 OK, 201 Created |
| 3xx | Redirección | 301 Moved, 302 Found |
| 4xx | Error del cliente | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5xx | Error del servidor | 500 Internal Server Error |

### Cómo funciona una web request completa
1. Usuario escribe `https://example.com` en el navegador.
2. DNS resuelve `example.com` → IP.
3. El navegador abre una conexión TCP al servidor (puerto 443).
4. Se realiza el handshake TLS (cifrado).
5. El navegador envía: `GET / HTTP/1.1`
6. El servidor responde con el HTML (código 200 OK).
7. El navegador renderiza la página.

### Cómo se construye una web
- **HTML:** estructura y contenido.
- **CSS:** estilos y diseño visual.
- **JavaScript:** comportamiento e interactividad.
- El servidor puede ser **estático** (sirve archivos) o **dinámico** (genera contenido con lógica de backend).

---

## 3. Computer Fundamentals

### Componentes de un ordenador
| Componente | Función |
|-----------|---------|
| CPU | Procesa instrucciones |
| RAM | Memoria temporal de trabajo |
| Almacenamiento (HDD/SSD) | Guarda datos de forma permanente |
| GPU | Procesamiento gráfico |
| Placa base | Conecta todos los componentes |
| PSU | Suministra energía |

### Modelo Cliente-Servidor
- **Cliente:** solicita un servicio o recurso (navegador, app).
- **Servidor:** responde a las solicitudes (web server, DB server).
- Comunicación mediante protocolos (HTTP, FTP, SSH...).
- El servidor escucha en un puerto específico esperando conexiones.

### Virtualización
- Permite ejecutar varios sistemas operativos en una sola máquina física.
- El **hipervisor** gestiona las máquinas virtuales (VMs).
  - **Tipo 1 (bare metal):** corre directamente sobre hardware → VMware ESXi, Hyper-V.
  - **Tipo 2 (hosted):** corre sobre un SO anfitrión → VirtualBox, VMware Workstation.
- En seguridad: fundamental para crear laboratorios aislados sin riesgo.
- Mi setup: VirtualBox sobre Windows 11, con Kali Linux 2026.1 como VM.

### Cloud Computing *(15/5/2026)*
-  Modelos de servicio: IaaS, PaaS, SaaS
-  Modelos de despliegue: pública, privada, híbrida
-  Proveedores principales: AWS, Azure, GCP
Terminologia:
- Public Cloud: Servicios cloud a los que accedes a través de internet y que son compartidos por muchas personas y empresas.
- Private Cloud: Una nube construida exclusivamente para una sola empresa, de forma que tenga mayor control y seguridad.
- Hybrid Cloud: Una combinación de nube pública y privada que pueden trabajar juntas y compartir datos.
- IaaS (Infrastructure as a Service): Un servicio donde alquilas componentes básicos de computación, como servidores y almacenamiento, desde la nube.
- PaaS (Platform as a Service): Un servicio que proporciona un entorno listo para usar para desarrollar y ejecutar aplicaciones sin tener que administrar servidores.
- SaaS (Software as a Service): Software que utilizas online sin instalar nada, como Gmail o Zoom.
- EC2: Los ordenadores cloud de Amazon que puedes crear, usar y redimensionar rápidamente cuando los necesites.

---

## 4. Operating Systems Basics *(15/5/2026)*

- Operating Systems: Introduction {
  Sistema operativo (OS): El software principal que administra el hardware, las aplicaciones y todos los recursos del sistema.
  Kernel space: La zona altamente privilegiada del sistema operativo con acceso directo al hardware, y donde reside el kernel, que administra directamente el hardware y los recursos del sistema.
  User space: La zona donde se ejecutan las aplicaciones normales con permisos limitados para garantizar la seguridad y estabilidad del sistema.
  Interfaz gráfica de usuario (GUI): La parte visual del sistema operativo —ventanas, iconos y menús— que te permite interactuar mediante clics y toques.
  Interfaz de línea de comandos (CLI): Una interfaz basada en texto donde escribes comandos para controlar el sistema con precisión y rapidez.
 }
- [ ] Windows Basics
- [ ] Linux CLI Basics
- [ ] Windows CLI Basics
- [ ] Operating System Security

---

## 5. Software Basics *(pendiente)*

- [ ] Data Representation
- [ ] Data Encoding
- [ ] Python: Simple Demo
- [ ] JavaScript: Simple Demo
- [ ] Database SQL Basics

---

## 6. Attacks and Defenses *(pendiente)*

- [ ] The CIA Triad
- [ ] Cryptography Concepts
- [ ] Become a Hacker
- [ ] Become a Defender

---

## Log de progreso

| Fecha | Módulo completado |
|-------|------------------|
| 2026-05 | What is Networking |
| 2026-05 | Intro to LAN |
| 2026-05 | OSI Model |
| 2026-05 | Packets & Frames |
| 2026-05 | Extending Your Network |
| 2026-05 | DNS in Detail |
| 2026-05 | HTTP in Detail |
| 2026-05 | How Websites Work |
| 2026-05 | Putting it all together |
| 2026-05 | Inside a Computer System |
| 2026-05 | Computer Types |
| 2026-05 | Client-Server Basics |
| 2026-05 | Virtualisation Basics |
