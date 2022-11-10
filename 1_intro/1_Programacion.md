---
title: Introducción a la programación
has_children: false
nav_order: 2
has_toc: false
---

# ¿Qué es programación?

{: .concept }
La programación es el proceso de escribir instrucciones para una computadora para lograr que esta haga lo deseado. 

Si alguna vez cocinaste algo usando una receta, podemos hacer la siguiente analogía: La persona que cocina es la computadora, la receta es el programa y el autor de la receta el programador. El autor provee un conjunto de instrucciones que el cocinero lee, interpreta y sigue. Cuanto mas complejas las instrucciones, mas complejo el resultado.

### Actividad:
{: .no_toc }

Sabes para dar buenas instrucciones? Trata de dibujar y cuadrado con el personaje, usando solo los comandos disponibles:

<div style="text-align: center">
    <iframe src="https://content.codecademy.com/programs/code-foundations-path/bop-i/intro-article-turtle-codey/index.html" width="640" height="450"></iframe>
</div>

Lo que hacemos en esta actividad, donde el personaje representa la computadora, es escribir un programa para dibujar algo. No le podemos pedir a la computadora que dibuje el cuadrado, porque en este caso solo sabe moverse una cantidad de pasos a la vez, asi que tenemos que trabajar con los comandos que la computadora conoce. 

La lista de instrucciones que escribimos representa el programa.

---

## Programa vs Algoritmo

Muchas veces escuchamos que estas palabras se usan indistintamente, pero en realidad son conceptos diferentes, y entender correctamente cada uno es muy importante.

* **Programa:** es un conjunto de instrucciones escritas de forma que la computadora pueda interpretarlas.
* **Algoritmo:** es un conjunto de pasos necesarios para resolver un problema.

Si seguimos hablando del ejemplo de la cocina, podemos decir que la receta es una serie de pasos para poder hacer un plato, en ese caso, la receta es un algoritmo. En este caso, la receta y el algoritmo están escritos en español.

{: .concept }
Cuando tomamos ese algoritmo y lo traducimos a un lenguaje que una computadora entiende, estamos implementando ese algoritmo en un programa.

Podemos usar el ejemplo de la actividad anterior. Cuando estamos pensando en los pasos necesarios para dibujar el cuadrado estamos haciendo un algoritmo, pero cuando expresamos esos pasos usando los comandos disponibles en la computadora, estamos escribiendo el programa.


Escribimos estas instrucciones con un lenguaje de programación, que luego es interpretado por el dispositivo. Estos conjuntos de instrucciones pueden recibir varios nombres:  programa, programa de computadora, aplicación (app) y ejecutable son algunos nombres populares.

Un programa puede ser cualquier cosa que esté escrita con código; Los sitios web, los juegos y las aplicaciones para teléfonos son programas. Si bien es posible crear un programa sin escribir código, el dispositivo interpreta la lógica subyacente y lo más probable es que esa lógica se haya escrito con código. Un programa que está ejecutando o ejecutando código está realizando instrucciones. El dispositivo con el que está leyendo esta lección está ejecutando un programa para imprimirlo en su pantalla.

✅ Busca en google: ¿quién fue el primer programador/a informático del mundo?

# ¿Qué es lenguaje de programación?

Los lenguajes de programación permiten a los desarrolladores escribir instrucciones para una computadora. Las computadoras solo pueden entender binarios (1 y 0) y, para la mayoría de los desarrolladores, esa no es una forma muy eficiente de comunicarse. Los lenguajes de programación son el vehículo para la comunicación entre humanos y computadoras.

En la actividad que hicimos anteriormente, el lenguaje de programación eran los comandos que teníamos disponibles. Un algoritmo puede escribirse en español, pero un programa debe estar escrito en un lenguaje que la computadora entienda.

Los lenguajes de programación vienen en diferentes formatos y pueden servir para diferentes propósitos. Por ejemplo, JavaScript se usa principalmente para aplicaciones web, mientras que Bash se usa principalmente para sistemas operativos.

