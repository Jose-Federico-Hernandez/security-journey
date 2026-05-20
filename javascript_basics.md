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
La tercera linea del código crea una interfaz de lectura que conecta los dos flujos que hemos importado antes, el resutlado (r1) es el objeto con el que interactuamos para hacer preguntas al usuario. Vamos a verlo
con un ejemplo fuera de nuestro programa:
```javascript
const respuesta = await rl.question("¿Cuál es tu nombre? ");
console.log(`Hola, ${respuesta}`);
rl.close(); // importante cerrarlo cuando termines
```
