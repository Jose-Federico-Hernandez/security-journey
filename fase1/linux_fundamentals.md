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
