---
sidebar_position: 2
---

# 2 - TypeScript

## Fundamentos de TypeScript

TypeScript es un lenguaje de programación de código abierto desarrollado por Microsoft. Se describe como un "superset" de JavaScript, lo que significa que cualquier código JavaScript válido también es código TypeScript. Este lenguaje agrega características adicionales como el tipado estático, interfaces y la programación orientada a objetos (POO), haciéndolo especialmente útil para proyectos grandes y complejos.

#### Ventajas de TypeScript en proyectos Angular

1. Detección temprana de errores

-   El tipado estático permite encontrar errores en tiempo de desarrollo, evitando posibles fallos en producción.


-   Ejemplo: Asignar un número a una variable que debería ser un string generará un error antes de ejecutar el programa.


2. Autocompletado y herramientas mejoradas

-   Los editores modernos, como Visual Studio Code, aprovechan las características de TypeScript para ofrecer autocompletado, refactorización segura y navegación fácil por el código.

3. Escalabilidad

-   TypeScript es ideal para proyectos grandes donde se requiere un código mantenible y estructurado.

4. Compatibilidad con JavaScript

-   TypeScript se transcompila a JavaScript, asegurando que pueda ejecutarse en cualquier entorno compatible con JavaScript.

<br/>

### Tipos en TypeScript

Los tipos son fundamentales en TypeScript y ayudan a garantizar que el código sea consistente y claro. Los principales tipos son:

**string:** Cadenas de texto.
```jsx title=""
let saludo: string = "Hola, TypeScript";
``` 


<br/>

**number:** Números enteros o decimales.
```jsx title=""
let numero: number = 42;
``` 


<br/>

**boolean:** Valores verdadero o falso.
```jsx title=""
let activo: boolean = true;
``` 


<br/>

**any:** Permite cualquier tipo de dato, útil en situaciones donde el tipo no se conoce de antemano.
```jsx title=""
let desconocido: any = "Texto o número";
``` 

<br/>

**void:** Usado en funciones que no retornan un valor.
```jsx title=""
function saludar(): void {
    console.log("Hola");
}
``` 

<br/>

**unknown:** Similar a any, pero requiere una verificación de tipo antes de usar el valor.
```jsx title=""
let dato: unknown = "Texto";
if (typeof dato === "string") {
    console.log(dato.toUpperCase());
}
``` 


<br/><br/>

### Interfaces en TypeScript

Las interfaces definen la estructura que deben seguir los objetos. Son útiles para validar que un objeto tenga propiedades específicas.

En este ejemplo, el objeto usuario debe tener las propiedades nombre y edad, ambas con el tipo de dato especificado en la interface Persona.

```jsx title=""
interface Persona {
    nombre: string;
    edad: number;
}

let usuario: Persona = { nombre: "Juan", edad: 25 };
``` 

<br/>

### Clases en TypeScript

Las clases permiten usar programación orientada a objetos (POO). Soportan conceptos como herencia, encapsulación y polimorfismo.

En este ejemplo:

-   Vehiculo es la clase base con propiedades marca y modelo.

-   Coche extiende la clase Vehiculo e incorpora una nueva propiedad puertas.

-   super() se usa para llamar al constructor de la clase base.

```jsx title=""
class Vehiculo {
    constructor(public marca: string, public modelo: string) {}
}

class Coche extends Vehiculo {
    constructor(marca: string, modelo: string, public puertas: number) {
        super(marca, modelo);
    }
}

let miCoche = new Coche("Toyota", "Corolla", 4);
``` 

<br/><br/>

### Transcompilación a JavaScript.

TypeScript necesita ser transcompilado a JavaScript para ejecutarse en navegadores o entornos como Node.js. Esto se realiza mediante el compilador de TypeScript (tsc).

1.  Instalar el compilador de TypeScript:
```jsx title=""
bash npm install -g typescript
``` 



2.  Compilar un archivo TypeScript a JavaScript:
```jsx title=""
bash tsc archivo.ts
``` 
Este comando generará un archivo archivo.js listo para ejecutarse en cualquier entorno que soporte JavaScript.

<br/><br/>

### Tipos y Estructuras de Datos
TypeScript amplía los tipos básicos de JavaScript con características avanzadas, proporcionando mayor control sobre los datos.

#### Tipos Avanzados

1.  Tuplas: Permiten definir un arreglo con un número fijo de elementos, cada uno con un tipo específico.
```jsx title=""
let coordenadas: [number, number] = [10, 20];
``` 

2.  Enums: Representan un conjunto de valores constantes. Los valores pueden ser numéricos o de texto.
```jsx title=""
enum Color {
Rojo = "Rojo",
Verde = "Verde",
Azul = "Azul"
}

let colorFavorito: Color = Color.Rojo;
``` 

3. Union Types: Permiten que una variable acepte múltiples tipos.

```jsx title=""
let id: string | number;

id = 123;
id = "ABC123";
``` 

4. Tipo Literal: Define valores específicos para una variable.

```jsx title=""
let direccion: "Norte" | "Sur" | "Este" | "Oeste";

direccion = "Norte";
``` 

#### Tipos Inferidos

TypeScript puede inferir automáticamente el tipo de una variable en función del valor asignado.
```jsx title=""
let nombre = "Carlos"; // TypeScript infiere que es de tipo string.
``` 

---------------------------------------------------

### Decoradores en TypeScript

Los decoradores son funciones especiales que se utilizan para modificar clases, métodos, propiedades o parámetros. Angular utiliza decoradores para definir componentes, directivas y servicios.

#### Tipos de Decoradores

1.  Decorador de Clase

```jsx title=""
function Log(clase: Function) {
  console.log(`La clase ${clase.name} ha sido declarada.`);
}

@Log
class MiClase {}

``` 
Explicación: El decorador @Log imprime un mensaje cuando la clase MiClase es creada.

<br/>

2. Decorador de Propiedad

```jsx title=""
function Capitalizar(target: any, key: string) {
  let valor = target[key];

  const getter = () => valor.toUpperCase();
  const setter = (nuevoValor: string) => valor = nuevoValor;

  Object.defineProperty(target, key, { get: getter, set: setter });
}

class Usuario {
  @Capitalizar
  nombre: string = "juan";
}

const usuario = new Usuario();
console.log(usuario.nombre); // JUAN

``` 

Explicación: El decorador @Capitalizar transforma la propiedad nombre a mayúsculas.

<br/>

3. Decorador de Método

```jsx title=""
function Autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const metodoOriginal = descriptor.value;
  const nuevoDescriptor: PropertyDescriptor = {
    configurable: true,
    enumerable: false,
    get() {
      const boundFn = metodoOriginal.bind(this);
      return boundFn;
    }
  };
  return nuevoDescriptor;
}

class Boton {
  mensaje = "Haz clic en mí";

  @Autobind
  mostrarMensaje() {
    console.log(this.mensaje);
  }
}

const boton = new Boton();
const botonHTML = document.querySelector('button');
botonHTML?.addEventListener('click', boton.mostrarMensaje);

``` 
Explicación: El decorador @Autobind asegura que this en el método mostrarMensaje siempre se refiera a la clase Boton.