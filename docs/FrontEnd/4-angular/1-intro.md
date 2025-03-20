---
sidebar_position: 1
---

# 1 - Introducción

Angular es un framework de desarrollo web creado por Google que se usa para construir aplicaciones web de una sola página (Single Page Applications - SPA).

### ¿Qué es un framework?

Es un conjunto de herramientas y reglas que facilitan el desarrollo de aplicaciones, proporcionando una estructura organizada y reutilizable.

### ¿Qué es una Single Page Application (SPA)?

Una Single Page Application (SPA) o Aplicación de Página Única es un tipo de aplicación web que carga una sola página HTML y actualiza dinámicamente su contenido sin necesidad de recargar la página completa.

##### ¿Cómo funciona?

Cuando entras a una SPA, el navegador carga una sola vez la estructura básica de la aplicación. Luego, a medida que navegas entre secciones, solo se actualizan las partes necesarias, sin volver a pedir todo el HTML desde el servidor.


### ¿Que lenguaje usa de programación usa Angular?

Angular usa TypeScript como lenguaje de programación principal.



### Angular CLI (Command Line Interface)

Angular CLI es una herramienta de línea de comandos que facilita la creación y gestión de proyectos en Angular.

💡 En pocas palabras: Angular CLI te permite crear, ejecutar y administrar aplicaciones Angular rápido y sin complicaciones, con solo escribir comandos en la terminal.

📌 ¿Qué puedes hacer con Angular CLI?

✅ Crear un proyecto Angular nuevo

✅ Generar componentes, servicios, módulos y más

✅ Ejecutar y probar la aplicación fácilmente

✅ Optimizar el código para producción

✅ Agregar librerías y configuraciones automáticamente



### Visual Studio Code - Extensiones

**- Bracket Pair Colorizer 2:** Resalta los pares de corchetes {}, [], () con colores diferentes para facilitar la lectura del código.

**- Iconos :** Cambia los íconos de archivos y carpetas en VS Code para identificar más fácilmente archivos de Angular, TypeScript, HTML, etc.

**- Angular Snippets:** Proporciona atajos de código (snippets) para generar rápidamente estructuras comunes de Angular (componentes, directivas, servicios, etc.).

**- Angular Language Service:** Ofrece autocompletado, sugerencias y resaltado de sintaxis en archivos Angular (.html y .ts), mejorando la productividad.

**- Angular Inline:** Permite ver y editar código HTML y CSS directamente en archivos TypeScript sin necesidad de abrir múltiples archivos.

**- Auto Close Tag:** Cierra automáticamente las etiquetas HTML y XML, evitando errores al escribir código en Angular.

**- TypeScript Importer:**  Facilita la importación automática de módulos y clases TypeScript cuando escribes código, ahorrando tiempo.



### Instalaciones

#### Node Js.

Descargar desde el siguiente enlace:

https://nodejs.org/en

<br/>

2. Comprobar si está instalado

```jsx title="Ejemplo"
npm -v
```

<br/>

#### Angular-CLI

1. Instalamos mediante la terminal de Node.

```jsx title="Terminal node"
npm install -g @angular/cli
```

2. Verificamos versión.

```jsx title="Terminal node"
ng version
```

3. Creamos un proyecto nuevo

```jsx title="Terminal node"
ng new mi-proyecto
```
Pedirá opciones como el uso de SCSS o CSS, y si deseas incluir routing.

4. Ejecutar la aplicación en el navegador:

```jsx title="Terminal node"
cd mi-proyecto
ng serve --open

```



