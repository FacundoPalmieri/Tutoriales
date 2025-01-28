---
sidebar_position: 1
---

# 1 - Introducción a JavaScript
----

## **Introducción a la Sintaxis de JavaScript**


### *1.Vinculación con HTML y CSS*

Se logra mediante la etiqueta < script > en HTML, que permite incluir archivos JavaScript externos o incorporar código directamente dentro de los documentos HTML.


Para vincular un archivo JavaScript externo a un documento HTML, se utiliza la etiqueta < script > con el atributo src, que contiene la ruta al archivo JavaScript. Esta etiqueta se puede colocar dentro del < head > o antes del cierre del < body > (Recomendado), dependiendo de cuándo se desea que se cargue el script.

Tradicionalmente, los scripts se colocan al final del cuerpo del documento (< body >) para no bloquear la carga de la página. Con async y defer, tenemos mayor flexibilidad para colocar las etiquetas < script > en la sección < head >, ya que su comportamiento asincrónico evita el bloqueo del renderizad

```jsx title="Ejemplo"
<script src="ruta/al/archivo.js"></script>
```


### *2.Atributos async y defer*

Para controlar cómo y cuándo se cargan los scripts, podemos usar los atributos async y defer, que influyen en el comportamiento de carga y ejecución de los scripts sin bloquear el renderizado del resto de la página.

#### async

El atributo async permite que el script se cargue de manera asincrónica con respecto al resto de la página. Esto significa que el navegador no necesita esperar a que el script se cargue y ejecute para continuar procesando el resto del contenido HTML. El script se ejecutará tan pronto como esté disponible, lo que puede mejorar la velocidad de carga de la página.


```jsx title="Ejemplo"
<script async src="ruta/al/archivo.js"></script>
```


#### defer

El atributo defer, por otro lado, también permite la carga asincrónica del script, pero retrasa la ejecución del mismo hasta que todo el documento HTML ha sido completamente cargado y analizado. Esto es útil para scripts que necesitan interactuar con elementos del DOM o cuyo orden de ejecución es importante.

```jsx title="Ejemplo"
<script defer src="ruta/al/archivo.js"></script>
```


### *3. Variables y Tipos de Datos*

JavaScript utiliza variables para almacenar datos que pueden variar durante la ejecución del programa. Las variables se declaran con las palabras clave let o const.

Estas variables pueden ser declaradas (se les asigna solamente un nombre) o declarada e inicializada(se le declara un nombre y un valor específico).

- **let**: para datos que pueden cambiar.
- **const**: para datos que no deben modificarse después de su inicialización.

```jsx title="Ejemplo"
let nombre;(se declara)
let nombre = "Pedro"; (Se declara primero y se inicializa alojando un valor)

```

#### Los tipos de datos principales en JavaScript son:

**String:** para textos.

**Number:** para números.

**Boolean:** para valores verdadero (true) o falso (false).

**null:** representa un valor nulo.

**undefined:** indica que una variable no tiene asignado un valor.

### *4. Operaciones Básicas*

JavaScript permite realizar operaciones aritméticas básicas como suma (+), resta (-), multiplicación (*) y división (/).

```jsx title="Ejemplo"
let suma = 10 + 5; // 15
let producto = 20 * 2; // 40

```

### *5. Condicionales*

Los condicionales en JavaScript son estructuras fundamentales que permiten tomar decisiones en un programa basándose en si una o más condiciones se cumplen. Al igual que en la vida real, donde tomamos decisiones basadas en diferentes situaciones, los condicionales nos permiten ejecutar distintas partes de código dependiendo de ciertas condiciones lógicas

#### if

La estructura básica del if en JavaScript se utiliza para ejecutar un bloque de código solo si una condición específica es verdadera. Por ejemplo:


```jsx title="Ejemplo"
if (suma > 20) {
  console.log("La suma es mayor que 20");
}

```

#### else

Para manejar múltiples condiciones, podemos extender el uso del if con else y else if. El else permite ejecutar un bloque de código cuando la condición del if es falsa. Por ejemplo:


```jsx title="Ejemplo"

if (suma > 20) {
  console.log("La suma es mayor que 20");
}else {
    console.log("La suma es menor que 20");
}
```

#### else if

Cuando hay múltiples posibles condiciones que queremos evaluar, podemos usar else if para definir condiciones adicionales:

```jsx title="Ejemplo"

if (suma > 20) {
  console.log("La suma es mayor que 20");
}else if(suma > 30) {
    console.log("La suma es mayor que 20 y 30");
} else{
    console.log("La suma es mayor que 20 y menor que 30");
}
```


### *6. Operadores de lógicos y de Comparación*

En JavaScript, los operadores lógicos y de comparación son herramientas esenciales que permiten evaluar condiciones y comparar valores. Estos operadores son fundamentales para controlar el flujo de ejecución en programas a través de estructuras condicionales como if, else, y bucles como while y for.


