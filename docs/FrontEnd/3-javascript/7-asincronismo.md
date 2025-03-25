---
sidebar_position: 7
---

# 7 - Asincronismo
----

## **Fundamentos de Asincronismo**

En JavaScript, el asincronismo es un concepto fundamental que permite a los desarrolladores manejar múltiples tareas simultáneamente, algo crucial en el desarrollo de aplicaciones modernas que requieren interactuar con APIs, manejar grandes volúmenes de datos o simplemente mejorar la experiencia del usuario. Para comprender la importancia del asincronismo, es necesario primero entender la diferencia entre ejecución sincrónica y asincrónica.

## ¿Qué es el asincronismo en JavaScript?
La ejecución **sincrónica** se refiere a la ejecución de código de manera secuencial, donde cada línea de código se ejecuta una después de la otra. 
JavaScript es un lenguaje single-threaded, lo que significa que solo ejecuta una operación a la vez en su hilo principal. **Sin embargo, usa un modelo asincrónico basado en eventos para manejar tareas que no pueden bloquear la ejecución del programa, permitiendo que otras operaciones continúen mientras se esperan respuestas.**

###  Formas de manejar el asincronismo

### *Async/Await*

Es una forma más clara y ordenada de trabajar con promesas. Permite escribir código asincrónico de manera similar a código sincrónico.


El await debe usarse dentro de una función asíncrona (es decir, dentro de una función marcada con async), porque su propósito es hacer que esa función específica se "pause" temporalmente en los puntos donde usas await, pero sin bloquear el hilo principal de ejecución. Así, el resto del programa puede seguir funcionando normalmente mientras se espera la resolución de la promesa.

#### ¿Por qué dentro de una función async?

Cuando una función está marcada con async, el await le permite esperar las promesas sin bloquear el flujo de ejecución. Si no estuviera dentro de una función async, usar await provocaría un error, ya que await solo es válido dentro de funciones que se marcan con async.


```jsx title="HTML"
async function obtenerDatos() {
    // La ejecución se pausa aquí hasta que se resuelva la promesa
    // fetch permite realizar peticiones Http a una API
    let respuesta = await fetch("https://jsonplaceholder.typicode.com/users/1");
}
``` 


Veamos un ejemplo de como funciona

```jsx title="HTML"
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Usuario API</title>
</head>
<body>
    <h1>Información del Usuario</h1>
    <div id="usuario"></div>

    <script src="script.js"></script> <!-- Enlazamos el archivo JS -->
</body>
</html>
``` 

```jsx title="JavaScript"
async function obtenerUsuario() {
    try {
        // 1️⃣ Hacer la petición a la API
        let respuesta = await fetch("https://jsonplaceholder.typicode.com/users/1");

        // 2️⃣ Convierte la respuesta en JSON
        let usuario = await respuesta.json();

        // 3️⃣ Modificar el HTML para mostrar los datos
        let usuarioDiv = document.getElementById("usuario");
        usuarioDiv.innerHTML = `
            <p><strong>Nombre:</strong> ${usuario.name}</p>
            <p><strong>Usuario:</strong> ${usuario.username}</p>
            <p><strong>Email:</strong> ${usuario.email}</p>
            <p><strong>Ciudad:</strong> ${usuario.address.city}</p>
        `;
    } catch (error) {

        // 4️⃣ Manejo de errores.
        console.error("Error al obtener el usuario:", error); 
    }
}

// Llamamos a la función cuando cargue la página
obtenerUsuario();

``` 

#### 📌 Paso 1: Hacer la petición con fetch

```jsx title=""
let respuesta = await fetch("https://jsonplaceholder.typicode.com/users/1");
``` 
✔️ fetch() es una función nativa de JavaScript que realiza peticiones HTTP.
✔️ Como fetch() devuelve una promesa, usamos await para esperar la respuesta.
✔️ respuesta contendrá un objeto con información de la petición HTTP.

<br/>

#### 📌 Paso 2: Convertir la respuesta en JSON

```jsx title=""
let usuario = await respuesta.json();
``` 
✔️ respuesta.json() también devuelve una promesa, por eso usamos await.
✔️ Esto convierte la respuesta HTTP en un objeto JavaScript legible.
✔️ usuario ahora contiene los datos del usuario en formato JSON.


<br/>

#### 📌 Paso 3: Insertar los datos en el HTML

