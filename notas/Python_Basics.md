## Python desde 0
Siguiendo el curso de THM, el siguiente bloque es introduccion a Python. Las notas de Python irán aparte de las demás para facilitar su consulta.
Como primer ejemplo, vamos a crear un programa que haga que el ordenador eliga aleatoriamente un número del 1 al 20 y el usuario tenga que adivinarlo, contando también la cantidad de intentos que ha necesitado.
```Python
import random #Biblioteca para elegir numeros aleatorios
secret = random.randint(1, 20) #Utilizamos la función random para que la máquina elija un numero aleatorio de 1 a  20
tries=0
guess=0 #Inicializamos variables teniendo en cuenta que guess no puede estar dentro del rango de numero de numeroSecreto
print("Estoy pensando un número del 1 al 20...") 
text=input("Inténta averiguarlo: ") #input nos devuelve un string
guess =int(text) #convertimos el string en numero
tries=tries+1 #sumamos uno al contador de intentos por cada intento
```
Hasta aquí tenemos inicializadas las variables y los mensajes por consola, pero falta la lógica completa del programa.
```Python
if guess <1 or guess >20:
  print("El número está fuera del rango. Inténtalo de nuevo.")
elif guess < secret:
  print("Demasiado bajo, prueba de nuevo")
elif guess > secret:
  print("Demasiado alto, prueba de nuevo")
else:
  print("Acertaste. Te tomó", tries, "intentos")
```
Aquí es donde se encuentra la lógica del programa. Utilizando la condicion lógica if-else tenemos en cuenta los posibles casos en el juego: el numero está fuera de rango, es demasiado alto, es demasiado bajo o lo has acertado. Tener en cuenta que, a diferencia de en Java, la sintaxis correcta es elif.

Para hacer que el programa se repita hasta que el numero se acierte, tendremos que usar el lógico while, de manera que mientras guess y secret sean distintos, el programa funcione en bucle. Esta función en Python se escribe así: 
```Python
import random #Biblioteca para elegir numeros aleatorios

secret = random.randint(1, 20) #Utilizamos la función random para que la máquina elija un numero aleatorio de 1 a  20
tries=0
guess=0 #Inicializamos variables teniendo en cuenta que guess no puede estar dentro del rango de numero de numeroSecreto

print("Estoy pensando un número del 1 al 20...")

while guess !=secret: #Aquí es donde comienza el bucle. Se lee "Mientras guess sea distinto de secret..."
  text=input("Inténta averiguarlo: ") #input nos devuelve un string
  guess =int(text) #convertimos el string en numero
  tries=tries+1 #sumamos uno al contador de intentos por cada intento
  if guess <1 or guess >20:
    print("El número está fuera del rango. Inténtalo de nuevo.")
  elif guess < secret:
    print("Demasiado bajo, prueba de nuevo")
  elif guess > secret:
    print("Demasiado alto, prueba de nuevo")
  else:
    print("Acertaste. Te tomó", tries, "intentos")
```
