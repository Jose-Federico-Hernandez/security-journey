## THM SEARCH SKILLS (CIBERSECURITY 101)

En este curso veremos ejemplos de sitios web populares que sirven para obtener información para varios propósitos en ciberseguridad.

### Shodan 
  Shodan se descibre usualmente como un motor de búsqueda para el Internet de las Cosas (IoT), pero esta definición se queda corta. Shodan contínuamente escanea internet buscando equipos de red, sistemas de control
  industrial, cámaras de tráfico y prácticamente cualquier dispositivo con una conexción pública a la red, para identificar qué está funcionando y dónde.

   Por ejemplo, buscar `apache 2.4.1` devolverá una lista de servidores que anuncian esa versión en sus cabeceras HTTP, organizada por país, organización y puerto. Durante un pentest o una evaluación de vulnerabilidades,
  este tipo de visibilidad es extremadamente útil, especialmente cuando se combina con un CVE* conocido que afecte a esa versión.

  Shodan también admite sus propios filtros de búsqueda, que permiten reducir significativamente los resultados:
  | Filtro | Descripción | Ejemplo |
  |---|---|---|
  | `country` | Restringe los resultados a un código de país específico. | `country:IE` |
  | `port` | Filtra por un puerto específico o un rango de puertos. | `port:22` |
  | `org` | Limita los resultados a una organización concreta o a un identificador ASN (quién posee un rango de direcciones IP). | `AS7224` (Amazon Web Services) |
  | `hostname` | Busca coincidencias con un hostname o dominio específico. | `hostname:fakebank.thm` |

  *CVE: Un CVE (Common Vulnerabilities and Exposures) es un identificador estándar para vulnerabilidades de seguridad conocidas públicamente. (Lo veremos un poco más adelante)

### VirusTotal

VirusTotal recopila los resultados de más de 70 motores antivirus y escáneres de sitios web en una única interfaz. Puedes enviar un archivo, una URL, un dominio o el hash de un archivo. VirusTotal te indicará
si alguno de esos motores lo ha marcado como malicioso o no.

Aunque no es infalible, VirusTotal es un recurso muy popular dentro de la comunidad Blue Team para obtener un consenso general sobre archivos y enlaces sospechosos, además de recopilar inteligencia sobre nuevas
amenazas en circulación.

### CVE (Common Vulnerabilities and Exposures)

El programa Common Vulnerabilities and Exposures (CVE) es lo más parecido que tiene la industria a un diccionario universal de vulnerabilidades conocidas.

Cada vulnerabilidad confirmada recibe un identificador único con el formato `CVE-AÑO-NÚMERO`, como por ejemplo `CVE-2025-55182`. Si la vulnerabilidad es lo suficientemente relevante, incluso puede recibir un nombre popular. Puede que hayas oído hablar de vulnerabilidades como *Heartbleed*, *React2Shell* y *Log4Shell*.

Estas vulnerabilidades reciben una puntuación (CVSS) basada en diversos factores, tales como:

- **Impacto** – ¿Qué daños puede causar esta vulnerabilidad?
- **Complejidad** – ¿Es fácil de explotar o no?
- **Disponibilidad** – ¿Qué probabilidad hay de que alguien pueda explotarla?

Las organizaciones utilizan este tipo de puntuaciones para priorizar su nivel de riesgo, abordando primero las vulnerabilidades con mayor puntuación.

Estos identificadores funcionan como un punto de referencia común entre fabricantes, investigadores, herramientas de seguridad y documentación, garantizando que todos se refieran a la misma vulnerabilidad cuando la discuten.

Sitios web como ExploitDB recopilan esta información junto con los llamados "Proof of Concepts" (PoCs), que son scripts capaces de demostrar la vulnerabilidad.

### Documentación de Productos y Herramientas

Cada herramienta o plataforma de seguridad importante proporciona su propia documentación, la cual suele ser más fiable y estar más actualizada que cualquier tutorial de terceros.

Cuando estés solucionando comportamientos inesperados o intentando comprender cómo utilizar una herramienta de una manera específica, la documentación oficial siempre debería ser tu primera opción, no la última.

#### Páginas MAN de Linux

¿Alguna vez te has encontrado con una herramienta o comando de línea de comandos que no conocías? Las páginas MANual de Linux están para ayudarte. Estas páginas sirven como documentación que puedes consultar directamente desde tu terminal sobre cualquier comando de Linux y sobre la mayoría de herramientas de ciberseguridad.

