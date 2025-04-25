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

![angular-vs-angularjs](/img/angular-vs-angularjs.png)

<br/>


![angular-vs-frameworks](/img/angular-vs-frameworks.png)


<br/>

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

https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2

<br/><br/>

**- Iconos :** Cambia los íconos de archivos y carpetas en VS Code para identificar más fácilmente archivos de Angular, TypeScript, HTML, etc.

https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme

<br/><br/>

**- Angular Snippets:** Proporciona atajos de código (snippets) para generar rápidamente estructuras comunes de Angular (componentes, directivas, servicios, etc.).

https://marketplace.visualstudio.com/items?itemName=Mikael.Angular-BeastCode

<br/><br/>

**- Angular Language Service:** Ofrece autocompletado, sugerencias y resaltado de sintaxis en archivos Angular (.html y .ts), mejorando la productividad.

https://marketplace.visualstudio.com/items?itemName=Angular.ng-template

<br/><br/>

**- Angular Inline:** Permite ver y editar código HTML y CSS directamente en archivos TypeScript sin necesidad de abrir múltiples archivos.

https://marketplace.visualstudio.com/items?itemName=natewallace.angular2-inline

<br/><br/>

**- Auto Close Tag:** Cierra automáticamente las etiquetas HTML y XML, evitando errores al escribir código en Angular.

https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag

<br/><br/>


**- TypeScript Importer:**  Facilita la importación automática de módulos y clases TypeScript cuando escribes código, ahorrando tiempo.

https://marketplace.visualstudio.com/items?itemName=pmneo.tsimporter

<br/><br/>


----------------------------------------------------------------------


### Instalaciones de NodeJs y creación Proyecto Angular

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
npm install @angular/cli -g 
```

2. Verificamos versión.

```jsx title="Terminal node"
ng version
```

3. Creamos un proyecto nuevo

```jsx title="Terminal node"
ng new mi-proyecto --no-standalone

// no standalone: Angular con módulos.
```
Pedirá opciones como el uso de SCSS o CSS, y si deseas incluir routing.


4. Seleccionamos la hoja de estilos que queres (Se recomienda SASS(SCSS))

![angular-creacion-proyecto-sass](/img/angular-creacion-proyecto-sass.png)

5. Luego nos preguntará por SSG/Prerendering, seleccionamos NO. 

6. Ejecutar la aplicación en el navegador:

```jsx title="Terminal node"
cd mi-proyecto
ng serve --open

```


## Instalación de Bootstrap

1. Instalación con npm

```jsx title="npm"
npm install bootstrap@5.3.5
``` 

2. Modificamos el angular.json

Debemos referencia al documento main de stylos de bootstrap en nuestra configuración de angular

![bs-angular](/img/bs-angular.png)


3. Reiniciamos el servidor


<br/>


## Instalación Angular Material

1. Instalamos con npm

```jsx title="Instalación angular material"
ng add @angular/material
``` 

- Nos indicará si queremos instalar la versión xxx. Seleccionamos que si

- Luego elegimos la opción "custom" de estilos visuales

- Luego nos preguntará si queremos usar la tipografía de angular material. Seleccionamos que si


<br/>

## Estructura de un proyecto Angular

```jsx title=""
[ App Module ] --> [ Components ] <--> [ Services ] | [ Templates ]

``` 



-   App Module: Contiene los módulos principales y dependencias de la aplicación.

-   Components: Definen las interfaces de usuario y la lógica específica de cada vista.

-   Services: Gestionan la lógica de negocio y el acceso a datos.


Cuando se genera un proyecto con Angular CLI, se crea una estructura organizada que facilita el desarrollo y mantenimiento. A continuación, se describen los archivos y carpetas clave, así como sus funciones principales.

#### Carpetas Principales

**src/**

-   **Descripción:** Carpeta principal donde se encuentra todo el código fuente del proyecto.

-   **Contenido:**  Contiene subcarpetas y archivos esenciales para la aplicación.
<br/>

**src/assets/**

-   **Descripción:** Destinada a almacenar recursos estáticos como imágenes, fuentes y otros archivos que no cambian durante la ejecución.

<br/>

**src/environments/**

-   **Descripción:** Contiene archivos de configuración para diferentes entornos (por ejemplo, desarrollo y producción).

-   **Uso:** Facilita la gestión de variables específicas para entornos de desarrollo y producción.

<br/>

#### Archivos Clave

1.  **app.module.ts**

-   **Descripción:** Es el módulo raíz de la aplicación.

-   **Funcionalidad:** 
    -   Define los componentes, directivas y servicios que se utilizan en el proyecto.

    -   Importa otros módulos necesarios y declara las dependencias.


Ejemplo de uso:
Si se agrega un nuevo componente, este debe ser declarado en app.module.ts para que Angular lo reconozca.

<br/>

2.   **app.component.ts**

-   **Descripción:** Es el componente principal de la aplicación.

-   **Funcionalidad:**
    -   Define la lógica y estructura de la vista inicial que se muestra al usuario.

    -   **Contiene:**
        -   **Decorador @Component:** Describe metadatos como el selector y las rutas de los archivos HTML y CSS asociados.

        -   **Clase TypeScript:** Gestiona la lógica del componente.

Ejemplo de uso:
El texto o elemento visible en la pantalla inicial de la aplicación suele estar definido en este componente.

<br/>

3.   **index.html**

-   **Descripción:** Archivo HTML principal que sirve como contenedor para la aplicación Angular.

-   **Funcionalidad:**
    -   Contiene la etiqueta < app-root > que referencia el componente raíz.

    -   Este archivo no se modifica directamente para agregar contenido, ya que Angular inyecta dinámicamente los componentes en el DOM.


Ejemplo de uso:
Este archivo actúa como un punto de entrada donde se carga la aplicación Angular.


<br/>

4.   **main.ts**

-   **Descripción:** Archivo de entrada de la aplicación.

-   **Funcionalidad:**
    -   Inicializa el módulo raíz (AppModule) y arranca la aplicación.

    -   Define cómo se renderiza la aplicación en el navegador.


<br/>

#### Flujo Básico de una Aplicación Angular

-   El navegador carga index.html.

-   index.html referencia el componente raíz (< app-root >) que está definido en app.component.ts.

-   main.ts se encarga de inicializar AppModule y arrancar la aplicación.


