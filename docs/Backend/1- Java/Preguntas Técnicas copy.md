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

Son estructuras para almacenar grupos de objetos:
List, Set, Map, Queue, etc.

---

### 23. ¿Diferencia entre List, Set y Map?

**List:**  Mantiene orden de inserción y Permite duplicados

- ArrayList: Es una lista que se almacen como un array. Más rápido para leer (acceso por índice) porque estan todos los elementos contiguos. Pero Eliminar en el medio puede ser mas costoso o bien si agregar si no queda espacio libre, tiene que mover todo el array

- LinkedList(Lista enlazada): Almacena en cualquier posición de memoria, pero mantiene una referencia de quien es el que le sigue Más rápido para insertar/eliminar en el medio. Leer es más lento porque no estan de manera contigua. 

**Set:** elementos únicos, El orden depende del tipo de Set.

- HashSet: Sin orden. El más rápido para inserción y búsqueda, porque no revisar todos, sino que por el hascode va a buscar donde deberia estar.

- LinkedHashSet: Orden de inserción. Como HashSet rápido para inserción y búsqueda, pero mantiene el orden, ya que internamente tiene una lista doblemente enlazada que le permite saber que elementos es el anterior y cual el siguiente.

- TreeSet: Orden natural (números de menor a mayor, strings alfabéticamente- O un orden personalizado que vos definís con un Comparator).Mas lento Porque internamente usa un árbol rojo-negro (una estructura de árbol balanceado). Cada vez que agregás un elemento, el árbol se reorganiza para mantener el orden siempre correcto.


**Map:** pares clave-valor . No permite clave duplicadas.

- HashMap: Sin orden. El más rápido para inserción y búsqueda, porque no revisar todos, sino que por el hascode va a buscar donde deberia estar.

- LinkedHashMap: Orden de inserción. Como HashSet rápido para inserción y búsqueda, pero mantiene el orden, ya que internamente tiene una lista doblemente enlazada que le permite saber que elementos es el anterior y cual el siguiente.

- TreeMap: Orden natural (números de menor a mayor, strings alfabéticamente- O un orden personalizado que vos definís con un Comparator).Mas lento Porque internamente usa un árbol rojo-negro (una estructura de árbol balanceado). Cada vez que agregás un elemento, el árbol se reorganiza para mantener el orden siempre correcto.

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

Un patrón de diseño es una solución probada y reutilizable para un problema común en el diseño de software.
Ejemplos:

Singleton: una sola instancia global. Ej un BEAN

Factory: Patrón que crea objetos sin exponer la lógica de creación.

Repository: Capa intermedia para acceder a datos.

MVC: arquitectura que separa vista, modelo y controlador.   
- Modelo (Model): representa los datos y la lógica de negocio (entidades, reglas, servicios).
- Vista (View): lo que ve el usuario; muestra la información (HTML, Angular, Flutter, etc).
- Controlador (Controller): recibe la petición, llama al modelo/servicios y devuelve una respuesta a la vista.
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

## 🧩 BLOQUE 2 – SPRING BOOT Y ECOSISTEMA

### 1. ¿Qué es Spring Boot?

Spring Boot es una extensión de Spring Framework que permite crear aplicaciones Java listas para usar, con configuración automática, dependencias preconfiguradas y un servidor embebido (como Tomcat).
Su objetivo es simplificar el desarrollo eliminando la configuración manual.

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

### 2. ¿Qué es la inversión  de control?
Es un principio donde Spring crea, gestiona e inyecta los objetos (beans) en lugar de que el desarrollador los construya manualmente con new. Para que Spring pueda controlarlos, las clases se registran como beans usando anotaciones como @Component, @Service o @Repository.

Clave: IoC significa que yo no controlo la creación ni gestión de los objetos, Spring lo hace por mí.

Ejemplo típico: cualquier bean inyectado en otra clase (el control de su creación lo hace Spring, no yo).


---

### 2. ¿Qué es la inyección de dependencias?
Es un mecanismo donde Spring entrega automáticamente a una clase los objetos que necesita.
 
Se puede usar @Autowired o inyección por constructor para indicar qué dependencias deben inyectarse. Para que Spring lo haga, los objetos deben ser beans (@Component, @Service, @Repository, etc.).

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

------------------------------------------------------------------------------------------------------------------------------------

## BLOQUE 3 Spring Data

📘 Es una familia de módulos de acceso a datos.

Incluye:

Spring Data JPA

Spring Data MongoDB

Spring Data Redis

Spring Data JDBC

👉 En resumen:

