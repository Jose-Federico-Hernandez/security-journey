# Linux Fundamentals THM

"Linux" es un término paraguas para referirse a todos los SO's basados en Unix. Linux es open-source y sus distribuciones más famosas son Debian y Ubuntu.
En esta room vamos a desplegar una MV de Linux y completar las tareas.
Una de las ventajas de Linux es que es muy poco pesado y necesita de muy pocos recursos para funcionar. Tan es así que algunas distribuciones ni siquiera
tienen interfaz gráfica. El usuario se comunica con el SO desde la **terminal**.

```bash
tryhackme@linux1:~$ aquí van los comandos
```
| Comando | Descripcion |
| ------- | ----------- |
| echo | Muestra por consola el texto que demos |
| whoami | Muestra con qué usuario estamos logeados | 
| ls | listar |
| cd | cambiar directorio |
| cat | concatenar (Mostrar el contenido de un archivo) |
| pwd | print working directory (en que directorio trabajamos) |
| find -name <nombre_archivo> | busca en todos los directorios |
| grep | buscar dentro del contenido de un archivo valores especificos |
| grep -R | busqueda recursiva (directorio y subdirectorio) |

## Shell Operators

| Operator | Descripcion |
| -------- | ----------- |
| & | Te permite ejecutar un comando en segundo plano, liberando la terminal mientras este se ejecuta |
| && | Te permite combinar varios comandos en una sola linea en terminal |
| > | Es un redireccionador. Normalmente la salida de la ejecución de un comando es la pantalla, este operador intercepta la salida y la manda a un archivo |
| >> | Es igual que > pero > sobreescribe cada vez que se usa y >> no |

Fin parte 1

Para el inicio de la segunda parte hemos hecho una conexion con ssh para acceder a la room. El usuario era tryhackme y la IP la conseguíamos en una cabecera al iniciar la MV.
La sintaxis:

```bash
ssh tryhackme@<IP_aqui>
```

Esto nos conecta con el usuario tryhackme. Al logear use whoami para saber que había sido exitoso.

## Flags y Switches

Son modificadores para los comandos. (Anteriormente vimos -R en grep para recursividad) En el ejemplo se usa ls -a (contraccion para -all) para que liste archivos ocultos.
Los comandos que aceptan esto, tienen también una opcion --help, que lista las posibles opciones que acepta el comando.Como vimos anteriormente, también podemos usar man ls" para dirigirnos al manual


## Interacción de archivos

Veamos unos comandos que nos permitirán crear, mover y eliminar archivos y carpetas.

| Comando | Nombre Completo | Uso |
| ------- | --------------- | --- |
| touch | touch | crear archivo |
| mkdir | make directory | crear una carpeta |
| cp | copy | copia un archivo o carpeta |
| mv | move | mueve un archivo o carpeta |
| rm / rm -R | remove | elimina un archivo / carpeta |
| file | file | determina el tipo de un archivo | 

## Permisos

```bash
ls -lh
```

Con esa flag del comando ls, podemos ver los permisos de todos los archivos del directorio. Un archivo o carpeta puede tener varias características que determinan tanto qué acciones están permitidas como qué usuarios 
o grupos puede realizarlas. Por ejemplo:

- Leer (Read)
- Escribit (Write)
- Ejecutar (Execute)

# Diferencias entre Usuarios y Grupos

Lo bueno de Linux es que los permisos pueden ser extremadamente granulares. Aunque un usuario sea técnicamente el propietario de un archivo, si los permisos están configurados adecuadamente, un grupo de usuarios puede tener el mismo conjunto de permisos —o uno diferente— sobre ese mismo archivo sin afectar al propietario.

Llevémoslo a un contexto real: el usuario del sistema que ejecuta un servidor web necesita permisos para leer y escribir archivos con el fin de que la aplicación web funcione correctamente. Sin embargo, empresas como los proveedores de hosting web también necesitan permitir que sus clientes suban archivos para sus sitios sin convertirlos en el usuario del sistema del servidor web, ya que eso comprometería la seguridad de los demás clientes.

Más abajo aprenderemos los comandos necesarios para cambiar entre usuarios.



## Cambiar Entre Usuarios

Cambiar de usuario en una instalación Linux es sencillo gracias al comando `su`. A menos que seas el usuario `root` (o estés usando permisos de root mediante `sudo`), necesitas saber dos cosas para cambiar de cuenta:

- El usuario al que quieres cambiar
- La contraseña de ese usuario

El comando `su` acepta varios switches que pueden ser útiles. Por ejemplo, ejecutar un comando al iniciar sesión o especificar una shell concreta. Te recomiendo leer la página del manual (`man su`) para conocer más detalles. Sin embargo, aquí veremos el switch `-l` o `--login`.

En pocas palabras, al usar `-l` con `su`, iniciamos una shell mucho más parecida a un inicio de sesión real del usuario: heredamos más propiedades del nuevo usuario, como variables de entorno y similares.

### Uso de `su` para cambiar interactivamente al usuario `user2`

```bash
tryhackme@linux2:~$ su user2
Password:
user2@linux2:/home/tryhackme$
```

Por ejemplo, al usar `su` para cambiar a `user2`, la nueva sesión nos deja en el directorio personal del usuario anterior.

