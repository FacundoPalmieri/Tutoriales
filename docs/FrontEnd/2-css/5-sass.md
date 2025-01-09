---
sidebar_position: 5
---

# 5 - SASS
---


## **Configuraciones**

### *Instalación de Node.JS*

Descargar desde la siguiente URL: https://nodejs.org/es

Luego desde la terminal usar el siguiente comando para verificar la versión desde el CMD del sistema.

```jsx title=""
node --version
```

En caso de tener un algún error, verificar:

-   Variable de entorno del sistema

-   Ejecutar desde PowerShell -->  $env:Path += ";C:\Program Files\nodejs\"


### *Instalación de SASS*

1. Abrir el proyecto en visual studio code

2. Crear una carpeta scss con un archivo estilos.scss en su interior.
  - Luego se generará automática una réplica de este archivo en formato css.

![scss-1](/img/scss-1.png)


3. Desde la terminal de visual studio code, **en PowerShell**, ejectuar el comando --> **npm init**

4. Presionamos varias veces ENTER, hasta que termine de instalar y aparezca nuevamente la ruta para ingresar un nuevo comando.

5.  En la consola ejecutar el comando -->  **sass --watch scss:css**
  -   Primer archivo origen, segundo archivo destino.
  -   Vinculamos el archivo original *scss* con el la réplica *css*.

Esto permitirá traducir de manera automática al archivo css lo que se escriba en el scss.

----------


## **Introducción a SASS**

SASS (Syntactically Awesome Style Sheets) es una extensión de CSS que añade características avanzadas y potentes a las hojas de estilo, permitiendo a los desarrolladores escribir código CSS de una manera más eficiente y mantenible. 

Siempre trabajaremos sobre el archivo **scss**, que luego será transcripto automáticamente al archivo **css** para que pueda ser interpretado por el navegador.


### *Sintaxis SASS*

SASS tiene dos sintaxis principales: 

**SCSS:** Es similar a la sintaxis de CSS y permite importar archivos CSS directamente.

    -   Utiliza llaves {} para agrupar bloques de código.

    -   Usa punto y coma ; para separar declaraciones.


```jsx title="SCSS"
$primary-color: #3498db;

body {
  color: $primary-color;
}

header {
    background-color: $primary-color;
    nav {
        padding: 20px;
        ul {
            list-style: circle;
            li {
                text-decoration:underline;
            }
        }
    }
}

```

<br/><br/>         

**SASS:** La sintaxis de indentación, conocida como SASS, utiliza la indentación y los saltos de línea en lugar de corchetes y punto y coma.

    -   Usa la indentación para indicar la jerarquía y agrupación.

    -   No utiliza llaves {} ni punto y coma ;.





```jsx title="SASS"
$primary-color: #3498db;

body 
  color: $primary-color


header 
    background-color: $primary-color
    nav 
        padding: 20px
        ul 
            list-style: circle
            li 
                text-decoration:underline

```

<br/><br/>

### *Declaración de variables y listas*

Se declaran con el símbolo $ y se utilizan directamente con el nombre de la variable.

```jsx title="_operaciones.scss"
$primary-color: #3498db;

```


```jsx title="destino"
body {
  color: $primary-color;
}

```



- También pueden realizarse listas. Las mismas comienzan desde el indice 1.

```jsx title="_operaciones.scss"
$colores: red, green, blue;

```


```jsx title="destino"
body {
   color:nth($colores, 2) // Se llama al green.
}

```


<br/><br/>

### *Mapas*

Los mapas son estructuras de datos que permiten almacenar pares **clave-valor**. Son similares a los objetos en JavaScript o los diccionarios en otros lenguajes de programación. Los mapas son útiles para organizar y acceder a datos estructurados de manera eficiente


Un mapa en SASS se declara utilizando paréntesis () y contiene pares de claves y valores separados por dos puntos :

Los mapas pueden ser utilizados para almacenar variables relacionadas, configuraciones de temas, conjuntos de colores, etc.

#### Sintaxis Básica

```jsx title="scss"
$map: (clave1: valor1, clave2: valor2, clave3: valor3);

```


```jsx title="scss"
$map --> El nombre de la variable que almacena el mapa.

clave1, clave2, clave3 --> Las claves del mapa.

valor1, valor2, valor3 --> Los valores asociados a las claves.

```


#### Ejemplo

#### Declaración de un Mapa de Colores