"Spring Data es la familia de módulos que simplifica el acceso a datos"


---

### Spring Data JPA

📘 Es una implementación concreta de Spring Data, enfocada en JPA (Java Persistence API).

Te permite mapear entidades Java con tablas de base de datos.

Usa interfaces como JpaRepository  lo que me da CRUD, paginación, sort y manejo automático de transacciones.

Esto genera automáticamente:

save(product)

findById(id)

findAll()

deleteById(id)

deleteAll()

existsById(id)


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



## BLOQUE 4 - Spring Security

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

Cuando un usuario hace login, la request pasa por el filtro de Spring Security, que extrae el usuario y la contraseña y los envía al AuthenticationManager.

El AuthenticationManager delega en un AuthenticationProvider (cuando autenticamos contra base de datos). Este provider usa el UserDetailsService para cargar al usuario y un PasswordEncoder para validar la contraseña.

Si todo es correcto, Spring crea un objeto Authentication autenticado y lo coloca en el SecurityContext.

**CON SESSION**

Spring crea una sesión (HttpSession) Y El servidor genera una cookie JSESSIONID y la envía al navegador.

En cada request posterior al login:

- El navegador envía automáticamente la cookie JSESSIONID.

- El filtro SecurityContextPersistenceFilter mira esa cookie.

- Busca la sesión en el servidor.

- Reconstruye el SecurityContext con el Authentication de ese usuario.

