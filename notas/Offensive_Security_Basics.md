# OFFENSIVE SECURITY BASICS
## Corresponde a Offensive Security Room de THM

En este ejercicio, interactuaré con una aplicación web y comenzaré a pensar como un atacante, de forma segura y responsable. Este enfoque es lo que se llama **seguridad ofensiva**. Antes de empezar, vamos a familiarizarnos
con algunos términos básicos:

- **Red Teaming**: Metodología de ataque estructurada y autorizada que simula un adversario real para probar la efectividad de las defensas y encontrar vulnerabilidades dentro de un alcance definido.
- **Penetration Test**: Evaluación de seguridad estructurada donde un tester autorizado intenta identificar y explotar vulnerabilidades dentro de un alcance definido para comprender el riesgo real.
- **Vulnerabilidad**: Debilidad o fallo en un sistema, aplicación o configuración que un atacante podría aprovechar.
- **Exploit**: técnica o método utilizado para aprovechar una vulnerabilidad y lograr un resultado específico, como a cceder a funciones o datos restringidos.
- **Scope (Alcance)**: límites de lo que está permitido probar durante el ejercicio. Define qué sistemas, aplicaciones y acciones están permitidos y cuáles fuera de los límites.

ESCENARIO DEL EJERCICIO:

  Después de meses trabajando en su idea de negocio, Mike finalmente está listo para lanzar su sitio web. Ha invertido mucho tiempo y esfuerzo en desarrollar un producto que cree que encantará a los usuarios. Sin embargo, Mike también sabe que empresas de todos los tamaños son atacadas diariamente. Antes de lanzar la web, quiere asegurarse de que no haya páginas sensibles o no deseadas accesibles públicamente.
Tu tarea consiste en realizar una evaluación de su aplicación web e identificar áreas expuestas que puedan representar un riesgo de seguridad. El objetivo es encontrar estas debilidades antes que los atacantes reales y ayudar a Mike a lanzar su web con confianza.

Mike te ha pedido que evalúes su aplicación web y detectes debilidades. Existen varias estrategias posibles, pero empezaremos identificando páginas ocultas que no deberían ser accesibles públicamente.
Prueba las siguientes rutas añadiéndolas al final de la URL:

```http://www.onlineshop.thm/```

Páginas a probar:

sitemap
mail
register
login
admin

Ejemplo:

```http://www.onlineshop.thm/admin```

Si una página no existe, aparecerá un error 404, indicando que el recurso solicitado no fue encontrado.

Introducir URLs manualmente funciona bien cuando tienes pocas páginas que comprobar, pero no es práctico con listas grandes. Para automatizar este proceso se utiliza una herramienta llamada Gobuster.
En la terminal debes ejecutar:

```gobuster dir --url http://www.onlineshop.thm/ -w /usr/share/wordlists/dirbuster/directory-list.txt```

Es importante escribir exactamente la sintaxis indicada para obtener resultados correctos.

- Explicación del comando
 ```gobuster```

Herramienta de línea de comandos utilizada para descubrir contenido web oculto.

```dir```

Modo de enumeración de directorios y archivos.

```--url http://www.onlineshop.thm/ ```


Define el sitio objetivo que Gobuster analizará.

```-w /usr/share/wordlists/dirbuster/directory-list.txt```


Indica el diccionario (wordlist) que Gobuster utilizará para probar nombres de directorios y archivos.

```
Gobuster v3.6
[+] Url: http://www.onlineshop.thm/
[+] Method: GET
[+] Wordlist: directory-list.txt
[+] Negative Status codes: 404
Starting gobuster in directory enumeration mode
/login (Status: 200) [Size: 314]
Progress: 87664 / 87665 (100.00%)
Finished
```

Este es el resultado de gobuster para nuestro ejercicio. Hemos encontrado una página de login oculta.

Parte del hacking ético consiste en aprender a encadenar debilidades. Una sola debilidad puede no parecer un problema crítico por sí sola, pero cuando se combina con otras, puede provocar consecuencias graves. Piensa en las debilidades de seguridad como una fila de fichas de dominó. Una ficha cayendo sola no causa mucho daño. Pero cuando las fichas están colocadas cerca unas de otras, derribar una puede desencadenar una reacción en cadena que haga caer todas las demás. 
En la tarea anterior, descubrir una página de login oculta fue esa primera ficha de dominó. Por sí sola, una página oculta puede no parecer peligrosa, pero puede volverse mucho más seria cuando se combina con otras debilidades, como contraseñas débiles. Los hackers éticos buscan y demuestran estas cadenas de debilidades, mostrando cómo pequeños problemas pueden alinearse para generar un impacto mayor. 
Piensa como un hacker
Para convertirte en hacker, debes pensar como uno. Los hackers miran más allá de si algo funciona correctamente y se preguntan cómo podría ser mal utilizado, combinado con otros comportamientos o usado para acceso no autorizado. Esto implica pensar de forma creativa y probar ideas nuevas. Los hackers éticos adoptan esta misma mentalidad, pero de manera segura y autorizada. Encuentran y demuestran riesgos antes de que lo hagan atacantes reales. 

