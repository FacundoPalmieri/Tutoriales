---
sidebar_position: 9
---

# 9 - Preguntas Técnicas.

## **Java**

## ☕ Bloque 1 – Fundamentos de Java

---

### ¿Qué es Java?
Java es un lenguaje de programación orientado a objetos, robusto y multiplataforma.  
Su lema “**Write once, run anywhere**” significa que el código puede ejecutarse en cualquier sistema que tenga una **JVM**.

---

### ¿Qué son el JDK, JRE y JVM?
- **JVM (Java Virtual Machine):** ejecuta el código Java compilado (los `.class`).  
- **JRE (Java Runtime Environment):** incluye la JVM y las librerías necesarias para ejecutar programas.  
- **JDK (Java Development Kit):** incluye el JRE y herramientas para desarrollar (como el compilador `javac`).

---

### ¿Por qué Java es multiplataforma?
Porque el código fuente se compila a **bytecode**, que no depende del sistema operativo.  
La **JVM** interpreta ese bytecode en cualquier plataforma.

---

### ¿Qué es la Programación Orientada a Objetos (POO)?
Es un paradigma que organiza el código en **clases** (plantillas) y **objetos** (instancias).  
Sus pilares son: **encapsulamiento, herencia, polimorfismo y abstracción**.

Encapsulamiento: Es ocultar los detalles internos de una clase y exponer solo lo necesario mediante getters y setters..

Herencia: Permite que una clase herede atributos y métodos de otra para reutilizar código.

Polimorfismo:Es la capacidad de un mismo método comportarse distinto según el objeto que lo use (por ejemplo, sobrescribir métodos).

Abstracción: Es enfocarse en lo esencial, ocultando la complejidad interna.
Por ejemplo, trabajar con una interfaz sin importar la implementación concreta.

---

### Diferencia entre Clase Abstracta e Interfaz
- **Clase abstracta:** puede tener métodos con o sin implementación.  No puede instanciarse directamente. Solo puede ser heredada
- **Interfaz:** solo define métodos que las clases deben implementar (a partir de Java 8 puede tener métodos default).

📌 Usar interfaz favorece la flexibilidad; usar clase abstracta permite compartir comportamiento.

---

### ¿Qué significa que Java sea fuertemente tipado?
Cada variable tiene un tipo definido (int, String, etc.) y no puede cambiar.  
Esto ayuda a detectar errores en tiempo de compilación.

---

### ¿Qué es la inmutabilidad?
Un objeto es **inmutable** si no puede cambiar su valor una vez creado.  
Ejemplo clásico: la clase `String`.

---

### 🧹 8. ¿Qué es el Garbage Collector (GC)?

Es un proceso automático que libera memoria (Heap) eliminando objetos que ya no se usan.
Por eso en Java no es necesario liberar memoria manualmente.

--- 

### 🔍 9. Diferencia entre == y equals()

== :compara referencias en memoria (si apuntan al mismo objeto) o valores en tipos de datos primitivos.

equals() : compara contenido lógico (si los valores son iguales).


```jsx title=""
String a = new String("hola");
String b = new String("hola");
System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true

```
---

### 📦 10. ¿Qué hace la palabra clave final?

En variables → no se puede reasignar.

En métodos → no se puede sobrescribir.

En clases → no se puede heredar.

---

### 🧩 11. ¿Qué diferencia hay entre final, finally y finalize()?

final: palabra clave para bloquear cambios.

finally: bloque de código que siempre se ejecuta (incluso si hay error).

finalize(): método llamado antes de que un objeto sea eliminado por el GC (obsoleto desde Java 9).

---

### 🧮 12. ¿Qué diferencia hay entre el stack y el heap?

Stack: Memoria que guarda variables locales y referencias (variable de los métodos. Al finaliza el método se libera memoria.).

Heap: Memoria que guarda los objetos creados con new (Gestionado por GC).


---

### 🔄 13. ¿Qué es la sobrecarga (overloading)?

Es cuando dos o más métodos tienen el mismo nombre, pero diferente firma (es decir, distinto número o tipo de parámetros). El compilador decide cuál usar según los argumentos que le pasemos.

```jsx title=""
void saludar() {}
void saludar(String nombre) {}

```

