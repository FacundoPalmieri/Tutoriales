---
sidebar_position: 2
---

# 2 - Funciones
----

## **Introducción a las Funciones**

### ¿Qué es una función?

### *Introducción*

Las funciones son fundamentales en cualquier lenguaje de programación, y JavaScript no es la excepción. Permiten agrupar código en bloques que realizan tareas específicas, lo cual facilita la organización, el mantenimiento y la reutilización del código.

### *Estructura Básica de una Función*

En JavaScript, una función se define con la palabra clave **function**, seguida de un nombre, paréntesis () que pueden contener parámetros, y finalmente un bloque de código encerrado entre llaves {}. 

```jsx title="Ejemplo"

function saludar() {
    console.log("¡Hola, mundo!");
}
```

### *Invocación de Funciones*

Para ejecutar la función saludar que definimos, la "llamamos" o "invocamos" usando su nombre seguido de paréntesis:

```jsx title="Ejemplo"

saludar();  // Muestra: ¡Hola, mundo!
```

### *Parámetros y Argumentos*

Las funciones pueden recibir datos externos mediante parámetros. Los parámetros actúan como variables locales dentro de la función.


```jsx title="Ejemplo 1"

function sumar(a, b)//parametros
{
    return a + b;
}

let resultado = sumar(5, 3); //argumentos dentro la invocación 
console.log(resultado);  // Muestra: 8
```




```jsx title="Ejemplo 2"
function prestarLibro(titulo, usuario) {
    console.log(`El libro "${titulo}" ha sido prestado a ${usuario}.`);
}

prestarLibro("1984", "Ana");
// Muestra: El libro "1984" ha sido prestado a Ana.
```


```jsx title="Ejemplo 3"

//valores predeterminados para los parámetros, que se usa en caso de que no se pase un argumento
function devolverLibro(titulo, diasRetraso = 0) {
    const multa = diasRetraso * 0.50;
    const mensaje = diasRetraso > 0 
        ? `Devuelto con ${diasRetraso} días de retraso. Multa: $${multa}` 
        : "Devuelto a tiempo. No hay multa.";
    console.log(`Libro "${titulo}": ${mensaje}`);
}

devolverLibro("El Principito");
// Muestra: Libro "El Principito": Devuelto a tiempo. No hay multa.

devolverLibro("El Principito", 3);
// Muestra: Libro "El Principito": Devuelto con 3 días de retraso. Multa: $1.5.
```


### Beneficios de Usar Funciones

**Reutilización de Código:** Una vez definida, una función puede ser invocada múltiples veces desde distintas partes del programa.

**Modularidad:** Divide el programa en pequeños bloques, facilitando la lectura y mantenimiento.

**Abstracción:** Permite a los desarrolladores pensar en problemas a un nivel más alto sin preocuparse por los detalles de implementación.

#### Buenas Prácticas

**Nombres descriptivos:** Elige nombres que describan claramente lo que la función realiza.

**Funciones cortas:** Idealmente, una función debe realizar una sola tarea. Esto las hace más fáciles de testear y mantener.

**Evitar efectos secundarios:** Una función idealmente no debería modificar variables externas o el estado del programa.


----------------------------

## **Scope**

### Introducción al Scope

En JavaScript, el scope o ámbito de una variable define la parte del código donde dicha variable está disponible para ser utilizada. Comprender el scope es esencial para gestionar la asignación y visibilidad de las variables, evitando errores y comportamientos inesperados en el programa.

### *Scope Global*

Cuando una variable se declara fuera de cualquier función o bloque, tiene un scope global, lo que significa que es accesible desde cualquier parte del código después de su declaración.

```jsx title="Ejemplo"
let color = 'azul'; // Variable global

function mostrarColor() {
    console.log(color); // Accede a la variable global
}

mostrarColor(); // Imprime: azul
```

#### Ventajas:

-   Ofrecen accesibilidad universal en el código.

#### Desventajas:

-   Pueden causar conflictos en proyectos grandes al ser modificadas desde cualquier parte.

### *Scope Local*

El scope local restringe el acceso a una variable al bloque o función donde se declara.

 Existen dos tipos principales:

**1. Scope de Función:** Variables declaradas dentro de una función no son accesibles fuera de ella.

```jsx title="Ejemplo"
    function establecerColor() {
        let color = 'rojo'; // Variable local a la función
    }
    
    // console.log(color); // Error: color no está definido
```


**2. Scope de Bloque**: Introducido con let y const en ES6, permite limitar el scope a un bloque específico, como en ciclos o condicionales.

```jsx title="Ejemplo"
if (true) {
    let color = 'verde'; // Variable de scope de bloque
    console.log(color); // Imprime: verde
}

// console.log(color); // Error: color no está definido


```

-----------------------

## **Funciones Anónimas y Flecha**

### ¿Qué son las Funciones Anónimas?

Las funciones anónimas son funciones sin nombre que pueden ser asignadas a variables, pasadas como argumentos o ejecutadas inmediatamente. Son útiles para encapsular lógica que no necesita reutilizarse o referenciarse directamente.

```jsx title="Ejemplo"
const suma = function(a, b) {
    return a + b;
};
console.log(suma(5, 3)); // Salida: 8
```

### *Usos comunes*

**- Callbacks:** Manejo de eventos o lógica asincrónica.

```jsx title="Ejemplo"
document.getElementById("miBoton").addEventListener("click", function() {
    alert("¡Botón pulsado!");
});
```

**- IIFE (Immediately Invoked Function Expressions):** Ejecución inmediata para crear un alcance privado.

```jsx title="Ejemplo"
(function() {
    let mensaje = "Ejecutada al instante";
    console.log(mensaje);
})();
```

**- Métodos de arrays:**

```jsx title="Ejemplo"
let dobles = [1, 2, 3].map(function(num) {
    return num * 2;
});
```

### ¿Qué son las Funciones Flecha?

Introducidas en ES6, las funciones flecha ofrecen una sintaxis más concisa para escribir funciones anónimas. No tienen su propio this, lo que las hace ideales para callbacks y programación funcional.

```jsx title="Ejemplo"
const suma = (a, b) => return a + b;
console.log(suma(5, 3)); // Salida: 8
```

### *Usos comunes*

**- Callbacks:** Manejo de eventos o lógica asincrónica.

```jsx title="Ejemplo"
let cuadrados = [1, 2, 3].map(n => n * n);
```


### Resumen

#### Función Regular.

Tienen la caracteristica de hoisted.

```jsx title="Ejemplo"
function sumar(valor1, valor2){
    return valor1 + valor2;
}

//Llamado a la función
console.log("El valor de la suma es: " + sumar(20,20));
```

<br/>

#### Función Flecha.

```jsx title="Ejemplo"
const sumar_flecha = (valor1, valor2) => {
    return valor1 + valor2;
}

//Llamado a la función
console.log("El valor de la suma es: " + sumar_flecha(20,20));
```
<br/>


#### Función Flecha con return implícito

```jsx title="Ejemplo"
const sumar_flecha_return_implicito = (valor1, valor2) => valor1 + valor2;

//Llamado a la función
console.log("El valor de la suma es: " + sumar_flecha_return_implicito(20,20));
```

<br/>
#### Función Anónima.

```jsx title="Ejemplo"
const sumar_anonima = function (valor1, valor2) => {
    return valor1 + valor2;
}

//Llamado a la función
console.log("El valor de la suma es: " + sumar_anonima(20,20));
```