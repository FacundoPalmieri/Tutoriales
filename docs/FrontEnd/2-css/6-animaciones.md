---
sidebar_position: 6
---

# 6 - Animaciones
---


## **Introducción a los Ejes y Gradientes**

### *Ejes*

**Eje principal (main axis):** Es la dirección principal en la que se distribuyen los elementos dentro de un contenedor flexible (como con flexbox o grid). En flexbox, depende del valor de la propiedad flex-direction.
Ejemplo: Si flex-direction es row, el eje principal será horizontal (de izquierda a derecha).

**Eje cruzado (cross axis):** Es el eje perpendicular al eje principal. Controla cómo se alinean los elementos en la dirección opuesta al eje principal.
Ejemplo: Si flex-direction es row, el eje cruzado será vertical.

### *Gradientes*

Un gradiente en CSS es una transición suave entre dos o más colores. Se utilizan como valores para propiedades como background-image y pueden ser de diferentes tipos:

**Linear gradient (gradiente lineal):** Los colores cambian en línea recta a lo largo de un eje

```jsx title="Ejemplo"
background: linear-gradient(to right, red, blue);
```

**Radial gradient (gradiente radial):** Los colores irradian desde un punto central hacia el exterior.

```jsx title="Ejemplo"
background: radial-gradient(circle, red, blue);

```

**Conic gradient (gradiente cónico):** Los colores giran alrededor de un punto, como un reloj.

```jsx title="Ejemplo"
background: conic-gradient(from 0deg, red, blue);
```

:::tip
Página para buscar color gradiente --> https://cssgradient.io/
:::


## **transiciones**

Las transiciones en CSS permiten animar cambios suaves entre dos estados de una propiedad CSS. Estas animaciones ocurren cuando se produce un cambio en el valor de una propiedad, por ejemplo, al pasar el mouse sobre un elemento o activando una clase con JavaScript.

La propiedad **transition** define qué propiedades se animarán, la duración, la curva de velocidad (timing function) y el tiempo de retraso.

```jsx title="Ejemplo"
transition: [propiedad] [duración] [función de tiempo] [retraso];
```

**- propiedad:** La propiedad CSS que se animará (como background-color, transform, etc.) o all para incluir todas las propiedades animables.

**- duración:** El tiempo que tomará la transición, como 1s (1 segundo) o 200ms (200 milisegundos).

**- función de tiempo:** La curva de velocidad, como ease, linear, ease-in, ease-out, etc.

**- retraso:** Tiempo antes de que comience la transición, como 0s o 300ms.

```jsx title="Ejemplo"
.button {
  background-color: blue;
  transition: background-color 0.5s ease;
}

.button:hover {
  background-color: red;
}

```

**Explicación:**

Cuando el usuario pasa el mouse sobre .button, el color de fondo cambiará de azul a rojo en 0.5 segundos con un efecto de velocidad suave (ease).


### *Propiedades relacionadas:*

**transition-property:** Especifica qué propiedad se anima

```jsx title="Ejemplo"
transition-property: background-color;
```


**transition-duration:** Define cuánto tiempo durará la transición
```jsx title="Ejemplo"
transition-duration: 1s;
```

**transition-timing-function:** Controla la curva de velocidad.
```jsx title="Ejemplo"
transition-timing-function: ease-in-out;

// Otros tipos:
transition-timing-function: linear; 
transition-timing-function: ease; 
transition-timing-function: ease-in;
transition-timing-function: ease-out;
transition-timing-function: ease-in-out;

```

**transition-delay:** Establece un retraso antes de que inicie la transición.
```jsx title="Ejemplo"
transition-delay: 0.2s;
```


## **Transformaciones**

Las transformaciones en CSS permiten cambiar la forma, posición, tamaño o rotación de un elemento en el espacio, sin afectar el flujo del documento. Se aplican mediante la propiedad **transform**, que puede combinar diferentes funciones de transformación.

### *Funciones Principales*

1. translate(x, y) (Trasladar):

Mueve un elemento en las direcciones horizontal (x) y vertical (y).

```jsx title="Ejemplo"
transform: translate(50px, 100px);
```
Mueve el elemento en diagonal, 50px a la derecha y 100px hacia abajo.