### 🧬 14. ¿Qué es la sobrescritura (overriding)?

Cuando una clase hija redefine un método de la clase padre para cambiar su comportamiento.

```jsx title=""
class Animal { void sonido() { System.out.println("Sonido genérico"); } }
class Perro extends Animal { void sonido() { System.out.println("Guau"); } }

```

---

### 🔒 15. Modificadores de acceso

public: visible desde cualquier lugar.

protected: visible dentro del mismo paquete o subclases.

default (sin palabra): visible solo en el mismo paquete.

private: visible solo dentro de la clase.

---

### 🧰 16. ¿Qué hace la palabra clave static?

Permite acceder a variables o métodos sin crear un objeto de la clase.

---

### 🧩 17. ¿Qué es una variable local y una variable de instancia?

Local: se declara dentro de un método, solo existe mientras se ejecuta.

De instancia: pertenece al objeto, vive mientras el objeto exista.

---


### 🧾 18. ¿Qué hace System.out.println()?

System es una clase del core de Java,
out es un flujo de salida estándar,
println() imprime texto seguido de salto de línea.


---

### ⚡ 19. ¿Qué es una expresión lambda?

Una forma corta de definir funciones anónimas.

Una función anónima es una función sin nombre, que se usa para hacer algo rápido sin necesidad de declararla aparte.

---

### 🧩 20. ¿Qué es un Optional?

Una clase que evita el uso de null y los NullPointerException.

---

### 🧭 21. ¿Qué diferencia hay entre programación imperativa y funcional?

Imperativa: indica cómo hacer algo (paso a paso).

Funcional: La programación funcional se centra más en describir qué se quiere lograr, en lugar de detallar cómo hacerlo paso a paso


---


### 22. ¿Qué son las colecciones en Java?

Son estructuras para almacenar y manipular grupos de objetos:
List, Set, Map, Queue, etc.

---

### 23. ¿Diferencia entre List, Set y Map?

List: elementos ordenados y repetidos.

Set: elementos únicos, sin orden.

Map: pares clave-valor.

---

### 24. ¿Qué es un Stream en Java?

Una forma funcional de procesar colecciones (filtrar, mapear, reducir) sin bucles tradicionales.

---

### 25. ¿Qué son las excepciones?

Errores que ocurren en tiempo de ejecución.
Se manejan con try-catch o se propagan con throws.

---

### 26. ¿Qué diferencia hay entre Checked y Unchecked exceptions?

Checked: se deben capturar o declarar (Ej: IOException).

Unchecked: no se obliga (Ej: NullPointerException).

---



### 27. ¿Qué es un Thread?

Un hilo de ejecución. Permite ejecutar tareas en paralelo dentro del mismo proceso.

---

### 28. ¿Qué es la sincronización?

Mecanismo para controlar el acceso concurrente a recursos compartidos en multithreading.

---


### 29. ¿Qué es un record (Java 14+)?

Una forma rápida de crear clases inmutables para transportar datos.


---


### 30. ¿Qué es el principio SOLID en Java?

Son 5 principios de diseño que mejoran la mantenibilidad del código:

S – Responsabilidad Única (Single Responsibility): Una clase debe tener una sola responsabilidad.

O – Abierto/Cerrado (Open/Closed): El código debe estar abierto a extensión, cerrado a modificación.

L – Sustitución de Liskov (Liskov Substitution): Las subclases deben poder reemplazar a sus superclases sin romper el código.

I – Segregación de Interfaces (Interface Segregation): Las interfaces deben ser pequeñas y específicas.

D – Inversión de Dependencias (Dependency Inversion): Las clases deben depender de abstracciones, no de implementaciones concretas.


---


### 31. ¿Qué es un patrón de diseño?

Solución reusable a un problema común.
Ejemplos:

Singleton: una sola instancia global.

Factory: crea objetos sin exponer la lógica.

Repository: capa intermedia para acceder a datos.

Controller: recibe peticiones y coordina la lógica.

---



### 32. ¿Qué son las anotaciones en Java?

Metadatos que agregan información al código.
Ejemplo: @Override, @Autowired, @Entity.

---


### 33. ¿Qué hace la anotación @Override?