- Los filtros de autorización (@PreAuthorize, reglas de httpSecurity`) utilizan las authorities del Authentication recuperado.

**CON JWT**

Con esa información genero el JWT: incluyo el username, las authorities y, si corresponde, un identificador para el refresh token; luego se define la expiración, se firma el token y se devuelve al cliente.

En las siguientes requests ya no se usan las credenciales, sino el JWT. Un filtro JWT intercepta cada request, valida el token y, si es válido, reconstruye un Authentication autenticado dentro del SecurityContext.

A partir de ahí, la autorización depende de anotaciones como @PreAuthorize, que consultan las authorities del token antes de permitir el acceso.

Si el JWT está expirado, normalmente se devuelve un 401. El cliente intercepta ese estado y realiza la solicitud al endpoint de refresh. Allí se valida el refresh token y, si es correcto, se genera un nuevo JWT y se devuelve.


**MIO**

En mi API no uso el login por defecto de Spring Security.
Tengo mi propio endpoint de login.
Cuando llega el usuario y contraseña, se llama al método authenticate,
Se busca el usuario mediante el userDetailService,
se valida la contraseña con el PasswordEncoder y verifica el estado de la cuenta.
Si todo es correcto, Spring genera un Authentication y yo lo guardo en el SecurityContextHolder.
Finalmente genero un JWT y lo devuelvo al cliente

---------------------------------------------------------------------------------------------------------------
##  BLOQUE 5- Test Unitarios

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

## BLOQUE 6 -  Microservicios

### 1. ¿Qué es un microservicio?

Un microservicio es una aplicación pequeña, independiente, enfocada en una única responsabilidad del negocio.
Se despliega por separado, tiene su propio ciclo de vida y normalmente su propia base de datos.
Se comunica con otros microservicios mediante HTTP o mensajería.

---

### 2. ¿Qué ventajas tiene una arquitectura de microservicios?

✔ Escalabilidad independiente
Cada microservicio escala solo donde hay demanda
(ej: pagos escala más que notificaciones).

✔ Despliegue independiente
Puedo actualizar un servicio sin tirar toda la aplicación.

✔ Aislamiento de fallos
Si falla "Notificaciones", no debería caer "Procesamiento de Pagos".

✔ Flexibilidad tecnológica
Puedo tener un microservicio en Java y otro en Go (Mercado Pago usa Go también).

✔ Organización por dominios de negocio (DDD)
Cada equipo es dueño de su microservicio → mayor autonomía.

✔ Ciclo de vida separado
Cada microservicio tiene su propio versionado, CI/CD, monitoreo, etc



¿Y las desventajas?” (TE LA PUEDEN PREGUNTAR)

Tenela lista:

❌ Mayor complejidad operativa

❌ Requiere monitoreo, logging distribuido, traicing (ejemplo: OpenTelemetry)

❌ Comunicación entre microservicios genera latencia

❌ Transacciones distribuidas (sagas) → más complejas

❌ Testing más difícil (contratos, integración)

---

### 3. ¿Qué es un API Gateway?

Es el punto de entrada único de la arquitectura.
Todas las peticiones externas pasan por ahí antes de llegar a los microservicios.

Funciones típicas:

Ej: Enrutamiento → decide a qué servicio mandar la request

---

### 4. ¿Qué es el Service Discovery?

Service Discovery es un mecanismo que permite a los microservicios encontrarse entre sí sin usar direcciones fijas. Cada servicio se registra y el que necesita comunicarse consulta quién está disponible en ese momento.

Ejemplo simple:

“Si tengo 3 instancias del servicio de pagos, un microservicio que quiere llamar a pagos no necesita saber sus IPs, solo pregunta al Service Discovery cuál instancia está disponible y se conecta a esa.”


### Problema: un microservicio está caído

Si A llama a B y B está caído:

La llamada falla → A recibe error o timeout.

Formas de manejarlo (patrones de resiliencia):

-  Retries: volver a intentar la llamada algunas veces.

-  Circuit Breaker: detectar que B está fallando y no seguir llamando hasta que se recupere.

-  Fallback: devolver un valor por defecto o usar caché.

Ejemplo práctico:

Servicio Pedidos llama a Pagos. Si Pagos está caído, Pedidos puede marcar el pedido como “pendiente de pago” (fallback) y reintentar más tarde, evitando bloquear todo el sistema.

---

### 5. ¿Qué es la comunicación síncrona y asíncrona entre microservicios?

a) Comunicación Síncrona (HTTP/REST)

El servicio A llama a B y espera la respuesta.

Si B está caído o lento, A queda bloqueado.

Es más simple, pero genera acoplamiento temporal.

Ideal cuando necesitás respuesta inmediata.

👉 Ejemplo:
El servicio de Pagos consulta al servicio de Cuentas si hay saldo disponible antes de aprobar una transacción.

b) Comunicación Asíncrona (mensajería: Kafka, RabbitMQ)

A envía un mensaje al broker y sigue su ejecución.

B procesará el mensaje cuando pueda.

Esto mejora la escalabilidad, la resiliencia y desacopla a los servicios.

No hay bloqueo: si B cae, A sigue funcionando.

👉 Ejemplo:
Después de aprobar una compra, el servicio de Pagos publica un evento “PagoAprobado”, y el servicio de Notificaciones lo procesa más tarde.


---------------------------------------------------------------------------------------------------------------

## BLOQUE 7 - Despligeuge

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



-------------------------------------------------------------------------------

## BLOQUE 8 - Compilación proyecto


### Al darle Play al IDE :

### 1. Maven compila el proyecto

**Maven usa el pom.xml para:**

- Ver qué dependencias necesita.

- Buscar cada una en el repositorio local (~/.m2/repository).

- Si falta alguna → la descarga desde Maven Central o el repo que tengas configurado (Todas las dependencias se descargan ya en .jar - versión final para compilación)


**Compila el código fuente**

- Compila tu código fuente (.java que están en src/main/java ) y genera los .class en target/classes.

- Esos .class son el bytecode: el lenguaje intermedio que entiende la JVM.

Ese classpath contiene:

- Tus clases compiladas → target/classes

- Los jars de las dependencias → los que Maven resolvió en ~/.m2/repository

- Cualquier jar manual (como sapjco3.jar) que hayas agregado en /lib


**Copia los recursos**

Todo lo que está en src/main/resources (por ejemplo, application.yml, banner.txt, etc.) también se mete en target/classes.





📦 En resumen, el classpath es la “bolsa de clases” que la JVM va a usar.

```jsx title=""
target/classes/com/facu/app/
    Main.class
    Usuario.class
    UsuarioService.class


```

### 2. La JVM arranca y carga el main()

Acá empieza la magia real:

- La JVM ejecuta tu clase con main() (por ejemplo com.facundo.MiAppSpringApplication).

- A medida que el programa pide otras clases (por ejemplo UsuarioService, ClienteRepository, SpringApplication),
la JVM las busca dentro del classpath.

- Si las encuentra → las carga en memoria.
- Si no → lanza ClassNotFoundException.

🧠 Así, la JVM “va pidiendo” clases a medida que el código lo necesita, no todas al inicio.


### Si hay una librería especial (como SAP JCo3)

Ahí entra el detalle que mencionabas:

“La máquina virtual debe saberlo e ir a buscarla donde le indique en la configuración de la misma.”


La parte Java (sapjco3.jar) se encuentra por el classpath.

La parte nativa (sapjco3.dll o .so) no está en el classpath,
sino que la JVM la busca en el java.library.path (otro conjunto de rutas).

Por eso, cuando arrancás la app, la JVM tiene dos “mapas”:

- Java bytecode (.class, .jar)
  - Los ubica en el classpath : Contiene la app y librerías Java puras

- Código nativo (.dll / .so)
  - java.library.path : Contiene librerías del sistema como SAP JCo.


En resumen:

✅ Al darle Play en el IDE :

- Maven compila el código fuente con las dependencias del .m2 (descarga si falta alguna).

- Genera bytecode en target/classes.

- IntelliJ arma el classpath con tus clases y todas las dependencias (.jar).

- La JVM recibe ese classpath, ejecuta el main(), y carga las clases que va necesitando.

- Si hay librerías nativas (como JCo3), las busca en el java.library.path.



-------------------------------------------------------------------------------

## BLOQUE 9 - Caché con Redis

Redis es una base de datos en memoria que se usa como cache para evitar ir a la base física. Esto reduce la latencia porque evita ir a la base de datos física, y por lo tanto evita abrir conexiones y transacciones. Al estar en memoria, las lecturas son muchísimo más rápidas y se reduce la carga sobre la base de datos, mejorando el rendimiento general del sistema.




```jsx title=""
package com.tuempresa.config;

import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import java.time.Duration;

@Configuration
@EnableCaching
public class RedisConfig {

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory redisConnectionFactory) {

        // Configuración general del cache
        RedisCacheConfiguration cacheConfig = RedisCacheConfiguration.defaultCacheConfig()
                // TTL de 10 minutos para todos los caches
                .entryTtl(Duration.ofMinutes(10))
                // Serializador de keys como String
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(new StringRedisSerializer()))
                // Serializador de values como JSON
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(new GenericJackson2JsonRedisSerializer()))
                .disableCachingNullValues();

        return RedisCacheManager.builder(redisConnectionFactory)
                .cacheDefaults(cacheConfig)
                .build();
    }
}
```




Para guardar caché:
- Value: Nombre del caché

- Key: Nombre del parámetro del método

```jsx title="guardar caché"
   @Cacheable(value = "products", key = "#id")
    public ProductResponseDTO getById(Long id) {
        // Lógica de obtención del producto
    }