```jsx title=""
let usuarioDiv = document.getElementById("usuario");
usuarioDiv.innerHTML = `
    <p><strong>Nombre:</strong> ${usuario.name}</p>
    <p><strong>Usuario:</strong> ${usuario.username}</p>
    <p><strong>Email:</strong> ${usuario.email}</p>
    <p><strong>Ciudad:</strong> ${usuario.address.city}</p>
`;

``` 
✔️ Usamos document.getElementById("usuario") para obtener el < div > donde mostraremos la info.
✔️ innerHTML nos permite insertar HTML dinámicamente con los datos obtenidos.
✔️ Usamos interpolación con ${} para insertar los valores dentro del HTML.


<br/>

#### 📌 Paso 4: Manejo de errores con try...catch

```jsx title=""
catch (error) {
    console.error("Error al obtener el usuario:", error);
}

```
✔️ Si algo falla (por ejemplo, si la API está caída o la URL es incorrecta), capturamos el error.
✔️ El mensaje de error se muestra en la consola con console.error().


## **El Call Stack y el Event Loop en JavaScript**

### El Call Stack (Pila de llamadas)

El Call Stack (o pila de llamadas) es una estructura de datos en la que se registran las funciones que están siendo ejecutadas en un programa. Cada vez que se invoca una función, esta se apila en el Call Stack. Cuando una función termina de ejecutarse, se desapila, y el control se devuelve a la función anterior en la pila.

Tenemos dos casos:

**- Si una función llama a otra:** Sigue el orden LIFO (Last In, First Out), es decir la última función en entrar en la primera en salir, hasta liberar a la primera función de origne.

**- Si hay funciones que NO llaman a ninguna otra:** Cada función entra, se ejecuta y se libera de la pila inmediatamente, antes de que la siguiente función se ejecute. El orden sigue exactamente como fueron llamadas.



-   JavaScript agrega funciones a la pila cuando se ejecutan.

-   JavaScript las saca cuando terminan.

-   Siempre se ejecuta una función a la vez (JavaScript es single-threaded).



### El Event Loop

El Event Loop es el mecanismo que permite manejar tareas asincrónicas sin bloquear el Call Stack.

Funciona así: 

1️⃣ El Call Stack ejecuta código síncrono.

2️⃣ Si hay una tarea asincrónica (setTimeout, fetch, eventos), JavaScript la delega al Web API del navegador.

3️⃣ Cuando la tarea termina, su callback se envía a la Callback Queue.

4️⃣ El Event Loop revisa si el Call Stack está vacío y, si es así, mueve el callback a la pila para ejecutarlo.

**En Resumen: El Event Loop maneja el flujo de ejecución en JavaScript. Cuando hay un setTimeout, la función se envía a la Web API para ejecutarse en segundo plano. Una vez que el tiempo ha transcurrido, la función se mueve a la Task Queue. El Event Loop la ejecuta solo cuando el Call Stack está vacío, asegurando que las tareas síncronas se completen primero**

```jsx title="Ejemplo"
console.log("Inicio");

setTimeout(() => {
    console.log("Tarea Asíncrona");
}, 2000);

console.log("Fin");


``` 

```jsx title="Salida por consola"
Inicio
Fin
Tarea Asíncrona

``` 



🔎 Paso a paso en el Event Loop

1️⃣ console.log("Inicio") se ejecuta y se muestra en consola.

2️⃣ setTimeout() se envía al Web API del navegador, y JavaScript sigue ejecutando.

3️⃣ console.log("Fin") se ejecuta.

4️⃣ Después de 2 segundos, el callback de setTimeout() pasa a la Callback Queue.

5️⃣ El Event Loop verifica que el Call Stack esté vacío y mueve el callback al Call Stack.

6️⃣ console.log("Tarea Asíncrona") se ejecuta.


## **Temporizadores en JavaScript: setTimeout y setInterval**

En JavaScript, los temporizadores son funciones que permiten programar la ejecución de código en un momento futuro, ya sea una sola vez después de un retraso especificado o repetidamente a intervalos regulares. Los dos métodos principales para manejar temporizadores en JavaScript son setTimeout y setInterval.

### *setTimeout (Se ejecuta una sola vez)*

La función setTimeout ejecuta una función después de un período de tiempo especificado, expresado en milisegundos. Este método es útil para programar una tarea que debe realizarse después de un retraso, como mostrar un mensaje emergente o realizar una actualización en la interfaz de usuario.

```jsx title="Sintaxsis "
setTimeout(función, tiempoEnMilisegundos);
``` 