```jsx title="scss"
$colors: (
  primary: #3498db,
  secondary: #2ecc71,
  error: #e74c3c,
  success: #2ecc71
);

```

#### Acceso a Valores del Mapa

Se puede acceder a los valores de un mapa utilizando la función **map-get**.

```jsx title="scss"
body {
  background-color: map-get($colors, primary);
  color: map-get($colors, success);
}

map-get($colors, primary): Devuelve el valor asociado a la clave primary en el mapa $colors.

```


### *Uso de Mapas en Bucles*

```jsx title="scss"
//Mapa
$colors: (
  primary: #3498db, (clave:valor)

  // primary = clave "name"
  // #3498db = valor "color"

);



@each $name, $color in $colors {
  .text-#{$name} {
    color: $color;
  }
}

```


```jsx title=""
@each $name, $color in $colors: Itera sobre cada par clave-valor en el mapa $colors.

.text-#{$name}: Crea una clase basada en el nombre de la clave.

color: $color: Aplica el color correspondiente a cada clase.
```


<br/><br/>

### *@extend*

@extend en SASS permite compartir un conjunto de reglas CSS de un selector a otro.

Esto es útil  ya que permite reutilizar estilos comunes sin duplicarlos. Cuando un selector extiende a otro, incluye todas las reglas de estilo del selector extendido, como si fueran propias.

:::tip
**Podemos relacionar el extend con el concepto de Herencia**
:::

#### Ejemplo

Supongamos que tenemos un conjunto de estilos para un mensaje básico y queremos reutilizar esos estilos en mensajes específicos como success y error.


```jsx title="scss"
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

```

Uso de @extend para Reutilizar Estilos

```jsx title="scss"
.success {
  @extend .message; // Hereda todas las propiedades de .message
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

```

Explicación

**.message:**  Define un conjunto base de reglas CSS para los mensajes en general.

**@extend .message:** La clase .success y .error están extendiendo la clase .message, lo que significa que heredan todas sus reglas.

**Estilos Adicionales:** Las clases .success y .error añaden sus propias reglas específicas, como cambiar el color del borde.

Resultado Final en CSS
El código SASS anterior se compilará en CSS de la siguiente manera:

```jsx title="css"
.message, .success, .error {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

```



### *@mixin*

En SASS, un mixin es un bloque de código que se puede definir una vez y reutilizar en múltiples lugares. Los mixins permiten incluir grupos de declaraciones CSS en otros selectores, y pueden aceptar argumentos para que sean más flexibles. Esto ayuda a evitar la repetición de código y hace que el CSS sea más modular y fácil de mantener.

:::tip
**Podemos relacionar el mixin con el concepto de Función**
:::

En este ejemplo se declara primero una función que se va a encargar de dar un borde al radio de un elemento. Recibirá por parámetro el valor y luego lo asignará a la propiedad.


```jsx title="Declaración"

@mixin border-radius($radius){
    border-radius: $radius;
}
```


Desde aquí, se llama a la función y se pasa el valor. 

```jsx title="Llamado"

.box{
    @include border-radius(10px)
}
```



#### *Cuándo Usar @extend y Cuándo Usar @mixin*

**@extend:** Cuando tengas un conjunto de reglas de estilo que necesites compartir entre varios selectores sin necesidad de parametrización, es decir, heredar estilos comunes sin necesidad de parámetros

**@mixin:** Cuando necesites reutilizar reglas de estilo que requieran parámetros o cuando necesites incluir un bloque de estilos en múltiples lugares.

<br/><br/>


### *Condicionales*

En SASS, los condicionales te permiten aplicar lógica a tus estilos, lo que hace que tu CSS sea más dinámico y adaptable. 

Se manejan de manera similar a los de otros lenguajes de programación y se implementan con la directiva @if. se utilizan para ejecutar diferentes bloques de código CSS en función de condiciones específicas. Esto permite personalizar los estilos de acuerdo con diversas variables o situaciones.

#### Sintaxis básica

**@if:** Evalúa una condición y ejecuta el bloque de código si la condición es verdadera.

**@else if:** Evalúa otra condición si la primera @if no es verdadera.

**@else:** Ejecuta el bloque de código si ninguna de las condiciones anteriores es verdadera.


#### Ejemplo

Supongamos que tenemos una variable que define un tema de color y queremos aplicar diferentes estilos según el valor de esta variable.

