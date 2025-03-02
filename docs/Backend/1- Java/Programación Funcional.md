---
sidebar_position: 4
---

# 4 - Programación Funcional

## Introducción a la Programación Funcional en Java
La programación funcional es un estilo de programación que se enfoca en el uso de **funciones** como bloques de construcción principales.

En lugar de **modificar el estado de los datos** de forma repetitiva, se trata de crear funciones puras que reciban datos y devuelvan nuevos resultados sin cambiar los datos originales.


## Clases Anónimas

Una clase es una estructura que sirve como plantilla para crear objetos. Define un conjunto de propiedades (atributos) y comportamientos (métodos) que los objetos que se basen en esa clase tendrán.
También podemos decir ques un tipo de dato complejo o compuesto.

Una **clase anónima** es una clase que se define y se usa al mismo tiempo sin darle un nombre.

Se utiliza cuando necesitamos una clase que solo se va a usar una vez, como para sobrescribir métodos de una clase o interfaz, sin tener que crear una clase completa.

:::tip
Una clase anónima es una instancia de una interfaz.
:::

Ejemplo 1 = Sobreescritura del método abstracto de la interfaz

```jsx title="Interfaz Animal"
public interface Animal{
    void hacerSonido(); //Método abstacto
}

```


```jsx title="Main"
public  class Main{
    public static void main(String[] args) {

        //Clase Anónima que implementa la interfaz Animal
        Animal perro = new Animal () {

            @Override
            public void hacerSonido(){
                System.out.println("Hola, estoy ladrando");
            }
        }; //Finaliza la clase anónima

        perro.hacerSonido(); //Se llama al método de la interfaz.
    }


}

```

<br/>

Ejemplo 2 = Método propio de la clase anónima.

```jsx title="Interfaz Vehiculo"
public interface Vehiculo{
   
}

```


```jsx title="Main"
public  class Main{
    public static void main(String[] args) {

        //Clase Anónima que implementa la interfaz Vehiculo
        new Vehiculo () {
            private int nPasajeros;
            
            //Métodos PROPIOS de la clase anónima
            public void manejar() {
                System.out.println("Estoy manejando");      
            }
        } .manejar(); // Llamado al método.
    }
}
```


## ¿Qué son las Funciones Lambda?
Las expresiones lambda son una forma de definir funciones anónimas (sin nombre) que pueden ser pasadas como argumentos a métodos o utilizadas para implementar interfaces funcionales (interfaces que contienen un solo método abstracto). Una expresión lambda se compone de:

-   Es una forma corta de escribir funciones que se puede pasar como parámetros a o usar dentro de colecciones.

-   Es una manera de representar una función o procedimiento sin tener que escribir toda la estructura de un método.


### Sintaxis

```jsx title="Sin parámetros"
() - > sentencia.
```

```jsx title="Con un parámetro"
 Parámetros - > sentencia
```

```jsx title="Con más de un parámetros"
(parámetro1, parámetro2) -> sentencia.
```

```jsx title="Con mas de una sentencia"
(parámetro) - > { sentencia1;
                sentencia2 ;}
```

### Interfaces Funcionales

Es aquella que tiene exactamente **UN SOLO MÉTODO ABSTRACTO**

-   Para poder hacer uso de expresiones lambda, debemos hacer uso también de interfaces funcionales. 

-   Las expresiones lambda se utilizan para implementar el único **método abstracto** de la interfaz funcional.



### Interfaces Funcionales PROPIAS

En Java es posible crear interfaces funcionales propias. Para poder realizarlo hay que asegurarse lo siguiente:

-   La interfaz contenga UN SOLO método abstracto.

-   La anotación @FunciontalInterfaces si bien es opcional, es una buena práctica utilizarla.

Ejemplo = Método sin parámetros.

```jsx title="Ejemplo"
@FunciontalInterfaces
public interface Mensajero {
    public void emitirMensaje();
}
```

```jsx title="Main"
public  class Main{
    public static void main(String[] args) {
        //Interfaz              Parámetros -> sentencia
        Mensajero msjeLambda = () -> {
                                        System.out.Println("Hola desde lambda");
                                    };
        msjeLambda.emitirMensaje();  
    }
}
```
<br/>


Ejemplo = Método con parámetros.

```jsx title="Ejemplo"
@FunciontalInterfaces
public interface Mensajero {
    public void emitirMensaje(String nombre);
}
```

```jsx title="Main"
public  class Main{
    public static void main(String[] args) {
        //Interfaz              Parámetros -> sentencia
        Mensajero msjeLambda = (nombre) -> {
                                        System.out.Println("Hola desde lambda" + nombre);
                                    };
        msjeLambda.emitirMensaje("Facundo");  
    }
}


Esta implementación es equivalente a :

Public void emitirMensaje (String nombre) {
     System.out.Println("Hola desde lambda" + nombre);
}
```
<br/>

Ejemplo = Método con 2 parámetros.

```jsx title="Ejemplo"
@FunciontalInterfaces
public interface Mensajero {
    public void emitirMensaje(String nombre, int edad);
}
```

```jsx title="Main"
public  class Main{
    public static void main(String[] args) {
        //Interfaz              Parámetros -> sentencia
        Mensajero msjeLambda = (nombre, edad) -> {
                                        System.out.Println("Hola desde lambda" + nombre);
                                        System.out.Println("Edad" + edad);
                                    };
        msjeLambda.emitirMensaje("Facundo,edad");  
    }
}

```








## ¿Qué son los Streams?
Los streams son una nueva abstracción introducida en Java 8 que permite procesar colecciones de datos de manera funcional. Proporcionan una forma de realizar operaciones sobre una secuencia de elementos, como filtrar, mapear, y reducir, de manera fluida y expresiva.

Características de los Streams:

**No almacenan datos:** Un stream es una secuencia de datos en la que se pueden aplicar operaciones, pero no almacena los datos en sí.

**Operaciones perezosas:** Las operaciones en los streams son perezosas, lo que significa que no se ejecutan hasta que se necesita el resultado final. Esto optimiza el rendimiento y la eficiencia del procesamiento.

**Soporte para procesamiento paralelo:** Los streams pueden ser paralelizados fácilmente, permitiendo el procesamiento de datos en múltiples hilos.


## Lambdas Vs Clases Anónimas.

1. Primero deberemos trabajar SIEMPRE con una interfaz funcional.

```jsx title="Interfaz Funcional"
@FunctionalInterface
public interface Operador {
    int operar (int num1, int num2);
}
```

<br/>

2. Implementeamos la interfaz con una función Anónima.

```jsx title="Ejemplo"
// Creamos una instancia de una clase anónima que implementa la interfaz Operador
Operador suma = new Operador () {

    //Sobreescribimos el método "Operar"
    @Override
    public int operar (int num1, int num2) {
        return num1 + num2;
    }
};

System.out.println("suma anónima: " + suma.operar(3,5));
// Imprime 8

```
<br/>

2. Implementeamos la interfaz con lambda

```jsx title="Ejemplo"
// Implementamos la interfaz utilizando una expresión lambda
Operador suma = (num1, num2) -> num1 + num2;

System.out.println("suma anónima: " + suma.operar(3,5));
// Imprime 8
```