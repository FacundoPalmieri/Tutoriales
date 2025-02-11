---
sidebar_position: 3
---

# 3 - Arrays
----

## Fundamentos de Arrays

### ¿Qué es un Array?

Un array en JavaScript es una estructura de datos que permite almacenar una colección de elementos. Estos elementos pueden ser de cualquier tipo, como números, strings, booleanos, objetos, e incluso otros arrays. Esto hace que los arrays sean extremadamente versátiles y útiles en muchas situaciones de programación.

#### Importancia

Los arrays son fundamentales en la programación porque permiten agrupar datos relacionados. Por ejemplo, si quisieras almacenar la información de los usuarios de una aplicación, un array puede contener todos esos datos y facilitar su manejo mediante diversas operaciones, como añadir, eliminar o buscar elementos específicos dentro de la colección.


### Declaración de Arrays

Para declarar un array, puedes utilizar corchetes [], con o sin incluir valores inicialmente. Aquí te muestro algunas formas de declarar arrays:

```jsx title="Ejemplo"
// Array vacío
const arrayVacio = [];

// Array con números
const numeros = [1, 2, 3];

// Array con strings
const colores = ["rojo", "verde", "azul"];

// Array con varios tipos de datos
const mixto = [1, "dos", true];
```

### Acceso al Array.

Cada elemento en un array tiene un índice, que indica su posición. Los índices en JavaScript comienzan en 0, por lo que el primer elemento de un array está en el índice 0, el segundo en el 1, y así sucesivamente. 

```jsx title="Ejemplo"
const frutas = ["manzana", "banana", "cereza"];

// Acceder al primer elemento
console.log(frutas[0]);  // "manzana"

// Acceder al tercer elemento
console.log(frutas[2]);  // "cereza"
```


### Recorrer un Arrays con bucles.

El bucle for es una de las formas más directas de recorrer un array. Usando la propiedad **.length**, podemos dinámicamente acceder a cada elemento del array sin necesidad de hardcodear el número de iteraciones.

La propiedad **.length** es crucial porque nos proporciona la cantidad de elementos que tiene el array, permitiendo que el bucle for se ejecute el número exacto de veces necesario para recorrer todos los elementos. Esto es especialmente útil cuando no sabemos la cantidad exacta de elementos en el array o si el tamaño del array puede cambiar.

```jsx title="Ejemplo"

// Supongamos que tenemos el siguiente array de números
const numeros = [10, 20, 30, 40, 50];

// Para recorrer este array usaremos un bucle for
for (let i = 0; i < numeros.length; i++) {
    console.log(numeros[i]); // Esto imprimirá cada número del array en la consola
}
```

## Métodos y Propiedades

### Métodos de modificación

Los arrays son estructuras de datos fundamentales en JavaScript, utilizadas para almacenar colecciones de elementos. A continuación, exploramos cuatro métodos esenciales para manipular arrays: 
- push.
- pop.
- shift.
- unshift.

#### Método push.

El método push añade uno o más elementos al final de un array. Este método modifica el array original y devuelve la nueva longitud del array.

```jsx title="Ejemplo"
let frutas = ['manzana', 'banana'];
frutas.push('naranja');

console.log(frutas);  // Salida: ['manzana', 'banana', 'naranja']

```
<br/>

#### Método pop.

El método pop elimina el último elemento de un array y lo devuelve. Este método cambia la longitud del array.


```jsx title="Ejemplo"
let frutas = ['manzana', 'banana', 'naranja'];
let frutaEliminada = frutas.pop();

console.log(frutaEliminada);  // Salida: 'naranja'
console.log(frutas);  // Salida: ['manzana', 'banana']

```
<br/>

#### Método shift.

El método shift elimina el primer elemento del array y lo devuelve, desplazando todos los elementos restantes a una posición inferior.


```jsx title="Ejemplo"
let frutas = ['manzana', 'banana', 'naranja'];
let primeraFruta = frutas.shift();

console.log(primeraFruta);  // Salida: 'manzana'
console.log(frutas);  // Salida: ['banana', 'naranja']

```
<br/>


#### Método unshift.

El método unshift añade uno o más elementos al inicio del array y devuelve la nueva longitud del array.


```jsx title="Ejemplo"
let frutas = ['banana', 'naranja'];
frutas.unshift('manzana');

console.log(frutas);  // Salida: ['manzana', 'banana', 'naranja']
```
<br/>

### Métodos de Búsqueda