Indica que un método está sobrescribiendo uno de su clase padre o interfaz.

---




### 34. ¿Qué hace el método hashCode()?

Devuelve un número entero usado por estructuras como HashMap para ubicar objetos rápidamente.

---


### 35. ¿Qué es un DTO?

Un Data Transfer Object. Se usa para transferir datos entre capas (por ejemplo, del backend al frontend).

---


### 36. ¿Qué diferencia hay entre POJO y Bean?

POJO: clase simple sin reglas especiales.

Bean: POJO gestionado por el contenedor de Spring.

------------------------------------------------------------------------------------------------------------------------------------

## 🧩 BLOQUE 3 – SPRING BOOT Y ECOSISTEMA

### 1. ¿Qué es Spring Boot?

Es una extensión de Spring Framework que simplifica la configuración y ejecución de aplicaciones Java.
Permite crear apps listas para usar, con servidor embebido (Tomcat o Jetty) y configuración automática.

---

### ¿Qué es Maven?

Maven es una herramienta de gestión de proyectos y dependencias en Java que ayuda a compilar, construir y empaquetar aplicaciones.

Funciones principales:

- Gestionar dependencias: descarga automáticamente las librerías que tu proyecto necesita.

- Compilar y construir: automatiza la compilación del código y la generación de archivos .jar o .war.

- Estandarizar proyectos: mantiene la misma estructura y configuración en todos los proyectos Java.

- Integración con plugins: permite ejecutar tests, generar documentación, desplegar aplicaciones, etc.

Usos típicos en Spring Boot:

- Agregar dependencias como spring-boot-starter-web o spring-boot-starter-data-jpa sin descargarlas manualmente.

- Construir el proyecto y generar el .jar ejecutable con un solo comando.

- Facilitar la integración con herramientas de CI/CD.

---

### 2. ¿Qué es la inyección de dependencias?

Es cuando Spring le da a una clase los objetos que necesita automáticamente, sin que yo los tenga que crear. Se puede usar @Autowired o inyección por constructor para indicar qué dependencias deben inyectarse. Para que Spring lo haga, los objetos deben ser beans (@Component, @Service, @Repository, etc.).

---


### 2. ¿Qué es la inversión  de control?
Es un principio donde Spring se encarga de crear y manejar los objetos (beans) y su ciclo de vida, en lugar de que yo los cree manualmente. Para que Spring pueda controlarlos, las clases se registran como beans usando anotaciones como @Component, @Service o @Repository.

Clave: IoC significa que yo no controlo la creación ni gestión de los objetos, Spring lo hace por mí.

Ejemplo típico: cualquier bean inyectado en otra clase (el control de su creación lo hace Spring, no yo).

---



### 3. ¿Qué es el principio de inversión de dependencias?

Las clases deben depender de abstracciones (interfaces), no de implementaciones concretas.

---



### 4. ¿Qué diferencia hay entre @Component, @Service, @Repository y @Controller?

Todas son detectadas por Spring, pero cada una tiene un propósito:

@Component: clase genérica gestionada por Spring.

@Service: capa de lógica de negocio.

@Repository: capa de acceso a datos (DAO).

@Controller / @RestController: manejan peticiones HTTP.

---


### 5. ¿Qué es un Bean en Spring?

Es un objeto gestionado por el contenedor de Spring.
Spring controla su ciclo de vida (creación, inyección, destrucción).

---


### 6. ¿Qué diferencia hay entre @Bean y @Component?

@Bean: se usa dentro de una clase de configuración (@Configuration) para registrar un objeto manualmente.

@Component: se usa directamente en la clase para que Spring la detecte automáticamente.

---


### 7. ¿Qué es una capa Service?

Es donde va la lógica de negocio.
Recibe datos del controlador, los procesa y llama a los repositorios si es necesario.

---


### 8. ¿Qué es la capa Repository?

Es la encargada del acceso a datos.
Generalmente extiende de JpaRepository o CrudRepository.

---



### 9. ¿Qué es el application.properties o application.yml?

Archivo de configuración donde se definen propiedades del proyecto:
puerto, base de datos, logs, JWT, etc.

---


### 10. ¿Qué es el archivo pom.xml?

