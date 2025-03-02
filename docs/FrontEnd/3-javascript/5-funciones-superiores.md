---
sidebar_position: 5
---

# 5 - Funciones del Orden Superior.
----

## Funciones del Orden Superior.

En JavaScript, las funciones del orden superior son funciones que pueden recibir otras funciones como argumentos, retornar funciones como resultado, o ambas cosas. Este concepto es fundamental en la programación funcional y es una característica poderosa de JavaScript, que permite crear código más modular, reutilizable y flexible.

#### Ejemplo 1

-   Se genera una array de números.

-   La función "porCadaUno" recibe el array y lo muestra por consola.

```jsx title="Ejemplo"
const numeros = [1,2,3,4,5];

function porCadaUno(array, fn){
    for(let i=0; i<array.lenght; i++){
        fn(array[i]);
    }

}

//Llamado a función
porCadaUno(numeros, console.log);

```
<br/>

#### Ejemplo 2

-  Se genera un array con números.

-  Se genera un array vacío donde iran los valores multplicados. 

-  Función "porCadaUno" recibe como primer parámetro el array con números.

-  Función "porCadaUno" recibe como segundo parámetro una función flecha.

```jsx title="Ejemplo"
const numeros = [1,2,3,4,5];
const multiplicado = [];

function porCadaUno(numeros, numero =>{
    multiplicado.push(numero *2);
})

console.log("Array multiplicado", multiplicado);
```



### Métodos comunes que usan funciones del orden superior

JavaScript proporciona varios métodos que utilizan funciones del orden superior para manipular arrays:

**forEach:** Itera sobre un array y ejecuta una función por cada elemento.

**map:** Crea un nuevo array con los resultados de la función aplicada a cada elemento del array original.

**filter:** Crea un nuevo array con todos los elementos que pasan una prueba definida por la función.

**reduce:** Ejecuta una función reductora sobre cada elemento del array, resultando en un único valor de retorno.

**find:** Retorna el primer elemento que cumple con una condición definida en la función.

**some:** Verifica si al menos un elemento en el array cumple con la condición implementada por la función.



```jsx title="find"

//Contamos con un array de cursos.

const cursos = [
    {
        nombre: "Angular",
        duracion: "3 meses",
        precio: 300
    },
    {
        nombre: "React",
        duracion: "2 meses",
        precio: 250
    },
    {
        nombre: "Vue",
        duracion: "2 meses",
        precio: 200
    },
    {
        nombre: "Svelte",
        duracion: "1.5 meses",
        precio: 180
    }
];

//Obtenemos el curso llamado "vue"
//La variable "curso" representa cada objeto del array "cursos" para luego podes acceder a sus propiedades, en este caso Nombre.
const resultado = cursos.find(curso => curso.nombre === "Vue");

console.log(resultado);

```

<br/>


```jsx title="filter"

//Contamos con un array de cursos.

const cursos = [
    {
        nombre: "Angular",
        duracion: "3 meses",
        precio: 300
    },
    {
        nombre: "React",
        duracion: "2 meses",
        precio: 250
    },
    {
        nombre: "Vue",
        duracion: "2 meses",
        precio: 200
    },
    {
        nombre: "Angular",
        duracion: "1.5 meses",
        precio: 180
    }
];

//Filtra y devuelve un nuevo array solo con la coincidencia.
//La variable "curso" representa cada objeto del array "cursos" para luego podes acceder a sus propiedades, en este caso Nombre.
const soloAngular = cursos.filter(curso => curso.nombre === "Angular");

console.log(soloAngular);
```

<br/>


```jsx title="map"

//Contamos con un array de cursos.

const cursos = [
    {
        nombre: "Angular",
        duracion: "3 meses",
        precio: 300
    },
    {
        nombre: "React",
        duracion: "2 meses",
        precio: 250
    },
    {
        nombre: "Vue",
        duracion: "2 meses",
        precio: 200
    },
    {
        nombre: "Angular",
        duracion: "1.5 meses",
        precio: 180
    }
];

//mapea y devuelve un nuevo array solo con la coincidencia.
//La variable "curso" representa cada objeto del array "cursos" para luego podes acceder a sus propiedades, en este caso Nombre.
const nombresCursos = cursos.map(curso => curso.nombre);

console.log(nombresCursos);
// Salida -> Angular, React, Vue, Angular
```