Aspectos clave a tener en cuenta:


 - Haz preguntas: no asumas que una función trabaja como debería. Pregunta “¿y si no funciona correctamente?”
 - Prueba lo inesperado: intenta acciones y entradas que los desarrolladores no consideraron.
 - Encadena pequeñas debilidades: un fallo pequeño puede ser inofensivo por sí solo, pero peligroso cuando se conecta con otros.
 - Piensa como un adversario: pregúntate “¿cómo abordaría este objetivo un atacante malicioso?” 


Un objetivo valioso
Los atacantes suelen estar interesados en obtener credenciales válidas, como nombres de usuario y contraseñas, porque el acceso puede desbloquear áreas privadas de una aplicación y aumentar sus capacidades. Veamos qué puede obtener un atacante una vez consigue acceso:


 - Funciones sensibles: características que realizan acciones importantes, como modificar datos, ver contenido restringido o ejecutar procesos reservados para usuarios autorizados.
 - Datos de usuarios: información personal o privada, como nombres, correos electrónicos o detalles de cuentas.
 - Funciones administrativas: herramientas de alto privilegio que permiten gestionar usuarios, cambiar configuraciones o tomar control completo de la aplicación.
 - Oportunidades de ataques adicionales: el acceso autenticado puede exponer nuevas vulnerabilidades y permitir avanzar más dentro del sistema. 


En la tarea anterior descubriste una página oculta que permite iniciar sesión a usuarios registrados. Aunque esta página parezca inofensiva, exponer funcionalidades de autenticación puede permitir intentos de acceso no autorizado. En esta tarea intentarás explotar esa debilidad comprobando si el mecanismo de login puede ser abusado. 

Parte práctica

Ahora ya sabes qué página puedes usar y tu siguiente objetivo es determinar si puedes encontrar credenciales válidas (usuario y contraseña) para acceder a la aplicación web. Uno de los nombres de usuario más comunes es:
admin
Comienza probando ese usuario con la siguiente lista de contraseñas:
abc123
123456
password
qwerty
654321
¿Lograste encontrar la contraseña correcta e iniciar sesión? El usuario admin combinado con una de las contraseñas anteriores te dará acceso y mostrará tu flag. 

Automatización del hacking

En la tarea anterior aprendiste el poder de las herramientas automatizadas en hacking ético. Aunque una lista corta de contraseñas puede probarse manualmente rápidamente, en el mundo real los pentesters prueban cientos o miles de contraseñas. 
En esta sección usarás Hydra, una herramienta de pruebas de contraseñas que automatiza intentos de login utilizando una wordlist. Como ya conocemos el usuario, Hydra probará sistemáticamente cada contraseña de la lista hasta encontrar una válida. Esta técnica se conoce como ataque de diccionario. 
Ejecuta el siguiente comando en la terminal:

```hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V```

Explicación del comando

```hydra```

Herramienta utilizada para realizar ataques de diccionario.

```-l admin```

Intenta iniciar sesión usando el usuario admin.

```-P passlist.txt```

Especifica la lista de contraseñas que se probarán.

```www.onlineshop.thm```

Define el sitio web objetivo.

```http-post-form```

Indica que el formulario utiliza peticiones HTTP POST.

```"/login:username=^USER^&password=^PASS^:F=incorrect"```

Define cómo se envía el login y cómo Hydra detecta un intento fallido.

```-V```

Activa la salida detallada mostrando cada intento realizado. 

Los argumentos del comando pueden parecer complejos al principio, pero por ahora basta con ejecutarlo y observar cómo Hydra prueba automáticamente cada contraseña de la lista hasta encontrar credenciales válidas. La contraseña correcta aparecerá en la penúltima línea de resultados. Mucho más rápido que hacerlo manualmente. 
Tanto si utilizaste el método manual como si dejaste que una herramienta automatizada hiciera el trabajo, ya deberías tener la contraseña del administrador.

