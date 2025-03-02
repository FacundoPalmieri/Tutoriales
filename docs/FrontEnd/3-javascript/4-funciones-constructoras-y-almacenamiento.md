---
sidebar_position: 4
---

# 4 - Funciones Constructoras y Almacenamiento.
----

## Función Constructora

### Función

Una función constructora es una función especial utilizada para crear y estructurar objetos. Se usa con la palabra clave new para instanciar un nuevo objeto basado en un "molde" definido dentro de la función.

🔹 Características principales:

✅ Se usa this para definir las propiedades del objeto.

✅ Se invoca con new para crear una nueva instancia.

✅ Cada instancia tiene sus propios valores de las propiedades.

✅ Puede incluir métodos, pero no es eficiente declararlos dentro de la función (porque se replican en cada instancia).

```jsx title="Sintaxis"
function Persona(nombre, edad) {
    this.nombre = nombre;
    this.edad = edad;
}

Persona.prototype.saludar = function() {
    console.log(`Hola, mi nombre es ${this.nombre} y tengo ${this.edad} años.`);
};

const persona1 = new Persona("Juan", 30);
persona1.saludar();



```

### Clase

Las clases en ES6 fueron introducidas para proporcionar una sintaxis más clara y concisa para alcanzar el mismo resultado que las funciones constructoras. La sintaxis de clase es más familiar para los programadores que vienen de otros lenguajes de programación orientados a objetos y ayuda a manejar la herencia de manera más intuitiva.

```jsx title="Sintaxis"
class Persona {
    constructor(nombre, edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    saludar() {
        console.log(`Hola, mi nombre es ${this.nombre} y tengo ${this.edad} años.`);
    }
}

const persona2 = new Persona("Ana", 25);
persona2.saludar();

```

🔹 Características principales:

✅ Usa constructor para definir propiedades.

✅ Los métodos van dentro de la clase sin necesidad de prototype.

✅ Se instancia con new.

----------------------------------------

## LocalStorage

#### Introducción a localStorage y sessionStorage

El almacenamiento web proporciona métodos y protocolos para almacenar datos de manera local en el navegador del usuario. Las dos principales herramientas que ofrece son: localStorage y sessionStorage.


### sessionStorage

sessionStorage es una API de almacenamiento web que permite guardar los datos solo se almacenan mientras la pestaña del navegador esté abierta.

📌 Características principales:

✅ Almacena datos en pares clave-valor.

✅ La información se borra cuando el usuario cierra la pestaña o el navegador.

✅ Solo almacena cadenas de texto (string), pero se pueden guardar objetos convirtiéndolos a JSON.

✅ Tiene un límite de almacenamiento de aproximadamente 5MB por dominio.

✅ Se accede a través del objeto sessionStorage.




Se recomienda para:
-   Información que debe desaparecer después de la sesión, como datos de un formulario en progreso en una compra o configuraciones temporales de visualización.

-   Datos sensibles que no deben persistir más allá de la sesión actual, como detalles de una transacción financiera.


🔹 Cómo usar sessionStorage

```jsx title="Sintaxis"
sessionStorage.setItem('clave', 'valor');
```

```jsx title="Guardar Dato"
sessionStorage.setItem("usuario", "Juan");
```

```jsx title="Obtener Dato"
let usuario = sessionStorage.getItem("usuario");
console.log(usuario); // "Juan"
```

```jsx title="Eliminar Dato"
sessionStorage.removeItem("usuario");
```

```jsx title="Limpiar todo el almacenamiento"
sessionStorage.clear();
```

🔹 Guardar objetos en sessionStorage

Hay que convertir los objetos a JSON con JSON.stringify() antes de guardarlos.

✅ Ejemplo con objetos

```jsx title="Ejemplo"
const usuario = {
    nombre: "Ana",
    edad: 25,
    email: "ana@email.com"
};

// Guardar el objeto como JSON
sessionStorage.setItem("usuario", JSON.stringify(usuario));

// Recuperar el objeto y convertirlo nuevamente a JavaScript
const usuarioGuardado = JSON.parse(sessionStorage.getItem("usuario"));

console.log(usuarioGuardado.nombre); // "Ana"
console.log(usuarioGuardado.edad); // 25

```
<br/>

### localStorage

El localStorage es una API de almacenamiento web que permite guardar datos en el navegador de forma persistente (es decir, que no se borran al cerrar la pestaña o el navegador).

📌 Características principales:

✅ Almacena datos en pares clave-valor.

✅ Permanece incluso si el usuario recarga la página o cierra el navegador.

✅ Solo **almacena cadenas de texto (string)** (aunque se pueden guardar objetos convirtiéndolos en JSON).

✅ Tiene un límite de almacenamiento de aproximadamente 5MB por dominio.

✅ Se accede a través del objeto localStorage.


Se recomienda para:

-   Guardar preferencias de usuario que no requieren ser borradas al cerrar el navegador.

-   Almacenar datos para el autocompletado de formularios en visitas futuras.

-   Guardar tokens de autenticación y configuraciones para uso a largo plazo.

🔹 Cómo usar localStorage

```jsx title="Sintaxis"
localStorage.setItem('clave', 'valor');
```