```




```jsx title="Eliminar caché"
   @CacheEvict(value = "products", key = "#id")
    public void UpdateById(Long id) {
        // Lógica de eliminación
    }
```


-------------------------------------------------------------------------------

## BLOQUE 10 - Concurrencia

Concurrencia es cuando dos o más requests, hilos o transacciones intentan acceder o modificar el mismo recurso al mismo tiempo. Soluciones fáciles de entender para entrevista:


### Estrategias:

-  Transacciones: todas las operaciones dentro del método se ejecutan como una unidad atómica.

Ejemplo al hacer un save
Se va guarda la entidad en el contexto de persistencia, buffer interno
Si todo sale bien → Spring hace commit y los cambios se persisten en la base de datos.
Si algo falla (una excepción no capturada) → Spring hace rollback y todos los cambios se revierten, como si nada hubiera pasado.


-  (Optimistic Locking) Bloqueo optimista: Es un mecanismo que evita que dos usuarios actualicen el mismo registro al mismo tiempo y se pisen los datos.
En la entidad agrego un campo version de tipo Integer anotado con @Version. Cada vez que se realiza un update, JPA incrementa automáticamente el contador. Es importante que el cliente reciba y reenvíe ese valor en cada request de actualización, así la API puede validar que está trabajando sobre la versión correcta y evitar pisar datos de otros procesos
En la entidad, se guarda un campo especial llamado versión (integer)
Cada vez que se actualiza la entidad, JPA aumenta el contador


-  (Pessimistic Locking) Bloqueo pesimista: Es un cuando un usuario va a leer o modificar un registro, la base lo bloquea para otros usuarios hasta que termine la operación.
Es útil cuando las colisiones son frecuentes y no queremos arriesgar sobreescrituras.
En optimista, el conflicto se detecta al final; en pesimista, se evita el conflicto desde el principio.

En Spring JPA se hace con @Lock(LockModeType.PESSIMISTIC_WRITE) en el repositorio y @Transactional en el servicio


-------------------------------------------------------------------------------

## BLOQUE 11 - Indices

En Spring JPA, los índices se definen en la entidad usando @Table(indexes=...). Los índices son estructuras que permiten buscar filas rápidamente, evitando recorrer toda la tabla. Por ejemplo, si tengo consultas frecuentes por nombre y precio, agrego índices sobre esas columnas. Esto mejora performance en selects masivos, aunque agrega un pequeña carga en inserts y updates.

Se  crea es una estructura interna (tipo árbol balanceado.) que el motor guarda dentro del mismo esquema, pero no aparece como tabla visible a nivel del usuario.


Un árbol balanceado es un árbol binario donde la diferencia de altura entre el subárbol izquierdo y el derecho está controlada, evitando que una rama se haga demasiado profunda. Esto garantiza que las operaciones de búsqueda, inserción y eliminación se mantengan rápidas


```jsx title=""
import jakarta.persistence.*;