## Uso de `su -l` para cambiar interactivamente al usuario `user2`

```bash
tryhackme@linux2:~$ su -l user2
Password:
user2@linux2:~$ pwd
user2@:/home/user2$
```

Ahora, después de usar `-l`, la nueva sesión nos lleva automáticamente al directorio personal de `user2`.


## Entendiendo los Permisos de Archivos en Formato Numérico

En Linux, cada archivo y directorio tiene un conjunto de permisos que controla quién puede leerlo, escribirlo o ejecutarlo. Estos permisos suelen mostrarse en formato simbólico, como:

```text
rwxrwxrwx
```

Este formato se divide en tres grupos:

| Sección | Aplica a | Ejemplo |
|---|---|---|
| Primeros 3 | Propietario | rwx |
| Siguientes 3 | Grupo | rwx |
| Últimos 3 | Otros | rwx |

Cada letra representa un permiso específico:

- `r` = lectura (read)
- `w` = escritura (write)
- `x` = ejecución (execute)


## Convertir Permisos Simbólicos a Números

Cada permiso tiene un valor numérico:

| Permiso | Valor |
|---|---|
| Lectura (`r`) | 4 |
| Escritura (`w`) | 2 |
| Ejecución (`x`) | 1 |

Para calcular el valor numérico, sumamos los valores de cada grupo.

## Ejemplo: `rwxrwxrwx`

| Grupo | Permisos | Cálculo | Valor |
|---|---|---|---|
| Propietario | rwx | 4 + 2 + 1 | 7 |
| Grupo | rwx | 4 + 2 + 1 | 7 |
| Otros | rwx | 4 + 2 + 1 | 7 |

Por tanto:

```text
rwxrwxrwx = 777
```


## Ejemplos Más Comunes

| Simbólico | Numérico | Significado |
|---|---|---|
| rwxr-xr-x | 755 | El propietario puede hacer todo; los demás pueden leer y ejecutar |
| rw-r--r-- | 644 | El propietario puede leer/escribir; los demás solo leer |
| rwx------ | 700 | Solo el propietario tiene acceso |


## ¿Por Qué Es Importante?

Entender los permisos numéricos es importante porque:

- Muchos comandos de Linux usan valores numéricos (por ejemplo: `chmod 755 archivo`)
- Puedes identificar rápidamente riesgos de seguridad
- Puedes controlar quién accede a archivos sensibles

Por ejemplo:

```bash
chmod 750 system_overview.txt
```

Esto significa:

- Propietario: acceso total
- Grupo: lectura y ejecución
- Otros: sin acceso

## Directorios Comunes

### /etc

Este directorio raíz es uno de los más importantes del sistema. La carpeta `etc` (abreviatura de *etcetera*) es una ubicación común donde se almacenan archivos de configuración utilizados por el sistema operativo.

Por ejemplo, el archivo `sudoers`, resaltado en la captura siguiente, contiene una lista de usuarios y grupos que tienen permiso para ejecutar `sudo` o ciertos comandos como el usuario `root`.

También se destacan los archivos `passwd` y `shadow`. Estos dos archivos son especiales en Linux porque muestran cómo el sistema almacena las contraseñas de cada usuario usando un formato cifrado llamado `sha512`.

## Algunos contenidos destacados del directorio `/etc`

```bash
tryhackme@linux2:/etc$ ls
shadow passwd sudoers sudoers.d
```

---

### /var

El directorio `/var`, donde `var` significa *variable data*, es una de las principales carpetas raíz de una instalación Linux. Esta carpeta almacena datos a los que los servicios o aplicaciones acceden o escriben frecuentemente.

Por ejemplo:

- Los archivos de logs de servicios y aplicaciones se almacenan aquí (`/var/log`)
- También guarda datos que no están necesariamente asociados a un usuario específico, como bases de datos y similares

## Algunos contenidos destacados del directorio `/var`

```bash
tryhackme@linux2:/var$ ls
backups log opt tmp
```

---

### /root

A diferencia del directorio `/home`, la carpeta `/root` es realmente el directorio personal del usuario del sistema `root`.

No hay mucho más especial en esta carpeta aparte de entender que es el *home directory* del usuario `root`. Sin embargo, merece una mención porque mucha gente asumiría lógicamente que los datos de este usuario estarían en un directorio como `/home/root`.

## Algunos contenidos destacados del directorio `/root`

```bash
root@linux2:~# ls
myfile myfolder passwords.xlsx
```

---

### /tmp

Este es un directorio raíz especial en Linux. Abreviatura de *temporary*, el directorio `/tmp` es volátil y se utiliza para almacenar datos temporales que normalmente solo necesitan usarse una o dos veces.

De manera similar a la memoria RAM, cuando el sistema se reinicia, el contenido de esta carpeta suele eliminarse automáticamente.

En pentesting, este directorio es muy útil porque cualquier usuario puede escribir en él por defecto. Esto significa que, una vez obtenemos acceso a una máquina, `/tmp` es un buen lugar para guardar herramientas como scripts de enumeración.

#### Algunos contenidos destacados del directorio `/tmp`

```bash
root@linux2:/tmp# ls
todelete trash.txt rubbish.bin
```
