---
sidebar_position: 1
---

# 1 - Introducción WEB

## ¿Cómo funciona la WEB?
El ámbito del desarrollo web es totalmente distinto al ámbito del desarrollo de aplicaciones de escritorio, no solo por utilizar tecnologías diferentes en cuanto a lenguajes de programación, sino también por el tipo de arquitectura sobre el cual los programas desarrollados van a tener que trabajar.

La internet o el mundo web en general está representado como un conjunto de computadoras interconectadas entre sí que se relacionan o pueden relacionarse mediante diferentes protocolos sin importar la distancia a la cual se encuentren unas de otras.

Cada una de estas computadoras pueden estar ofreciendo un servicio o información para ser consumido (un servidor) como así también pueden ser consumidores de dicho servicio o información (clientes), de esta manera, por ejemplo, si se tiene una página o aplicación web en un servidor, ésta se está ofreciendo como un servicio, donde cada una de las personas que accedan a la misma serán consideradas clientes. 

Esta forma de interconexión donde se ofrecen servicios que pueden ser consumidos se conoce como la arquitectura cliente servidor.


## Direccionamiento Web

Todo recurso en internet posee una dirección que lo representa y que permite que determinados clientes puedan acceder a él. Esta dirección es comúnmente conocida como URL (Uniform Resource Locator).

![url](/img/url.png)

Si bien la forma más conocida que tiene una URL suele ser como la de la imagen, **en realidad esto se trata de una “traducción” de una dirección IP que apunta al servidor del cual será consumida determinada información o acceso a un servicio.**

## Java - Spring boot - Maven

### JDK (Java Development Kit)

#### ¿Qué es?

-   El JDK es el kit de desarrollo de Java. Incluye:

        -   El JRE (Java Runtime Environment): para ejecutar aplicaciones Java.

        -   El compilador javac: para compilar archivos .java a bytecode .class.

        -   Herramientas como java, javac, javadoc, etc.


Una vez que el JDK compila el código a .class, viene el turno de la JVM (Java Virtual Machine).

La JVM:

-       Ejecuta los archivos .class (bytecode).

-       Provee el entorno de ejecución: memoria, hilos, etc.

-       Es parte del JRE (Java Runtime Environment).

⏩ Tu aplicación Spring Boot corre en la JVM como cualquier programa Java.




#### Responsabilidad

✅ Compilar y ejecutar aplicaciones Java.

 Ejemplo

```jsx title=""
javac MiClase.java     # compila usando JDK
java MiClase           # ejecuta usando el JRE (parte del JDK)


```


###  Spring Boot

#### ¿Qué es?

Spring Boot es un framework que simplifica el desarrollo de aplicaciones Java, especialmente APIs y aplicaciones web.

Parte de Spring Framework, pero:

-   Viene con muchas cosas configuradas por defecto (auto-configuración).

-   Incluye un servidor embebido como Tomcat o Jetty.

-   Te permite crear aplicaciones listas para ejecutar con java -jar.

####  Responsabilidad

✅ Facilita el desarrollo rápido de aplicaciones Java modernas, eliminando configuración manual.

➤ ¿Corre en la JVM?
Sí. Las apps Spring Boot compilan a bytecode Java y se ejecutan sobre la JVM, igual que cualquier programa Java.



### Maven


#### ¿Qué es?

Maven es una herramienta de construcción (build tool). Se encarga de:

-   Descargar dependencias (como Spring Boot).

-   Compilar código.

-   Correr tests.

-   Empaquetar en .jar o .war.

-   Ejecutar tu aplicación.

-   Se basa en un archivo central: pom.xml, donde declarás las dependencias y configuración del proyecto.

####  Responsabilidad

✅ Automatiza todo el ciclo de vida del desarrollo de una app Java.



### ¿Cómo trabajan juntos?

Imaginá que hacés una API en Spring Boot. Esto pasa:

Vos escribís código Java.

El JDK (más precisamente javac) lo compila.

Spring Boot organiza tu aplicación y proporciona la infraestructura web.

Maven:

Descarga Spring Boot y otras librerías.

Compila tu código con javac.

Empaqueta todo en un .jar.

Te permite ejecutar con mvn spring-boot:run.


![java-spring-maven](/img/java-spring-maven.png)