```jsx title="scss"
$theme: dark;

body {
  @if $theme == light {
    background-color: #ffffff;
    color: #000000;
  } @else if $theme == dark {
    background-color: #000000;
    color: #ffffff;
  } @else {
    background-color: #f0f0f0;
    color: #333333;
  }
}

```

#### Explicación

**Variable $theme:** Define el tema actual (en este caso, dark).

**Condicional @if:** Evalúa el valor de $theme.

-   Si $theme es light, se aplican los estilos correspondientes (fondo blanco y texto negro).

-   Si $theme es dark, se aplican los estilos para el tema oscuro (fondo negro y texto blanco).

-   Si $theme no es ni light ni dark, se aplican los estilos por defecto (fondo gris claro y texto gris oscuro).

<br/><br/>

### **Bucles**

Los bucles en SASSpermiten iterar sobre una lista de valores o un rango de números, lo que hace que la creación de estilos repetitivos sea más eficiente y menos propensa a errores.


**@for:** Repite un bloque de código un número determinado de veces.

**@each:** Itera sobre una lista de valores o un mapa.

**@while:** Repite un bloque de código mientras se cumpla una condición

<br/><br/>

### *@for*

El bucle @for te permite iterar sobre un rango de números.

```jsx title="Sintaxis"
@for $variable from <start> through <end> {
  // Código a repetir
}
```

```jsx title="Explicación"
@for --> Directiva que inicia el bucle.

$variable --> La variable del contador del bucle, que cambia en cada iteración.

from --> Palabra clave que indica el inicio del rango.

<start> --> Valor inicial del contador del bucle.

through --> Palabra clave que indica el final del rango (incluye el valor final).

<end> --> Valor final del contador del bucle.

{} --> Llaves que encierran el bloque de código que se repetirá.
```

<br/>

En este ejemplo se utiliza  para generar clases de columnas con anchos variables:

```jsx title="scss"
@for $i from 1 through 12 {
  .col-#{$i} {
    width: (100% / 12) * $i;
  }
}

```
#### Explicación
```jsx title=""
@for $i from 1 through 12 --> Inicia un bucle que se ejecutará desde 1 hasta 12.

.col-#{$i} --> Utiliza la interpolación #{$i} para generar clases como .col-1, .col-2, ..., .col-12.

width: (100% / 12) * $i --> Calcula el ancho de cada columna basado en el índice del bucle.
```

<br/><br/>

### *@each*

El bucle @each se usa para iterar sobre listas o mapas.

```jsx title="Sintaxis"
@each $variable in <list> {
  // Código a repetir
}

```


```jsx title="Explicación"
@each --> Directiva que inicia el bucle.

$variable --> La variable que toma cada valor de la lista en cada iteración.

in --> Palabra clave que indica la lista a iterar.

<list> --> La lista o mapa a iterar.

{} --> Llaves que encierran el bloque de código que se repetirá.

```

<br/>

Ejemplo con una lista de colores:

```jsx title="scss"
$colors: red, green, blue, yellow;

@each $color in $colors {
  .text-#{$color} {
    color: $color;
  }
}

```


#### Explicación

```jsx title=""
$colors: red, green, blue, yellow; --> Lista de colores.

@each $color in $colors --> Inicia un bucle que itera sobre cada color en la lista $colors.

.text-#{$color} --> Utiliza la interpolación #{$color} para generar clases como .text-red, .text-green, etc.

color: $color --> Aplica el color correspondiente a cada clase.

```


<br/><br/>

### *@while*

El bucle @while repite un bloque de código mientras se cumpla una condición. 


```jsx title="Sintaxis"
@while <condition> {
  // Código a repetir
}

```


```jsx title="Explicación"
@while --> Directiva que inicia el bucle.

<condition> --> La condición que se evalúa antes de cada iteración. Mientras sea verdadera, el bucle continúa.

{} --> Llaves que encierran el bloque de código que se repetirá.
```


<br/>


Ejemplo simple:

```jsx title="scss"
$i: 1;

@while $i <= 5 {
  .item-#{$i} {
    font-size: 10px * $i;
  }
  $i: $i + 1;
}

```


#### Explicación

```jsx title=""
$i: 1: Inicializa la variable $i con el valor 1.

@while $i <= 5: Inicia un bucle que se ejecutará mientras $i sea menor o igual a 5.

.item-#{$i}: Utiliza la interpolación #{$i} para generar clases como .item-1, .item-2, etc.

font-size: 10px * $i: Aplica un tamaño de fuente que aumenta con cada iteración.

$i: $i + 1: Incrementa el valor de $i en 1 con cada iteración.
```