#### Métodos indexOf e includes

En JavaScript, los métodos **indexOf** e **includes** son herramientas esenciales para buscar elementos dentro de arrays. Estos métodos proporcionan maneras eficientes de localizar la presencia y posición de elementos, facilitando el manejo de datos y la toma de decisiones en el código.

#### Método indexof

El método indexOf devuelve el primer índice en el que se puede encontrar un elemento dado en el array, o -1 si el elemento no está presente. Es útil para localizar la posición exacta de un elemento y es sensible a tipos de datos, es decir, diferencia entre números, strings, etc.

```jsx title="Ejemplo"

let nombres = ['Ana', 'Juan', 'Carlos', 'Marta'];
let indice = nombres.indexOf('Carlos');

console.log(indice);  // Salida: 2

```

<br/>

#### Método includes

El método includes determina si un array contiene un determinado elemento, retornando true o false según corresponda. Este método no proporciona la posición del elemento, sino solo la confirmación de su existencia, lo que lo hace ideal para condiciones y verificaciones rápidas.

```jsx title="Ejemplo"
let frutas = ['manzana', 'naranja', 'banana'];
let contiene = frutas.includes('naranja');

console.log(contiene);  // Salida: true

```

<br/>


### Métodos de Ordenamiento

#### sort()
Ordena un array modificándolo directamente.

Por defecto, sort() convierte los elementos a cadenas y los ordena alfabéticamente. Para ordenar números correctamente, debemos proporcionar una función de comparación.

```jsx title="Ejemplo con números"
const numeros = [5, 2, 9, 1, 5, 6];

// Orden ascendente
numeros.sort((a, b) => a - b);
console.log("Ascendente:", numeros); // [1, 2, 5, 5, 6, 9]

// Orden descendente
numeros.sort((a, b) => b - a);
console.log("Descendente:", numeros); // [9, 6, 5, 5, 2, 1]

```

<br/>

Para ordenar palabras  JavaScript se basa en valores Unicode, lo que coloca primero las mayúsculas y luego las minúsculas.

```jsx title="Ejemplo"
palabras.sort((a, b) => a.localeCompare(b));

console.log("Orden correcto:", palabras);
// ["banana", "kiwi", "manzana", "Pera", "Uva"]


```

#### reverse.

El método reverse() en JavaScript invierte el orden de los elementos de un array. Si lo aplicamos a un array de números sin usar sort() primero, simplemente cambiará su orden actual, sin ordenarlos.

```jsx title="Ejemplo sin ordenamiento previo, solo reverse."
const numeros = [3, 1, 4, 1, 5, 9];

numeros.reverse();

console.log("Invertido:", numeros);
// [9, 5, 1, 4, 1, 3]

```
<br/>

Ejemplo ordenando primero y luego invirtiendo
Si queremos los números en orden descendente, primero usamos sort(), luego reverse():

```jsx title="Ejemplo ordenado primero e invirtiendo después."
const numerosOrdenados = [3, 1, 4, 1, 5, 9];

numerosOrdenados.sort((a, b) => a - b).reverse();
console.log("Orden descendente:", numerosOrdenados);
// [9, 5, 4, 3, 1, 1]

```
<br/>

### Concatenar Arrays.

#### Método join en JavaScript para Convertir Arrays en Cadenas de Texto.

El método join en JavaScript es una herramienta esencial para convertir un array en una cadena de texto, permitiendo especificar un separador entre los elementos del array. Este método es particularmente útil cuando necesitas una representación en forma de cadena de los datos almacenados en un array, ya sea para mostrar la información en la interfaz de usuario, para logs, o para preparar datos para ser enviados a un servidor.

join toma un único argumento opcional: el separador, que determina qué string se utiliza para separar los elementos del array en la nueva cadena resultante. Si no se proporciona un separador, los elementos se concatenarán con una coma (,). Si el separador es una cadena vacía (''), los elementos se concatenarán sin ningún espacio entre ellos.

<br/>

```jsx title=" Uso básico con coma (default)"
let colores = ['Rojo', 'Verde', 'Azul'];
let resultado = colores.join();

console.log(resultado);  // "Rojo,Verde,Azul"
```
<br/>

```jsx title="Uso con separador de espacio"
let nombres = ['Ana', 'Juan', 'Carlos'];
let listaNombres = nombres.join(' ');

console.log(listaNombres);  // "Ana Juan Carlos"
```

<br/>