2. scale(x, y) (Escalar):

Cambia el tamaño del elemento.

```jsx title="Ejemplo"
transform: scale(1.5, 2);
```

-   Aumenta el ancho en un 150% y la altura en un 200%.
-   Si solo usas un valor (scale(2)), el cambio se aplica uniformemente en ambas direcciones.


3. rotate X:

Gira el elemento alrededor de su punto central.

```jsx title="Ejemplo"
transform: rotateX(180deg);
```

- Gira el elemento 180 grados pero a lo ALTO, tomando como base el Eje X

4. rotate Y:

Gira el elemento alrededor de su punto central.

```jsx title="Ejemplo"
transform: rotateY(180deg);
```

- Gira el elemento 180 grados pero hacia a los costados, tomando como base el Eje Y


5. rotate Z:

Gira el elemento alrededor de su punto central.

```jsx title="Ejemplo"
transform: rotateZ(180deg);
```

- Gira el elemento 180 grados simulando que rueda o gira como una pelota.


## **Animaciones**

Las animaciones en CSS permiten que los elementos cambien de estilo de forma gradual según fotogramas clave definidos. A diferencia de las transiciones, las animaciones no dependen de un evento como hover; se ejecutan automáticamente o mediante control específico.

### *Propiedades de animaciones*

1. **animation-name:** Define el nombre de la animación (debe coincidir con el nombre del bloque @keyframes).

```jsx title="Ejemplo"
animation-name: slideIn;
```

2. **animation-duration:** Especifica cuánto tiempo dura la animación.

```jsx title="Ejemplo"
animation-duration: 2s;
```

3. **animation-timing-function:** Controla la curva de velocidad de la animación.

-   Valores comunes: 
    -   ease
    -   linear
    -   ease-in
    -   ease-out
    -   ease-in-out
    -   funciones personalizadas con cubic-bezier.

```jsx title="Ejemplo"
animation-timing-function: ease-in-out;

```

4. **animation-delay:** Establece un retraso antes de que comience la animación.

```jsx title="Ejemplo"
animation-delay: 1s;
```

5. **animation-iteration-count:** Define cuántas veces se repite la animación.

Valores: un número (como 3) o infinite.

```jsx title="Ejemplo"
animation-iteration-count: infinite;

```

### *@keyframes*

Me permite determinar como quiero que inicie y finalice la animación.

1. **from y to:**

Es la forma más sencilla de definir una animación con dos estados: el inicial (from) y el final (to).

Se utiliza para transiciones simples.

```jsx title="Ejemplo"

@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
```

2. **Porcentajes (%):**

Permite especificar múltiples etapas intermedias en una animación.

Ideal para crear movimientos o efectos más complejos.

```jsx title="Ejemplo"

@keyframes colorShift {
  0% {
    background-color: red;
  }
  50% {
    background-color: yellow;
  }
  100% {
    background-color: green;
  }
}

```

3. **Fotogramas clave parciales:**

Puedes definir solo algunas etapas intermedias y CSS interpolará automáticamente los valores entre ellas.

```jsx title="Ejemplo"
@keyframes pulse {
  50% {
    transform: scale(1.5);
  }
}

```

En este caso, el estado inicial (0%) y final (100%) se toman como el estado por defecto del elemento.

----------

###  *Librerías de animaciones*

https://animate.style/

Instalación 

1. Ir a la documentación de la web, y buscar la sección de Instalación

![animate-1](/img/animate-1.png)


![animate-2](/img/animate-2.png)

```jsx title="Ejemplo"
<head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"
  />
</head>

```



2. Incorporamos la clase en la etiqueta del elemeto, seguido de la funionalidad.

```jsx title="Sintaxis"
<h1 class="animate__animated + FUNCIONALIDAD ">An animated element</h1>
```


Ejemplo:

- Seleccionamos la funcionalidad y copiamos

![animate-3](/img/animate-3.png)


- Agregamos en el HTML 

```jsx title="Sintaxis"
<h1 class="animate__animated animate__fadeIn ">An animated element</h1>
```


--------

Animaciones para scroll.

https://michalsnik.github.io/aos/