```jsx title="Guardar Dato"
localStorage.setItem("usuario", "Juan");
```

```jsx title="Obtener Dato"
let usuario = localStorage.getItem("usuario");
console.log(usuario); // "Juan"
```
```jsx title="Guardar Número y recuperar"
localStorage.setItem('puntuacion', '10');

// Parsear valor a entero para recuperar y mostrarlo.
let puntuacion = parseInt(localStorage.getItem('puntuacion'));
console.log(puntuacion); // 10

```


```jsx title="Eliminar Dato"
localStorage.removeItem("usuario");
```

```jsx title="Limpiar el almacenamiento"
localStorage.clear();
```
🔹 Guardar objetos en localStorage

Como localStorage solo almacena cadenas de texto, hay que convertir objetos a JSON con JSON.stringify() antes de guardarlos.

✅ Ejemplo con objetos

```jsx title="Ejemplo"
const usuario = {
    nombre: "Ana",
    edad: 25,
    email: "ana@email.com"
};

// Guardar el objeto como JSON
localStorage.setItem("usuario", JSON.stringify(usuario));

// Recuperar el objeto y convertirlo nuevamente a JavaScript
const usuarioGuardado = JSON.parse(localStorage.getItem("usuario"));

console.log(usuarioGuardado.nombre); // "Ana"
console.log(usuarioGuardado.edad); // 25
```

------------------------------------------

## JSON y Almacenamiento de Objetos

JSON (JavaScript Object Notation) es un formato de intercambio de datos ligero y fácil de leer, basado en la sintaxis de objetos de JavaScript. Se usa ampliamente para almacenar y transmitir datos en aplicaciones web y APIs.

📌 Características principales:
✅ Formato basado en texto (se puede leer fácilmente).
✅ Utiliza pares clave-valor.
✅ Compatible con la mayoría de los lenguajes de programación.
✅ Es el formato estándar para la comunicación entre servidores y clientes web.

### Guardar Objetos JSON en localStorage y sessionStorage

En el contexto de las aplicaciones web, es común que necesites almacenar datos en el navegador del usuario. Aquí es donde localStorage y sessionStorage entran en juego. Ambos proporcionan un espacio para almacenar datos en forma de clave-valor, pero con algunas diferencias:

**localStorage:** Almacena datos sin fecha de expiración. Los datos no se eliminarán cuando se cierre el navegador y estarán disponibles entre las sesiones de navegación.

**sessionStorage:** Almacena datos solo para una sesión de página. Los datos se eliminan cuando se cierra la pestaña o el navegador.

Ambos, localStorage y sessionStorage, solo pueden almacenar cadenas (strings), por lo que para almacenar objetos JavaScript complejos, es necesario serializarlos a JSON. Utilizando JSON.stringify() puedes convertir un objeto JavaScript en una cadena JSON, y con JSON.parse(), puedes convertir una cadena JSON de vuelta a un objeto JavaScript.

```jsx title="Ejemplo"

// Objeto usuario
const usuario = {
  id: "1234",
  nombre: "Juan",
  correo: "juan@example.com"
};

// Convertir el objeto usuario a una cadena JSON para almacenamiento
localStorage.setItem('usuario', JSON.stringify(usuario));

// Recuperar el objeto usuario de localStorage y convertirlo de nuevo a un objeto JavaScript
const usuarioAlmacenado = JSON.parse(localStorage.getItem('usuario'));

console.log(usuarioAlmacenado);
```

### Recuperar y convertir JSON a objetos

#### ¿Qué es JSON.parse?

JSON.parse es una función incorporada en JavaScript que se utiliza para convertir una cadena de texto en formato JSON a un objeto de JavaScript. Este proceso es conocido como deserialización, y es fundamental cuando recuperamos datos almacenados que necesitan ser manipulados o accesados como objetos dentro de nuestras aplicaciones.

Cuando almacenamos datos en localStorage o sessionStorage, estos datos deben ser cadenas. Si tenemos objetos JavaScript, primero los convertimos en una cadena JSON usando JSON.stringify(). Para revertir este proceso, utilizamos JSON.parse().

Imaginemos que tenemos un sitio web de comercio electrónico y necesitas almacenar la información del carrito de compras del usuario entre sesiones. Podriamos almacenar esta información en localStorage como un string JSON y recuperarlo cada vez que el usuario visite la página.

```jsx title="Ejemplo"
// Supongamos que este es el carrito de compras del usuario
const carrito = {
    items: [
        { id: 1, producto: "Libro", cantidad: 2, precio: 15.00 },
        { id: 2, producto: "Lápiz", cantidad: 10, precio: 1.50 }
    ]
};

// Convertimos el objeto carrito en una cadena JSON y lo almacenamos
localStorage.setItem('carrito', JSON.stringify(carrito));

// Recuperamos la cadena JSON del almacenamiento y la convertimos de nuevo a un objeto JavaScript
const carritoAlmacenado = JSON.parse(localStorage.getItem('carrito'));

// Ahora podemos acceder y manipular los datos del carrito como un objeto JavaScript
console.log(`Total de productos: ${carritoAlmacenado.items.length}`);
```