@Entity
@Table(
    name = "products",
    indexes = {
        @Index(name = "idx_product_name", columnList = "name"),
        @Index(name = "idx_product_price", columnList = "price")
    }
)
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private Double price;

    private String description;

    // getters y setters
}

```



-------------------------------------------------------------------------------

## BLOQUE 12 - Auditoria

Para implementar auditoría tengo dos estrategias:
(1) Auditoría simple usando una clase base con campos como createdAt, updatedAt, createdBy, updatedBy.
(2) Auditoría histórica completa usando Hibernate Envers para guardar el historial completo de cambios en tablas _aud.


HIBERNATE ENVERS
Qué hace @Audited
- Cada vez que haces un insert, update o delete sobre la entidad, Envers crea un registro nuevo en la tabla de auditoría.
-	La tabla de auditoría guarda:
-	Los valores de todos los campos de la entidad.
-  La fecha de cambio.
- 	El tipo de cambio (ADD, MOD, DEL).
-	Un número de revisión (revision ID) para identificar el orden de los cambios.

-------------------------------------------------------------------------------

## BLOQUE 13 - AOP


La programación orientada a aspectos (AOP) permite separar lógicas transversales del negocio principal, como logging, auditoría, manejo de transacciones o seguridad.
En Spring, se puede implementar mediante aspects que interceptan la ejecución de métodos en clases específicas.
Por ejemplo, con un @Around advice, puedo:
•	Ejecutar código antes y después de un método.
•	Capturar los parámetros del método y su retorno.
•	Registrar logs de ejecución o fallos de manera centralizada sin ensuciar la lógica de negocio.
Esto facilita mantener un código limpio y consistente, ya que no repito la misma lógica de logging en cada método.

En mi proyecto implementé un sistema de auditoría usando AOP con Spring.
La idea fue centralizar el logging de acciones importantes sin ensuciar el código de los servicios.

Necesitaba registrar en base de datos quién ejecuta cada acción, con qué parámetros, qué resultado devuelve y si ocurrió algún error.
“Creé una anotación personalizada @LogAction y un aspecto LogActionAspect que intercepta cualquier método que tenga esa anotación.

1.	Intercepto el método anotado
2.	Obtengo los parámetros reales del método
3.	Ejecuto el método original
4.	Guardo logs en base


-------------------------------------------------------------------------------

## BLOQUE 14 - Pool de conexiones

Un pool de conexiones es un conjunto de conexiones abiertas a la base de datos que la aplicación mantiene listas para ser reutilizadas. No es de la API, sino de la conexión con la BD.

### Cómo funciona
•	Cuando la aplicación necesita acceder a la base de datos, toma una conexión del pool en lugar de abrir una nueva, lo que ahorra tiempo y recursos.
•	Al terminar la operación, la conexión se devuelve al pool para ser reutilizada por otra operación.
•	Si todas las conexiones del pool están en uso, nuevas solicitudes esperan hasta que alguna se libere, lo que puede causar esperas en picos de carga.

### Configuración de alto pico
•	Muchos pools permiten definir un número máximo de conexiones y un número mínimo de conexiones iniciales.
•	Durante un pico, el pool puede crear temporalmente conexiones adicionales hasta el límite máximo.
•	Esto ayuda a que la API siga respondiendo aunque haya muchos usuarios simultáneos.


### 	Simultaneidad
•	Cada usuario o hilo que necesite acceder a la base puede tomar una conexión disponible.
•	Si no hay conexiones libres, la solicitud queda en espera hasta que una se libere.


-------------------------------------------------------------------------------

## BLOQUE 15 - HILOS

Un hilo es una línea de ejecución. En Spring cada request HTTP se procesa en un hilo del thread pool del servidor.
Si tengo tareas que no deben bloquear ese request (por ejemplo enviar un mail o generar un reporte), uso @Async, que ejecuta la tarea en un pool de hilos separado.
En entornos concurrentes puede ocurrir un race condition si varios hilos acceden al mismo recurso. Para eso se usan mecanismos de concurrencia: sincronización, o a nivel de base de datos, locking optimista o pesimista en JPA



