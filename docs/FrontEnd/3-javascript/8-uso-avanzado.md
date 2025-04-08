---
sidebar_position: 8
---

# 8 - Uso avanzando
----

## Trabajo con Librerías

### ¿Qué son las librerías en JavaScript?

Las librerías en JavaScript son colecciones de código preescrito que desarrolladores pueden integrar en sus proyectos para resolver problemas comunes de manera más rápida y eficiente. Estas librerías actúan como "cajas de herramientas", proporcionando funciones y métodos que simplifican tareas que, de otro modo, requerirían escribir y depurar mucho código desde cero.

<br/>

### ¿Cómo funcionan las librerías?

Una librería se puede imaginar como un conjunto de herramientas especializadas. Al incorporarlas en un proyecto, se pueden utilizar estas herramientas para realizar tareas específicas, lo que no solo ahorra tiempo, sino que también mejora la calidad y la eficiencia del desarrollo. 
Por ejemplo, en lugar de programar desde cero la validación de formularios, una librería especializada puede ofrecer funciones ya probadas y optimizadas para validar entradas de usuarios

<br/>

### Aplicaciones comunes de las librerías

Algunas de las áreas donde las librerías de JavaScript se utilizan con frecuencia incluyen:

**Validación de datos:** Asegurar que los datos ingresados por el usuario cumplan con ciertos criterios antes de ser procesados.

**Interfaces visuales:** Mejora de la presentación y la experiencia del usuario mediante elementos visuales interactivos, como sliders, modales, y alertas personalizadas.

**Manejo de fechas:** Facilitar la manipulación de fechas y horas, como formateo, comparación y cálculo de intervalos.

**Peticiones AJAX:** Simplificar la comunicación con servidores para enviar y recibir datos de manera asíncrona sin recargar la página completa.

<br/>

### Ejemplos de librerías populares

**jQuery:** Facilita la manipulación del DOM y maneja eventos, animaciones, y peticiones AJAX.

**Sweet Alert:** Permite la creación de alertas visualmente atractivas y personalizables que mejoran la experiencia del usuario.

**Luxon:** Ofrece herramientas para manejar fechas y horas de forma sencilla y potente.

<br/>

### Uso de CDN para Vincular Librerías

Un CDN es una red de distribución de contenido que permite acceder a archivos hospedados en servidores distribuidos globalmente. Para vincular una librería a través de un CDN, simplemente se debe incluir un < script > en el archivo HTML que apunte a la URL del CDN.

Ejemplo con SweetAlert:

```jsx title=""
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
``` 

<br/>

### Uso de Archivos Minificados
Los archivos minificados son versiones optimizadas de los archivos JavaScript, donde se han eliminado espacios, comentarios y saltos de línea para reducir el tamaño del archivo, mejorando así el tiempo de carga.

```jsx title=""
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11/sweetalert2.min.js"></script>
``` 
Aquí se está utilizando la versión minificada de SweetAlert, lo que asegura un rendimiento óptimo al incluir la librería en tu proyecto.

<br/>

### Vinculación de Librerías con Fetch

En algunos casos, es posible que quieras utilizar Fetch para descargar dinámicamente una librería y ejecutarla en tu proyecto. Esto puede ser útil si necesitas cargar librerías bajo ciertas condiciones o en respuesta a eventos específicos.

Ejemplo básico con Fetch:
```jsx title=""
jsx fetch("https://cdn.jsdelivr.net/npm/sweetalert2@11/sweetalert2.min.js") .then((response) => response.text()) .then((script) => { const scriptTag = document.createElement("script"); scriptTag.textContent = script; document.body.appendChild(scriptTag); Swal.fire("Librería cargada con éxito!"); }) .catch((error) => console.error("Error cargando la librería:", error));
``` 

Este código utiliza Fetch para descargar la librería SweetAlert desde un CDN y luego inyectarla en la página como un script. Una vez cargada, la librería puede ser utilizada inmediatamente.


## Alertas y Notificaciones

### Configuración de Toastify

Toastify es una librería ligera de JavaScript que permite crear notificaciones o "toasts" personalizadas y visualmente atractivas en una aplicación web. Estas notificaciones son ideales para mostrar mensajes temporales que no requieren interacción del usuario. 
A continuación, se explica cómo configurar y personalizar Toastify, enfocándonos en propiedades clave como gravity, position, y style.

#### 1. Instalación de Toastify

Antes de empezar a configurar Toastify, es necesario integrarlo en tu proyecto. Puedes hacerlo de dos maneras: descargando la librería o vinculándola directamente desde un CDN.

Vinculación desde un CDN:

```jsx title=""
<script src="<https://cdn.jsdelivr.net/npm/toastify-js>"></script>
``` 

<br/>

### 2. Creación de una Notificación Básica

Las options son un objeto que define las propiedades de la notificación.

```jsx title="Método"
Toastify({
  text: "¡Esta es una notificación básica!",
}).showToast();

``` 
<br/>

### 3. Propiedad gravity
La propiedad gravity define la verticalidad de la notificación, es decir, si aparecerá en la parte superior o inferior de la pantalla.

gravity: "top": Coloca la notificación en la parte superior (valor por defecto).

gravity: "bottom": Coloca la notificación en la parte inferior.

```jsx title=""
Toastify({
  text: "Notificación en la parte inferior",
  gravity: "bottom", // Aparece en la parte inferior
}).showToast();
``` 

<br/>

#### 4. Propiedad position

La propiedad position se utiliza para definir la horizontalidad de la notificación dentro de la pantalla, especificando si aparecerá a la izquierda, centro o derecha.

position: "left": Alinea la notificación a la izquierda.

position: "center": Centra la notificación horizontalmente.

position: "right": Alinea la notificación a la derecha (valor por defecto).

```jsx title=""
Toastify({
  text: "Notificación centrada",
  gravity: "top", // Aparece en la parte superior
  position: "center", // Centra la notificación horizontalmente
}).showToast();
``` 

<br/>

#### 5. Propiedad style

La propiedad style permite aplicar estilos CSS personalizados a la notificación, como colores de fondo, fuente, bordes, etc.

```jsx title=""
Toastify({
  text: "Notificación personalizada",
  gravity: "top",
  position: "right",
  style: {
    background: "linear-gradient(to right, #00b09b, #96c93d)",
    color: "white",
    fontWeight: "bold",
  }
}).showToast();
``` 


Toastify es una herramienta poderosa y flexible para añadir notificaciones a tus aplicaciones web. Mediante el uso de propiedades como gravity, position, y style, puedes personalizar completamente la apariencia y ubicación de las notificaciones, asegurando que se adapten a las necesidades específicas de tu proyecto. Estas configuraciones permiten mejorar significativamente la experiencia del usuario, manteniéndolo informado de manera sutil y no intrusiva.


### Sweet Alert (Recomendado)

#### Instalación 

```jsx title="bash"
npm install sweetalert2
``` 


```jsx title="script"
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
``` 


#### URL  

https://sweetalert2.github.io/