Archivo de Maven donde se definen las dependencias, plugins y versión de Java usada.


---

### 11. ¿Qué diferencia hay entre @Controller y @RestController?

@Controller: devuelve vistas (HTML, JSP, etc).

@RestController: devuelve directamente JSON o XML.


---

### 12. ¿Qué es un ResponseEntity?

Objeto que permite controlar la respuesta HTTP: código de estado, headers y cuerpo.


---

### 13. ¿Qué es el manejo de excepciones en Spring Boot?

Se pueden manejar globalmente usando @ControllerAdvice y @ExceptionHandler.

---------------------------------------------------------------------------------------------------------------

## Spring Data

📘 Es un módulo de Spring que simplifica el acceso a datos.

Te permite trabajar con bases de datos sin escribir tanto código SQL o JDBC.

Ofrece repositorios genéricos (JpaRepository, CrudRepository) con métodos listos como save(), findById(), delete(), etc.

Su objetivo es: reducir el código repetitivo al trabajar con datos.

👉 En resumen:

"Spring Data es una capa que simplifica el acceso a datos usando repositorios automáticos."


---

### Spring Data JPA

📘 Es una implementación concreta de Spring Data, enfocada en JPA (Java Persistence API).

Te permite mapear entidades Java con tablas de base de datos.

Usa interfaces como JpaRepository para interactuar con la BD sin escribir queries SQL manuales.

Puedes definir métodos como findByEmail(String email) y Spring genera la query automáticamente.

👉 En resumen:

"Spring Data JPA es la parte de Spring Data que usa JPA para mapear objetos Java con tablas."


---

### JPA (Java Persistence API)

📘 Es una especificación, no una herramienta.

Define cómo deben guardarse objetos Java en una base de datos relacional.

Marca las reglas y anotaciones (@Entity, @Table, @Id, @OneToMany, etc.), pero no implementa nada.

Necesita un proveedor de persistencia (como Hibernate) para funcionar.

👉 En resumen:

"JPA define el estándar para mapear objetos Java con tablas, pero necesita un motor como Hibernate que lo implemente."

---
### Hibernate

📘 Es una implementación concreta de JPA (el motor real que hace el trabajo).

Convierte objetos Java en registros de base de datos y viceversa.

Genera automáticamente las consultas SQL.

Administra el estado de las entidades, las relaciones (@OneToMany, etc.), y la caché.

👉 En resumen:

"Hibernate es la herramienta que implementa JPA y realiza realmente la conexión entre Java y la base de datos."


---

Spring Data (más general)

   ↓

Spring Data JPA (usa JPA)

   ↓

JPA (define las reglas)

   ↓

Hibernate (las implementa)

   ↓
   
Base de datos

---

### ¿Qué hace Spring Data JPA?

Permite interactuar con bases de datos usando repositorios en lugar de SQL.
Spring genera las consultas a partir de los nombres de los métodos.


---

###  ¿Qué es el fetch en JPA?

Indica cómo se cargan las relaciones:

EAGER: carga todo junto.

LAZY: carga bajo demanda (recomendado para rendimiento).

---



###  ¿Qué diferencia hay entre save() y saveAndFlush()?

save(): guarda el objeto en memoria y lo persiste cuando se confirma la transacción.

saveAndFlush(): guarda y fuerza la escritura inmediata en la base de datos.

---


### ¿Qué es una transacción (@Transactional)?

Es un bloque de operaciones que deben ejecutarse todas juntas.
Si algo falla, se hace rollback y no se aplican los cambios.

---


### ¿Qué hace la anotación @Transactional(readOnly = true)?

Optimiza consultas que solo leen datos, evitando operaciones de escritura innecesarias.

---

---------------------------------------------------------------------------------------------------------------


## Spring Security

### 1. ¿Qué es Spring Security?

Es el módulo que maneja autenticación (login) y autorización (permisos).
Se integra fácilmente con JWT y roles de usuario.

---

### 2. ¿Qué es un JWT (JSON Web Token)?

Es un token firmado que identifica al usuario.
Se usa para autenticación sin mantener sesión en el servidor.

---

### 3. ¿Qué es la autenticación y autorización?