```jsx title="Ejemplo "
console.log("Inicio");
setTimeout(() => {
    console.log("Esto se ejecuta después de 2 segundos");
}, 2000);
```


#### 🚀 Cancelar setTimeout

Si queremos cancelar un setTimeout antes de que se ejecute, usamos clearTimeout

```jsx title=""
const temporizador = setTimeout(() => {
    console.log("Esto no se ejecutará");
}, 3000);

clearTimeout(temporizador); // Cancela el setTimeout antes de que se ejecute


``` 

<br/>

### *setInterval (Se ejecuta repetidamente)*

La función setInterval es similar a setTimeout, pero en lugar de ejecutar una función solo una vez después de un retraso, la ejecuta repetidamente en intervalos regulares. Esto es útil para tareas que necesitan realizarse periódicamente, como actualizar un reloj en pantalla o verificar el estado de una solicitud de red.

```jsx title="Sintaxsis "
setInterval(función, intervaloEnMilisegundos);
``` 

```jsx title="Ejemplo "
let contador = 1;

const intervalo = setInterval(() => {
    console.log(`Ejecutando cada 2 segundos: ${contador}`);
    contador++;

    if (contador > 5) {
        clearInterval(intervalo); // Detiene la repetición después de 5 ejecuciones
    }
}, 2000);

```

![settimeout-vs-setinterval](/img/setimeout-vs-setinterval.png)



## **Manejo de Erroes (Uso de Try-Catch-Finally)**

En el desarrollo de software, es crucial manejar correctamente los errores que puedan surgir durante la ejecución del código. JavaScript ofrece una estructura robusta para este propósito: el bloque try-catch-finally. Este bloque permite capturar y manejar errores de manera controlada, asegurando que tu aplicación pueda seguir funcionando incluso cuando se encuentra con problemas inesperados.


### *bloque try*

El bloque try contiene el código que puede lanzar una excepción. JavaScript intentará ejecutar todo el código dentro de este bloque. Si ocurre un error, la ejecución se detiene inmediatamente y el control pasa al bloque catch.

```jsx title=""
try {
    // Código que puede causar un error
}
``` 

### *bloque catch*

El bloque catch se ejecuta si ocurre una excepción en el bloque try. Aquí puedes definir cómo manejar los errores, ya sea mostrando un mensaje al usuario, intentando una solución alternativa, o registrando el error para una revisión posterior. El bloque catch recibe como parámetro el objeto de error, que contiene información sobre lo que salió mal.

```jsx title=""
 catch (error) {
    // Código para manejar el error
    console.error("Se ha producido un error:", error.message);
 }

``` 

### *bloque finally*

El bloque finally es opcional y se ejecuta después de que el bloque try y el bloque catch hayan terminado. Esto ocurre independientemente de si se lanzó una excepción o no. Es ideal para realizar tareas de limpieza, como cerrar conexiones o liberar recursos.

```jsx title=""
try {
    let x = 10;
    console.log("El valor de x es:", x);
} catch (error) {
    console.log("Ocurrió un error:", error.message);
} finally {
    console.log("Este código siempre se ejecuta");
}
``` 


## Ejemplo completo

Ejemplo usando 
-   async/await

-   Call Stack

-   Event Loop

-   setInterval

-   try-catch-finally


```jsx title="html"
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consumiendo API con Async/Await</title>
</head>
<body>

    <h1>📡 Datos del Usuario</h1>
    <p id="mensaje">Cargando datos...</p>  <!-- Mensaje de espera -->
    <div id="usuario"></div>  <!-- Aquí se insertarán los datos del usuario -->

    <script src="script.js"></script>  <!-- Enlazamos el archivo JavaScript -->
</body>
</html>



``` 