```jsx title="Uso con separador de guión"
let elementos = ['Hidrógeno', 'Oxígeno', 'Carbono'];
let formula = elementos.join('-');

console.log(formula);  // "Hidrógeno-Oxígeno-Carbono"
```

<br/>

```jsx title="Uso con separador vacío"

let letras = ['J', 'a', 'v', 'a'];
let palabra = letras.join('');
console.log(palabra);  // "Java"
```
<br/>

## Objetos Literales y su Relación

### Introducción a Objetos Literales

En JavaScript, un objeto literal es una colección de pares de **clave-valor** encerrados entre llaves {}. Cada clave está separada de su valor correspondiente por dos puntos :, y los pares clave-valor están separados entre sí por comas. Esta estructura permite representar datos de manera organizada y accesible mediante claves específicas.

```jsx title="Ejemplo"
const persona = {
    nombre: "Juan",
    edad: 30,
    ciudad: "Buenos Aires"
};
```

### Diferencia con Arrays

A diferencia de los arrays, que son colecciones indexadas de elementos, los objetos literales no mantienen un orden específico de sus elementos y no se accede a sus valores mediante índices numéricos. En los arrays, el acceso a los elementos se realiza a través de índices comenzando desde 0, mientras que en los objetos, el acceso se realiza a través de las claves definidas.

```jsx title="Ejemplo"
// Array
const colores = ["rojo", "verde", "azul"];
console.log(colores[0]); // Accede al primer elemento: "rojo"

// Objeto Literal
const semaforo = {
    rojo: "Detenerse",
    verde: "Avanzar",
    azul: "Cuidado"
};
console.log(semaforo.rojo); // Accede al valor de la clave 'rojo': "Detenerse"
```

<br/>

### Array de Objetos

```jsx title="Ejemplo"
// Array inicial con 3 productos
const productos = [
    { id: 1, nombre: "Laptop", precio: 800 },
    { id: 2, nombre: "Mouse", precio: 20 },
    { id: 3, nombre: "Teclado", precio: 50 }
];

// Nuevo producto a agregar
const nuevoProducto = { id: 4, nombre: "Monitor", precio: 300 };

// Agregar producto con push
productos.push(nuevoProducto);

//Mostrar todos los objetos
console.log("Lista de productos actualizada:", productos);

//Acceder a elementos de objetos mediante bucle
for(let objeto of productos){
    console.log(objeto.nombre);
}

```

<br/>

### Manipulación de Arrays y Objetos

En JavaScript, los arrays pueden almacenar varios tipos de datos, incluidos objetos literales. Esto es especialmente útil en aplicaciones donde se manejan colecciones de datos complejos. Los métodos de array como push, sort, y otros se pueden utilizar eficientemente en arrays que contienen objetos literales, lo que permite manipular datos de formas potentes y flexibles.

#### Agregar Objetos con push

Para agregar nuevos objetos a un array, se utiliza el método push. Esto es especialmente útil cuando se está construyendo una lista dinámicamente.

```jsx title="Ejemplo"
const usuarios = [];
usuarios.push({ nombre: "Ana", edad: 25 });
usuarios.push({ nombre: "Luis", edad: 30 });

console.log(usuarios);
// Resultado: [{ nombre: "Ana", edad: 25 }, { nombre: "Luis", edad: 30 }]

```

<br/>

####  Ordenar Objetos con sort

El método sort puede ser personalizado para ordenar arrays de objetos basándose en alguna clave específica de los objetos.

```jsx title="Ejemplo"
usuarios.sort((a, b) => a.edad - b.edad);

console.log(usuarios);
// Resultado: [{ nombre: "Ana", edad: 25 }, { nombre: "Luis", edad: 30 }]
```

<br/>

#### Filtrar con filter

Para filtrar elementos de un array basándose en una condición, se usa filter, que es muy útil para obtener subconjuntos de un array basados en propiedades de los objetos.

```jsx title="Ejemplo"
const mayoresDe25 = usuarios.filter(usuario => usuario.edad > 25);

console.log(mayoresDe25);
// Resultado: [{ nombre: "Luis", edad: 30 }]
```

#### Buscar un Objeto con find

Para encontrar el primer objeto que cumple con una condición específica, se usa find.

```jsx title="Ejemplo"

const luis = usuarios.find(usuario => usuario.nombre === "Luis");
console.log(luis);
// Resultado: { nombre: "Luis", edad: 30 }
```