#### Operadores de Comparación

Los operadores de comparación evalúan dos valores y retornan un valor booleano (true o false) dependiendo de si la comparación es verdadera.


```jsx title=" == (igual)"
Compara si dos valores son iguales, ignorando el tipo de dato.
jsx console.log(2 == '2'); // true

```

```jsx title="=== (Estrictamente igual)"
Compara si dos valores son iguales y del mismo tipo.
jsx console.log(2 === '2'); // false
```

```jsx title="!= (No igual)"
Compara si dos valores no son iguales, ignorando el tipo de dato.
jsx console.log(2 != '3'); // true
```

```jsx title="!== (Estrictamente no igual)"
Compara si dos valores no son iguales o no son del mismo tipo.
jsx console.log(2 !== '2'); // true
```

```jsx title="<, < = , >, >= (Menor que, Menor o igual que, Mayor que, Mayor o igual que)"
Utilizados para comparaciones numéricas.
jsx console.log(3 < 4); // true 
console.log(5 >= 5); // true
```


#### Operadores Lógicos

Los operadores lógicos se usan para combinar múltiples condiciones booleanas.

```jsx title="&& (AND)"
Retorna true si ambos operandos son verdaderos.
jsx console.log(true && false); // false
```



```jsx title="|| (OR)"
Retorna el primer operando verdadero o el último operando si todos son falsos. Sin embargo, considera valores "falsy" como:
 0, '', false, null y undefined.

let nombre = "";
let saludo = nombre || "Invitado";
console.log(saludo); // "Invitado"
```



```jsx title="?? (Nullish Coalescing )"
El operador Nullish Coalescing (??) retorna el operando de la derecha si el operando de la izquierda es null o undefined. 
A diferencia de ||(OR), no considera otros valores "falsy" como 0 o ''.

let valor2 = 0;
let resultadoNullish = valor2 ?? "Valor predeterminado";

console.log(resultadoNullish); // 0
```




```jsx title="! (NOT)"
Invierte el valor booleano del operando.
jsx console.log(!true); // false
```


```jsx title="Ejemplo"

let edad = 20;
let acceso = (edad >= 18) && (edad <= 30);
console.log(acceso); // true si la edad está entre 18 y 30
```



### *7. Ciclos*

En JavaScript, los ciclos o bucles permiten repetir una o más instrucciones de manera eficiente hasta que se cumpla una condición específica. A continuación, exploraremos los tres tipos principales de ciclos: for, while, y do...while.

#### Ciclo For

El ciclo for es uno de los más utilizados para ejecutar un bloque de código un número determinado de veces. Su estructura incluye tres partes principales:

**Inicialización:** Establece el estado del contador, por ejemplo, let i = 0.

**Condición:** El ciclo se ejecuta mientras la condición sea verdadera. Por ejemplo, i < 10.

**Actualización:** Actualiza el valor del contador después de cada iteración. Por ejemplo, i++.

```jsx title="Ejemplo"

for (let i = 0; i < 5; i++) {
    console.log('Número: ' + i);
}

Este código imprimirá los números del 0 al 4 en la consola.
```


#### Ciclo While

El ciclo while sigue ejecutándose mientras la condición especificada sea verdadera. A diferencia del ciclo for, la inicialización y actualización del contador suelen hacerse fuera y dentro del ciclo, respectivamente.


```jsx title="Ejemplo"

let i = 0;
while (i < 5) {
    console.log('Número: ' + i);
    i++;
}

Este código también imprimirá los números del 0 al 4.
```



#### Ciclo Do...While
A diferencia del ciclo while, el ciclo do...while garantiza que el cuerpo del bucle se ejecute al menos una vez, ya que la condición se evalúa después de la ejecución del código dentro del bucle

```jsx title="Ejemplo"

let i = 0;
do {
    console.log('Número: ' + i);
    i++;
} while (i < 5);

Este código imprimirá los números del 0 al 4, asegurando que el bloque de código se ejecute al menos una vez.
```


#### Uso de break y continue

En JavaScript, los bucles son estructuras que repiten bloques de código. Dentro de estos ciclos, las declaraciones break y continue permiten manejar el flujo de ejecución de formas más flexibles y potentes. Aquí veremos cómo y cuándo utilizar estas declaraciones para controlar los ciclos eficazmente

#### Uso de break

La declaración break se utiliza para salir de un ciclo de forma inmediata, sin importar si la condición del ciclo sigue siendo verdadera.

```jsx title="Ejemplo"

for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break; // Sale del ciclo cuando i es igual a 5
    }
    console.log(i); // Imprime los números de 0 a 4
}
```

#### Uso de continue

La declaración continue omite la iteración actual del ciclo y continua con la siguiente, siempre y cuando la condición del ciclo siga siendo verdadera.

```jsx title="Ejemplo"

for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) {
        continue; // Omite los números pares
    }
    console.log(i); // Imprime los números impares entre 0 y 9
}
```