Autenticación: comprobar quién es el usuario.

Autorización: comprobar qué puede hacer.

---

### 4. ¿Qué es CORS?

Cross-Origin Resource Sharing.
Controla qué dominios pueden hacer peticiones a tu API.

---

### 5. ¿Qué es un filtro (Filter) en Spring Security?

Es un componente que intercepta las peticiones HTTP antes de llegar al controlador, útil para validar tokens.

---

### 6. Flujo

- El usuario hace login
  - Envía sus credenciales (por ejemplo, usuario y contraseña) a un endpoint /login o /authenticate.

- El filtro de seguridad intercepta la petición

  - Un filtro (por ejemplo, UsernamePasswordAuthenticationFilter o uno personalizado para JWT) detecta la petición y crea un objeto Authentication con los datos del usuario (por ejemplo, usuario y contraseña sin validar).

  - Este objeto se envía al AuthenticationManager.

- El AuthenticationManager delega al AuthenticationProvider

  - El AuthenticationManager no valida directamente; deriva la validación a un AuthenticationProvider.

  - El AuthenticationProvider usa el UserDetailsService para buscar el usuario en la base de datos y comparar la contraseña con un  PasswordEncoder.

- Si las credenciales son válidas:

  - El AuthenticationProvider devuelve un objeto Authentication completo y autenticado, con los roles o authorities del usuario.

  - Spring guarda ese objeto en el SecurityContext → el usuario queda autenticado.

- El usuario hace otra petición (ya autenticado)

  - Ahora Spring revisa si el token/sesión es válido y si tiene permiso para acceder al recurso.

  - Si el endpoint tiene anotaciones como @PreAuthorize("hasRole('ADMIN')"), Spring evalúa los roles del usuario antes de ejecutar el método.



---------------------------------------------------------------------------------------------------------------
## Test Unitarios

### 1. ¿Qué es un test unitario?

Verifica el funcionamiento de una parte pequeña del código (por ejemplo, un método).
En Spring se usa JUnit + Mockito.

---

### 2. ¿Qué es un test de integración?

Prueba el funcionamiento completo entre varias capas (controller + service + repository).

---

### 3. ¿Qué hace la anotación @SpringBootTest?

Levanta el contexto completo de Spring para pruebas de integración.

---

### 4. ¿Qué hace Mockito?

Permite simular objetos y comportamientos (mocks) para probar unidades de código sin depender de componentes reales.


---------------------------------------------------------------------------------------------------------------

## Microservicios

### 1. ¿Qué es un microservicio?

Es una aplicación pequeña e independiente que cumple una función específica.
Cada microservicio tiene su propia base de datos y se comunica con otros por HTTP o mensajería.

---

### 2. ¿Qué ventajas tiene una arquitectura de microservicios?

Escalabilidad individual.

Despliegue independiente.

Aislamiento de fallos.

Flexibilidad tecnológica.

---

### 3. ¿Qué es un API Gateway?

Punto de entrada único que recibe todas las peticiones y las redirige al microservicio correspondiente.

---

### 4. ¿Qué es el Service Discovery?

Permite que los microservicios se descubran entre sí dinámicamente (por ejemplo, con Eureka).

---

### 5. ¿Qué es la comunicación síncrona y asíncrona entre microservicios?

Síncrona: espera respuesta inmediata (HTTP).

Asíncrona: usa colas o mensajería (RabbitMQ, Kafka).


---------------------------------------------------------------------------------------------------------------

## Despligeuge

### 1. ¿Qué es un Dockerfile?

Archivo con instrucciones para construir una imagen Docker de la aplicación (instalar dependencias, copiar código, exponer puerto, etc.).

---


### 2. ¿Qué es una imagen Docker?

Es una plantilla inmutable que contiene todo lo necesario para ejecutar tu app (Java, dependencias, etc.).

---

### 3. ¿Qué es un contenedor Docker?

Es una instancia en ejecución de una imagen.
Permite ejecutar tu app en cualquier entorno sin diferencias.

---


### 4. ¿Qué es un pipeline de CI/CD?

Proceso automatizado para compilar, testear y desplegar tu aplicación.
Ejemplo: GitHub Actions, Jenkins, GitLab CI.