<br/><br/>

#### Ventajas de Usar Bucles en SASS

**Eficiencia:** Permiten generar estilos repetitivos de manera automática, ahorrando tiempo y esfuerzo.

**Mantenibilidad:** Facilitan la actualización de estilos, ya que los cambios se pueden hacer en un solo lugar.

**Dinamismo:** Hacen que el código CSS sea más dinámico y adaptable a diferentes condiciones.


### *Condicional y Bucles - Caso práctico.*

Vamos a crear un conjunto de clases para definir el tamaño de fuente basado en una condición de tamaño de pantalla y utilizar un bucle para generar múltiples tamaños.

```jsx title="scss"
$screen-size: large; // Cambiar esta variable a "small" para ver el efecto

@for $i from 1 through 5 {
  @if $screen-size == large {
    .font-size-#{$i} {
      font-size: 20px * $i;
    }
  } @else {
    .font-size-#{$i} {
      font-size: 10px * $i;
    }
  }
}

```


#### Explicación
```jsx title=""
1.  Variable $screen-size:

    -   Define el tamaño de pantalla. Puedes cambiar su valor a "small" o "large" para ver cómo afectan los estilos.

2.  Bucle @for:

    -   Itera desde 1 hasta 5 para generar clases .font-size-1, .font-size-2, hasta .font-size-5.

3.  Condicional @if:

    -   Comprueba el valor de $screen-size.

    -   Si $screen-size es large, se aplica un tamaño de fuente de 20px * $i.

    -   Si $screen-size es otro valor (como small), se aplica un tamaño de fuente de 10px * $i.
```


#### Resultado CSS
```jsx title="Si $screen-size es large"
.font-size-1 {
  font-size: 20px;
}

.font-size-2 {
  font-size: 40px;
}

.font-size-3 {
  font-size: 60px;
}

.font-size-4 {
  font-size: 80px;
}

.font-size-5 {
  font-size: 100px;
}


```


```jsx title="Si $screen-size es small:"
.font-size-1 {
  font-size: 10px;
}

.font-size-2 {
  font-size: 20px;
}

.font-size-3 {
  font-size: 30px;
}

.font-size-4 {
  font-size: 40px;
}

.font-size-5 {
  font-size: 50px;
}

```




----------------
## **Organización de archivos**

Siguiendo las buenas prácticas, lo más habitual es tener un archivo principal **style.scss** que contenga tus estilos en SASS, y luego compilar este archivo en un **archivo style.css** que el navegador puede interpretar. 

### ¿Por qué usar style.scss y style.css?

#### Compatibilidad.

-   Los navegadores no entienden SASS directamente, por lo que necesitas compilar tus archivos SASS (.scss o .sass) en CSS (.css) antes de usarlos en tu sitio web.

-   Esto se hace utilizando un compilador de SASS que convierte el archivo style.scss en style.css.


#### Organización y mantenimiento.

-   SASS permite dividir los estilos en múltiples archivos parciales (partials) y luego importarlos en un archivo principal. Esto hace que el código sea más modular y fácil de mantener.

1. Dentro de la carpeta **scss**, generaremos dos nuevas carpetas.
  - bases.
  - components.

![scss-2](/img/scss-2.png)


2. Dentro de la carpeta **bases** crearemos los siguientes archivos:

- _generales.scss
- _operaciones.scss
- _queries.scss.

![scss-3](/img/scss-3.png)

* Se coloca el guión bajo para identificar que son archivos parciales.

**generales** : Irá todo lo común al proyecto.
  - reset
  - Fuentes

**operaciones** :  Todo lo relacionado a ciclos, condicionales, etc.

**queries**: Las medias queries para adaptación mobile.

3. Dentro de la carpeta **components** crearemos los siguientes archivos:

- _header.scss (Si usamos boostrap no hace falta crearlo)
- _main.scss
- _footer.scss


### *Vinculación de archivo parciales a style.scss*

A través de:  @import + "./nombreArchivo.scss" + ;  

(El nombre del archivo va sin guiones bajos)

```jsx title="_variables.scss"
$primary-color: #3498db;

```

```jsx title="style.scss"

@import "./variables.scss";

```
