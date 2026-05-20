## JavaScript Basics

Vamos a hacer el mismo programa que hemos hecho en Python anteriormente pero usando JavaScript, el lenguaje más usado en paginas web. Para este ejercicio se asume que no se tiene ningún conocimiento sobre Javascript.
El programa hará la siguiente:
  1. El ordenador, en secreto, elegirá un número aleatorio de 1 a 20
  2. El usuario irá probando números hasta que acierte
  3. El ordenador, a cada intento del usuario, responderá si su numero es mas alto o mas bajo

JavaScript se ejecuta en node.js, por tanto las primeras lineas de codigo serán importar modulos de node.js
```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });
```
El primer modulo que importamos sirve para leer entrada linea por linea desde la terminal. El segundo modulo importa dos flujos del proceso Node y los renombra a input y output para luego pasarlos a readline de forma 
más legible. 
La tercera linea del código crea una interfaz de lectura que conecta los dos flujos que hemos importado antes, el resutlado (rl) es el objeto con el que interactuamos para hacer preguntas al usuario. Vamos a verlo
con un ejemplo fuera de nuestro programa:
```javascript
const respuesta = await rl.question("¿Cuál es tu nombre? ");
console.log(`Hola, ${respuesta}`);
rl.close(); // importante cerrarlo cuando termines
```
Volviendo al juego, empezamos declarando variables. Para ello, usaremos la siguiente sintaxis:
```javascript
let tries=0;
let guess=0;
```
También creamos una constante que será el número secreto que use el ordenador para jugar
```javascript
const secret = Math.floor(Math.random() * (20)) + 1;
```
- Math.floor elimina los decimales
- Math.random te da un numero aleatorio entre 0 y 1
- (*) 20 amplia el rango de 0 a 20
- +1 cambia el rango de 0-19 a 1-20

Una vez que el número secreto está elegido ya podemos mostrar por consola:
```javascript
console.log("Estoy pensando un número entre el 1 y el 20...");
```
Una vez que ya tenemos declaradas las variables, el siguiente paso logico es que el usuario introduzca un numero, guardarlo como texto y convertirlo en un int base 10 con parseInt(text, 10)
La instrucccion await pausa el sistema hasta que el usuario responda, lo cual es just lo que necesitamos para cuando queremos pedirle un número:
```javascript
const text = await rl.question("Intenta acertarlo: ");
guess=parseInt(text, 10);
```
El bloque try crea un entorno seguro para que, en caso de error, el programa no se bloquee. Pensemos en ello como intentar ejecutar un conjunto de instrucciones y, si ocurre algún problema, manejarlo correctamente y, al final, limpiar los recursos utilizados. 
Vamos a verlo todo junto:
```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * (20)) + 1; // 1 <= secret <= 20
    let tries = 0;
    let guess = 0; 

    console.log("Estoy pensando un número entre el 1 y el 20...");

    const text = await rl.question("Take a guess: "); // r1.question devuelve texto (del usuario)
    guess = parseInt(text, 10); // convertimos el texto a numero

    tries = tries + 1; // añade un intento

} finally {
    rl.close();
}
```
Veamos un resumen de lo que hemos hecho hasta ahora: 
  - Estamos importando el módulo readline con la parte /promises, indicando que el script manejará esperas sin congelar todo el programa.
  - Luego importamos stdout (salida estándar) y stdin (entrada estándar) como input y output.
  - Usando estos módulos importados, creamos la constante readline con el nombre rl.
  - El programa genera su número secreto usando Math.random() y lo guarda como una constante llamada secret.
  - Declaramos dos variables: tries y guess.
  - Mostramos en pantalla que “se ha seleccionado un número entre 1 y 20”.
  - La respuesta del usuario se considera un intento y se guarda en una constante llamada text.
  - La respuesta del usuario se convierte en un número usando parseInt().
  - El número de intentos aumenta en uno.
  - Finalmente, limpiamos y cerramos la interfaz readline, rl, que habíamos creado anteriormente.