Para ver la página del manual, ejecuta:

```bash
man <comando>
```

Por ejemplo: 

```bash
user@thm:~$ man nc

NC(1)                     User Commands                NC(1)

NAME
  nc - arbitrary TCP and UDP connections and listens

SYNOPSIS
  nc [-46DdhklnrStUuvz] [-i interval] [-p source_port] [-s source_ip_address]
  [-T ToS] [-w timeout] [-X proxy_protocol] [-x proxy_address[:port]]
  [hostname] [port[s]]

DESCRIPTION
  The nc (or netcat) utility is used for just about anything under the sun involving TCP or UDP. It can open TCP connections, send UDP packets, listen on arbitrary TCP and UDP ports, do port scanning, and deal with both IPv4 and IPv6. Unlike telnet(1), nc scripts nicely, and separates error messages onto standard error instead of sending them to standard output, as telnet(1) does with some.

Common uses include:

· simple TCP proxies
· shell-script based HTTP clients and servers
· network daemon testing
· a SOCKS or HTTP ProxyCommand for ssh(1)
· and much, much more
OPTIONS
  -4
    Forces nc to use IPv4 addresses only.

  -6
    Forces nc to use IPv6 addresses only.

  -D
    Enable debugging on the socket.

  -d
    Do not attempt to read from stdin.

  -h
    Prints out nc help.

  -i interval
    Specifies a delay time interval between lines of text sent and received. Also causes a delay time between connections to multiple ports.

  -k
    Forces nc to stay listening for another connection after its current connection is completed. It is an error to use this option without the -l option.

  -l
    Used to specify that nc should listen for an incoming connection rather than initiate a connection to a remote host. It is an error to use this option in conjunction with the -p, -s, or -z options. Additionally, any timeouts specified with the -w option are ignored.

  -n
    Do not do any DNS or service lookups on any specified addresses, hostnames or ports.

  -p source_port
    Specifies the source port nc should use, subject to privilege restrictions and availability.

  -r
    Specifies that source and/or destination ports should be chosen randomly instead of sequentially within a range or in the order that the system assigns them.

  -s source_ip_address
    Specifies the IP of the interface which is used to send the packets. For UNIX-domain datagram sockets, specifies the local temporary socket file to create and use so that datagrams can be received. It is an error to use this option in conjunction with the -l option.

  -T ToS
    Specifies IP Type of Service (ToS) for the connection. Valid values are the tokens "lowdelay", "throughput", "reliability", or an 8-bit hexadecimal value preceded by "0x".

  -t
    Causes nc to send RFC 854 DON'T and WON'T responses to RFC 854 DO and WILL requests. This makes it possible to use nc to script telnet sessions.

  -U
    Specifies to use UNIX-domain sockets.

  -u
    Use UDP instead of the default option of TCP.

  -v
    Have nc give more verbose output.

  -w timeout
    If a connection and stdin are idle for more than timeout seconds, then the connection is silently closed. The -w flag has no effect on the -l option, i.e. nc will listen forever for a connection, with or without the -w flag. The default is no timeout.

  -X proxy_protocol
    Requests that nc should use the specified protocol when talking to the proxy server. Supported protocols are "4" (SOCKS v.4), "5" (SOCKS v.5) and "connect" (HTTPS proxy). If the protocol is not specified, SOCKS version 5 is used.

  -x proxy_address[:port]
    Requests that nc should connect to hostname using a proxy at proxy_address and port. If port is not specified, the well-known port for the proxy protocol is used (1080 for SOCKS, 3128 for HTTPS).

  -z
    Specifies that nc should just scan for listening daemons, without sending any data to them. It is an error to use this option in conjunction with the -l option.
```

### GitHub y Vulnerabilidades

GitHub puede ser un gran recurso para mantenerse actualizado sobre las últimas amenazas y vulnerabilidades. Los investigadores suelen publicar allí código de prueba de concepto (PoC), herramientas de explotación 
e informes técnicos detallados, normalmente más rápido que los canales oficiales.

Buscar directamente un identificador CVE (por ejemplo, `CVE-2026-1337`) en GitHub suele revelar repositorios que contienen código PoC, scripts de escaneo o análisis detallados de la vulnerabilidad.

Dicho esto, no todos los PoCs son igual de fiables. Algunos están incompletos, otros están intencionadamente defectuosos y, en ocasiones, un repositorio etiquetado como "PoC" puede ser malicioso en sí mismo. 
Verifica siempre aquello que estés a punto de ejecutar.