```jsx title="JavaScript"
// 1️⃣ Hacer la petición a la API
async function obtenerUsuario() {
    try {
        // 2️⃣ Simular la petición a la API con fetch() y la URL ficticia
        let respuesta = await fetch("https://jsonplaceholder.typicode.com/users/1");
        
        // 3️⃣ Convierte la respuesta en JSON
        let usuario = await respuesta.json();
        
        // 4️⃣ Modificar el HTML para mostrar los datos
        let usuarioDiv = document.getElementById("usuario");
        usuarioDiv.innerHTML = `
            <p><strong>Nombre:</strong> ${usuario.name}</p>
            <p><strong>Usuario:</strong> ${usuario.username}</p>
            <p><strong>Email:</strong> ${usuario.email}</p>
            <p><strong>Ciudad:</strong> ${usuario.address.city}</p>
        `;
        
        // Actualizamos el mensaje a "Datos cargados exitosamente"
        document.getElementById('mensaje').innerText = "✅ Los datos se cargaron con éxito";
        
        // 5️⃣ Detener el setInterval una vez que los datos estén cargados
        clearInterval(intervalo);
    } catch (error) {
        // 6️⃣ Manejo de errores
        console.error("Error al obtener los datos:", error);
        document.getElementById('mensaje').innerText = "❌ Hubo un error al cargar los datos.";
        
        // 7️⃣ Detener el setInterval en caso de error también
        clearInterval(intervalo);
    }
}

// 8️⃣ Mostrar mensajes cada 5 segundos
let contador = 1;
let intervalo = setInterval(() => {
    // Mostrar el mensaje de espera en el HTML
    document.getElementById('mensaje').innerText = `⌛ Esperando respuesta... ${contador}`;
    contador++;
    
    // Si han pasado 5 segundos, detener el setInterval
    if (contador > 5) {
        clearInterval(intervalo);
    }
}, 5000);

// 9️⃣ Llamamos a la función para obtener los datos
obtenerUsuario();

``` 


## **Concepto de Promesas en JavaScript**

Una Promesa en JavaScript es un objeto que representa la eventual finalización (o fracaso) de una operación asíncrona y su valor resultante. Las Promesas son esenciales para manejar operaciones asíncronas como solicitudes a APIs o temporizadores, permitiendo que el código funcione de manera no bloqueante.

### Estados de una Promesa

Una Promesa puede encontrarse en uno de los siguientes tres estados:

**Pending (Pendiente):** Es el estado inicial. La promesa está en proceso, lo que significa que aún no ha sido cumplida ni rechazada.

**Fulfilled (Resuelta):** La operación se completó con éxito, y la promesa tiene un valor resultante.

**Rejected (Rechazada):** La operación falló, y la promesa tiene una razón para el fallo, generalmente un error. 

### Sintaxis

Una promesa en JavaScript se crea usando el constructor new Promise(), que recibe una función ejecutora con dos parámetros:

**resolve:** Se llama cuando la operación tiene éxito.

**reject:** Se llama cuando la operación falla.

#### Ejemplo
```jsx title=""
const miPromesa = new Promise((resolve, reject) => {
    setTimeout(() => {
        let exito = true;
        if (exito) {
            resolve("Operación exitosa ✅");
        } else {
            reject("Error en la operación ❌");
        }
    }, 2000);
});

```


### Manejo de estados con then(), catch() y finally()

Para trabajar con los resultados de una Promesa, utilizamos los siguientes métodos:

**then(onFulfilled):** Este método se ejecuta cuando la promesa se resuelve con éxito. Recibe una función que será llamada con el valor resultante de la promesa.

```jsx title=""
promesa.then((resultado) => {
    console.log("Promesa resuelta con valor:", resultado);
});
``` 
<br/>

**catch(onRejected):** Este método se ejecuta cuando la promesa es rechazada. Recibe una función que será llamada con la razón del rechazo.

```jsx title=""
promesa.catch((error) => {
    console.error("Promesa rechazada con error:", error);
});
``` 

<br/>

**finally(onFinally):** Este método se ejecuta independientemente de si la promesa fue resuelta o rechazada. Es útil para ejecutar código de limpieza o de finalización, sin importar el resultado de la promesa.

```jsx title=""
promesa.finally(() => {
    console.log("Promesa finalizada, sea cual sea el resultado.");
});
``` 

## Ejemplo completo

Imaginemos una función que simula una solicitud de datos a una API utilizando un setTimeout para simular la demora en la respuesta:

```jsx title=""
const obtenerDatos = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const exito = true; // Cambia esto para probar diferentes resultados
            if (exito) {
                resolve("Datos obtenidos correctamente");
            } else {
                reject("Error al obtener los datos");
            }
        }, 2000);
    });
};

obtenerDatos()
    .then((resultado) => {
        console.log(resultado);
    })
    .catch((error) => {
        console.error(error);
    })
    .finally(() => {
        console.log("Operación completada.");
    });
```
En este ejemplo, obtenerDatos() devuelve una promesa que se resuelve o rechaza después de 2 segundos. Dependiendo del valor de exito, la promesa será resuelta o rechazada. El then() maneja el resultado exitoso, catch() maneja cualquier error, y finally() se ejecuta al final de cualquier manera.