Las computadoras son dispositivos electrónicos, y como tales solo aceptan impulsos eléctricos como entrada (instrucciones), estos impulsos conformal el **lenguaje de máquina**, en el que cada instrucción es un conjunto de unos y ceros (binario). Durante los inicios de la programación este era el único lenguaje en el que podíamos comunicarnos con las computadoras.

<div style="text-align: center">
    <img src="https://i.giphy.com/media/1YhafU1NFtethNxJwa/giphy.webp" onerror="this.onerror=null;this.src='https://i.giphy.com/1YhafU1NFtethNxJwa.gif';" alt="">
</div>
<div style="text-align: center">
    <img src="https://media0.giphy.com/media/l0HlMDr5SOKGpNu5a/giphy.gif?cid=ecf05e47sve3xqz2aj3g2qxw6qi4e7imawq4prxtzba2uh1n&rid=giphy.gif&ct=g"  alt="">
</div>

Este lenguaje de maquina es considerado el lenguaje de mas bajo nivel, y a partir de ahi se fueron desarrollando lenguajes que se asemejen mas y mas al lenguaje natural de los humanos. Cuanto mas fácil sea de leer y escribir para nosotros, decimos que el lenguaje es de mas alto nivel.

Los lenguajes de bajo nivel generalmente requieren menos pasos que los idiomas de alto nivel para que un dispositivo interprete las instrucciones. Sin embargo, lo que hace que los lenguajes de alto nivel sean populares es su legibilidad y compatibilidad. JavaScript se considera de alto nivel, y es el lenguaje que utilizaremos durante la primera parte de este curso.

El siguiente código ilustra la diferencia entre un lenguaje de alto nivel con JavaScript y un lenguaje de bajo nivel con código ensamblador ARM.

```javascript
let number = 10
let n1 = 0, n2 = 1, nextTerm;

for (let i = 1; i <= number; i++) {
    console.log(n1);
    nextTerm = n1 + n2;
    n1 = n2;
    n2 = nextTerm;
}
```

```c
 area ascen,code,readonly
 entry
 code32
 adr r0,thumb+1
 bx r0
 code16
thumb
 mov r0,#00
 sub r0,r0,#01
 mov r1,#01
 mov r4,#10
 ldr r2,=0x40000000
back add r0,r1
 str r0,[r2]
 add r2,#04
 mov r3,r0
 mov r0,r1
 mov r1,r3
 sub r4,#01
 cmp r4,#00
 bne back
 end
```

Estos dos programas hace exactamente lo mismo, imprimiendo una cadena de Fibonacci hasta 10.

## Elementos de un programa

Podemos diseccionar la estructura de cualquier programa y encontrar algunos elementos fundamentales:
* **Sentencia:** Cada instrucción que le damos a una computadora es una sentencia, y por lo general tiene un símbolo que marca donde termina la instrucción. Este símbolo puede variar de lenguaje en lenguaje. Ejemplo: *mover__pasos*
* **Variable:** Los programas pueden manipular distintas formas de datos que pueden cambiar su comportamiento. Los lenguajes de programación vienen con un mecanismo para almacenar estos datos temporalmente para poder poder usarlos mas tarde. Estos datos son conocidos como *variables*, una variable es una sentencia que intrude al dispositivo a que guarde datos en su memoria. Igual que en algebra, en un programa las variables tendrán un nombre y su valor puede cambiar. Ejemplo: en la sentencia *mover_X_pasos*, *X* es una variable. 
* **Condicionales:** Existe la posibilidad de que nuestro programa no deba ejecutar algunas de sus sentencias bajo ciertas condiciones. Este tipo de control sobre una aplicación la hace mas robusta y mantenible. Una sentencia común utilizada en la programación moderna para controlar cómo se ejecuta un programa es la declaración `if..else` que significa `si..sino`.
* **Bucles:** Son sentencias que nos permiten hacer que la computadora repita las mismas instrucciones un numero determinado de veces.

✅ Aprenderás más sobre estas sentencias en lecciones posteriores.