---
sidebar_position: 1
---

# 1 - Nuevos Desarrollos


## Flujo de Autenticación

1. #### Inicio del Proceso de Autenticación

    - El proceso comienza cuando el usuario intenta iniciar sesión enviando sus credenciales (nombre de usuario y contraseña) al endpoint ("/login"). La solicitud es procesada por el método **login()** en el controlador, el cual delega la autenticación al servicio **userDetailsService.loginUser()**.

    ```jsx title=""
    @PostMapping("/login")
    public ResponseEntity<Response<AuthResponseDTO>> login(@RequestBody @Valid AuthLoginRequestDTO userRequest) {
        Response<AuthResponseDTO> response = this.userDetailsService.loginUser(userRequest);
        return new ResponseEntity<>(response, HttpStatus.OK);
    }
    ```

  

    - *(La clase userDetailsService tiene métodos que gestionan la autenticación de los usuarios, valida las credenciales, y devuelve un token JWT cuando la autenticación es exitosa.)*

2. #### Autenticación de las credenciales

    -   El método **LoginUser** autentica las credenciales por medio del método **authenticate** de la misma clase.

3. #### authenticate.

    -   3.1 El método **authenticate** realiza tres pasos principales:

        - **Obtiene los detalles del usuario** : Por medio del método **loadUserByUsername**  recupera los detalles del usuario desde la base de datos(Ej Roles y permisos) y los carga en una lista GrantedAuthority que luego se asignará a la autenticación. Este es un método crucial porque Spring Security necesita los detalles del usuario en un formato específico (UserDetails) para realizar la autenticación.

            -----------------------------------------------------------------------------
            #### loadUserByUsername

            -   El método retorna un UserDetails, que es una interfaz de Spring Security que encapsula la información necesaria para la autenticación y autorización de un usuario.

            -   Spring Security utiliza un objeto **User** con los detalles de usuario que contiene la lista de roles y permisos en formato **GrandAuthorityList**

            -   Si no se devuelve un UserDetails, Spring Security no podría gestionar correctamente el proceso de autenticación ni autorizar al usuario para acceder a recursos protegidos.

            --------------------------------------------------------------------------------

        -  **Compara Contraseñas**:  Verifica la contraseña recibida utilizando **PasswordEncoder**, en caso de no coinicidir, incrementa los intentos fallidos y bloquea la misma si alcanza el limite.

        - Verifica si la cuenta está activa

        - Luego de pasar todos los filtros, reinicia a cero los intentos fallidos de inicio de sesión

        - **Genera Authentication**: Si las credenciales son correctas, el método crea un objeto **UsernamePasswordAuthenticationToken** con el nombre de usuario, la contraseña y las autoridades del usuario con el userDetails recibido del **loadUserByUsername** . Este objeto es esencial porque es lo que Spring Security usa para mantener la autenticación del usuario en el contexto de seguridad.



<br/>


4. **Generación del JWT y respuesta al cliente:**

Cuando la autenticación es exitosa, se devuelve el control al método **loginUser** que realiza:

    - Almacenamiento de la autenticación en **SecurityContextHolder**
    *(El objeto Authentication contiene información sobre el usuario autenticado, como el nombre de usuario, contraseñas y roles o permisos. De aqui tomará información los filtros de seguridad)*

    - Llama al método **createToken** en la clase JwtUtils, lo que genera un JWT que contiene:

        -   El nombre de usuario (obtenido de authentication.getPrincipal()).
        -   Las autoridades o permisos (obtenidos de authentication.getAuthorities() y convertidos a un formato de cadena separada por comas).
        -   El issuer (quién generó el token).
        -   La fecha de expiración (en este caso, 30 minutos después de la emisión).
        -   Un ID único para el token.
        -   La fecha de inicio de validez (el token es válido inmediatamente).
        -   El token se firma utilizando el HMAC256 y una clave secreta, garantizando su integridad y seguridad.

*(La clase JwtUtils se encarga de manejar la creación, validación y extracción de información de un token JWT)*

    - **Elimina el Refresh Token anterior y genera uno nuevo**: Además del JWT, también se genera un refresh token, que se guarda en la base de datos. Este token permite renovar el JWT sin requerir que el usuario ingrese sus credenciales nuevamente.

    -  Por último, el método **loginUser** devuelve un objeto AuthResponseDTO, que incluye:
        - El nombre de usuario y los roles del usuario.

        - El JWT generado

        - El refresh Token  
    
     El token y el refresh Token deberá ser almacenado por el cliente (por ejemplo, en el almacenamiento local o en cookies seguras) y enviado en las solicitudes futuras mediante el encabezado HTTP Authorization.


5. **Validación del token en solicitudes posteriores:**

Cuando el cliente envía una solicitud posterior con el token JWT, la cadena de filtros del jwt intercepta la solicitud y  el **JwtTokenValidator** realiza los siguientes pasos:

    -   Extrae el token JWT del encabezado Authorization de la solicitud HTTP.
    -   Verifica la validez del token: Se valida que el token no haya expirado y que su firma sea válida utilizando la clave secreta y el algoritmo HMAC256.
    -   Extrae la información del token: Una vez validado, se extraen los datos contenidos en el token, como el nombre de usuario y las autoridades. Estos datos se utilizan para autenticar al usuario y otorgar acceso a los recursos solicitados.
    -   Si el token es válido, la autenticación se completa con éxito y Spring Security configura el contexto de seguridad para la solicitud posterior.

6. **Renovación del JWT con Refresh Token**: Si el JWT expira, el método refreshToken() permite renovar el JWT utilizando el refresh token.
    -   Se valida el refresh token, se elimina el antiguo y se genera uno nuevo.
    -   Se genera un nuevo JWT y se devuelve al usuario junto con el nuevo refresh token.


-------------------------------------------------------

## Flujo del proceso -  Autenticación con Google OAuth2

1. **Inicio de sesión con Google.**

    -   El usuario selecciona autenticarse con Google.

2. **Autenticación con Google OAuth2:**

    - Spring Security maneja el proceso de autenticación con Google, redirigiendo al usuario para que ingrese sus credenciales de Google.

3. **Configuración de Spring Security:**

    -   En SecurityConfig, se configura el inicio de sesión con OAuth2 y se agrega el filtro OAuth2UserFilter.

4. **Verificación del usuario registrado:**

    -   El filtro **OAuth2UserFilter** verifica si el usuario autenticado por Google está registrado en la base de datos.

5. **Generación del JWT:**

    -   Si el usuario está registrado, se llama a createToken en JwtUtils para generar un JWT.

6. **Generación del Refresh Token:**

    -   Se genera el refresh token.

7. **Respuesta al cliente**

    -   El JWT se añade al encabezado de la respuesta con Authorization: Bearer [token]

8. **Validación del token en solicitudes posteriores:**

Cuando el cliente envía una solicitud posterior con el token JWT, el **JwtTokenValidator** realiza los siguientes pasos:

    -   Extrae el token JWT del encabezado Authorization de la solicitud HTTP.
    -   Verifica la validez del token: Se valida que el token no haya expirado y que su firma sea válida utilizando la clave secreta y el algoritmo HMAC256.
    -   Extrae la información del token: Una vez validado, se extraen los datos contenidos en el token, como el nombre de usuario y las autoridades. Estos datos se utilizan para autenticar al usuario y otorgar acceso a los recursos solicitados.
    -   Si el token es válido, la autenticación se completa con éxito y Spring Security configura el contexto de seguridad para la solicitud posterior.



### Funcionamiento Interno de la Autenticación

#### 📌 ¿Cómo Spring Security procesa la autenticación?

#### Filtros HTTP interceptan las solicitudes:

Antes de llegar al controlador, las solicitudes HTTP pasan por una cadena de filtros de seguridad ubicado en **appConfig**. Este flujo incluye un filtro de autenticación JWT(implementado manualmente) que intercepta la solicitud para realizar tareas como:

-   Extraer el token JWT de la cabecera de la solicitud.

-   Validar la firma del token.

-   Comprobar la validez del token (es decir, si no ha expirado).

Este proceso ocurre al principio del ciclo de vida de la solicitud HTTP, antes de que Spring Security procese la autenticación. Si todo es correcto, Spring Security delega automáticamente la responsabilidad del proceso de autenticación al AuthenticationManager.


#### AuthenticationManager recibe las credenciales y delega la validación:

El AuthenticationManager es el componente que recibe las credenciales de autenticación (nombre de usuario y contraseña) y delega la validación a un AuthenticationProvider específico.

En nuestra implementación, no se usa el AuthenticationManager directamente como Spring Security lo haría por defecto, sino que se creó una lógica personalizada en el método authenticate. Sin embargo, el AuthenticationManager sigue siendo responsable de coordinar este proceso.

#### AuthenticationProvider se encarga de autenticar al usuario:

El AuthenticationProvider se encarga de autenticar al usuario validando las credenciales con los datos almacenados en la base de datos. En tu caso, este paso ocurre implícitamente dentro de la implementación personalizada de autenticación.

-   **Método loadUserByUsername(username):** Este es el punto donde Spring Security (o nuestra implementación personalizada) recupera la información del usuario. Si el usuario no existe, lanzará una excepción (como UserNameNotFoundException). Si el usuario existe, el siguiente paso es validar las credenciales.

-   **Llamada al método authenticate:** acá es donde la lógica desarrollada en el proyecto se encarga de realizar la validación de las credenciales. Spring Security, a través de su AuthenticationProvider, haría este paso automáticamente en su flujo, pero lo estás manejando tú explícitamente.

#### PasswordEncoder verifica que la contraseña sea correcta:

En este punto, el PasswordEncoder compara la contraseña proporcionada por el usuario con la almacenada en la base de datos.

En nuestra implementación, este paso es gestionado de manera explícita en el método authenticate. Si las contraseñas no coinciden, el flujo incrementa los intentos fallidos y puede bloquear la cuenta si se alcanzan los límites establecidos.

En caso de éxito se retorna un objeto Authentication **UsernamePasswordAuthenticationToken** al método **loginUser**

Este paso, que estás manejando de manera explícita, normalmente sería gestionado automáticamente por el AuthenticationProvider en un flujo estándar de Spring Security.

####  El flujo regresa a tu método loginUser después de la autenticación exitosa:

Si la autenticación es exitosa, en el método **loginUser** se realiza las siguientes acciones: 

- El objeto Authentication (que contiene detalles como el nombre de usuario y las autoridades) se almacena en el SecurityContextHolder. Esto permite que el usuario esté autenticado y acceda a los recursos protegidos del sistema durante su sesión, sin necesidad de volver a ingresar sus credenciales en cada solicitud.

- Se elimina el refresh Token anterior.

- Se generar el JWT y el nuevo Refresh Token.

- Se devuelve al cliente un objeto authResponseDTO, con detalles del usuario autenticado.


## --------------------------------------


## Java Mail Sender

Es una interfaz de Spring que actúa como un cliente de correo. Permite enviar emails desde una aplicación Java utilizando el protocolo SMTP( Simple Mail Transfer Protocol - Protocolo simple de transferencia de correo) por ejemplo, usando Gmail, Outlook, etc.

Se puede: 

-   Adjuntar archivos con MimeMessageHelper.

-   Enviar correos HTML.

-   Enviar a varios destinatarios.

-   Agregar imágenes incrustadas.

<br/>

1.  Configuración del servidor SMTP

En el archivo application.properties o application.yml, configuras los datos del servidor de correo.

```jsx title=""
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=tu.email@gmail.com
spring.mail.password=tu_contraseña
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
``` 



2. Crear un servicio e inyecatar JavaMailSender

```jsx title=""
@Autowired
private JavaMailSender mailSender;

``` 

3. Crear el mensaje
```jsx title=""
SimpleMailMessage message = new SimpleMailMessage();
message.setFrom("tu.email@gmail.com");
message.setTo("destinatario@email.com");
message.setSubject("Asunto del correo");
message.setText("Contenido del correo");

``` 


## --------------------------------------

## Estructura de datos

### Normalización de tablas
<br/>

#### Set de Datos

![set-datos.png](/img/set-datos.png)


<br/>

#### 1° Forma Normal

→ Separamos los datos para que cada celda tenga un solo valor

→ Cada combinación única se pone en una fila

![1-forma-normal.png](/img/1-forma-normal.png)

<br/>

#### 2° Forma Normal

-   Solo se aplica si la PK es compuesta.

-   Si la clave primaria tiene dos columnas (compuesta), cada dato de la tabla debe depender de las dos juntas, no de una sola.


![2-forma-normal-b.png](/img/2-forma-normal-b.png)




👉 Aquí la clave primaria es (ID_Alumno + Curso), porque un mismo alumno puede estar en varios cursos.

🔍 Ahora revisamos qué columna depende solo de parte de la clave:

Nombre_Alumno depende solo de ID_Alumno

❌ No depende del curso → ¡esto rompe la regla de 2FN!

Profesor depende del curso

❌ No depende del alumno → también rompe la regla


![2-forma-normal.png](/img/2-forma-normal.png)

<br/>

#### 3° Forma Normal

-   Eliminar columnas que no dependen directamente de la clave primaria, sino de otra columna.

-   En este caso, en la tabla Curso_Profesor, el profesor depende del curso, no necesitamos modificar más porque ya cumple 3FN.


![resumen-normalización](/img/resumen-normalizacion.png)


<br/>

### Relaciones

#### OneToOne

-   En una relación OneToOne la clave foránea (FK) siempre va en la entidad dependiente de la relación o de la "menos importante"

Ejemplo, si tenemos 2 entidades:
    -   User (importante)
    -   Refresh Token (dependiente)

El usuario puede existir sin un refresh (usuario NO autenticado), en cambio para que el refresh se genere debe primero existir un usuario.

```jsx title="Entidad Refresh"
@OneToOne
    @JoinColumn(name = "user_id", nullable = false, unique = true)  // FK en RefreshToken
    private UserSec user;
``` 

<br/>

#### OneToMany o ManyToOne

-   En una relación OneToMany o ManyToOne la FK SIEMPRE estará del lado de "Muchos", ejemplo:

Un empleado trabaja en un departamento (Empleado ↔ Departamento)

Un departamento tiene muchos empleados.

Un empleado solo puede pertenecer a un departamento.


🔹 ¿Quién depende de quién? → Empleado depende de Departamento.

🔹 ¿Dónde está el "muchos"? → Empleado.

![extra-relaciones](/img/extra-relaciones.png)


<br/>

#### ManyToMany

La relación solo está del lado de la tabla intermedia.

- Se crea una tabla intermedia con las PK de las otras dos tablas.

- Las 2 tablas en cuestión, se desconocen entre sí.

- Las 2 tablas en cuestión, desconocen la tabla intermedia.


<br/>

#### -  Cuando saber si un dato debe ser una tabla o un campo de otra tabla

![extra-model](../../static/img/extra-model.png)

<br/>

## --------------------------------------


## Model

-   Colocar @Entity en la clase

-   Colocar @Data en la clase

    -   Lombok crea automáticamente:

        ✅ getters y setters para todos los campos

        ✅ toString()

        ✅ equals() y hashCode()

        ✅ Un constructor vacío

        ✅ Métodos canEqual() (útil para herencia)

-   Nombrar la tabla en inglés @Table(name= "nombre")

-   Agregar restricciones de bases de datos en los atributos con la anotación @Column o @JoinColumn para relaciones.


```jsx title="Ejemplo"
@Entity
@Data
@Table(name= "countries")
public class Pais {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    @Column(length = 15, unique = true, nullable = false)
    private String name;

    @Column(length = 15, unique = true, nullable = false)
    private String nationality;

    @Column(nullable = false)
    private boolean enabled;
}
``` 


<br/>

### Sintaxis Relación OneToOne

La relación estará del lado de la entidad menos importante o dependiente.

Agregaremos:

-   Un atributo de la otra clase usando composición.

-   Anotation @OneToOne

-   Propiedad (targetEntity = UserSec.Class) para establecer con que clase se hará la relación.

-   Ver si corresponde agregar propiedad "cascade = CascadeType.xxx" para establecer comportamiento en cascada. Es decir, cualquier acción de insertar, eliminar, actualizar, etc impactará en la tabla relacionada.

- Utilizamos la annotation @JoinColumn para establecer el nombre en la columna unión.

```jsx title="Entidad Refresh"
@OneToOne
    @JoinColumn(name = "user_id", nullable = false, unique = true)  // FK en RefreshToken
    private UserSec user;
``` 



### Sintaxis Relación ManyToOne o OneToMany

En la entidad Muchos:

- Un atributo de la otra clase usando **composición.**

- Anotation @ManyToOne(targetEntity = otraClase.class): Especifica la entidad destino de la relación.

- Utilizamos la annotation @JoinColumn: Establece el nombre en la columna unión.

```jsx title="Entidad MUCHOS"
@ManyToOne(targetEntity = NombreClaseEntidadUno.class) // Curso.class
@JoinColumn(name = "curso_id")
private Curso curso;

``` 

#### Para Agregar Bidireccionalidad (no recomendando)

En la entidad Uno:

-   Colocaremos una lista del otro objeto usando  **composición.**

- Anotation @OneToMany(targetEntity = otraEntidad.class, fetch = FetchType.LAZY, mappedBy = atributoDeLaOtraEntidad)




### Sintaxis Relación ManyToMany

- Un atributo de la otra clase usando **composición.**

- Anotation @ManyToMany(targetEntity = otraClase.class)

- Anotation @JoinTable


```jsx title="Entidad CLUB"
@JoinTable(
    name = "club_competitions",   // 1 Nombre tabla intermedia
    joinColumn = @JoinColumn(name = "club"), // 2 Nombre de la columna
    inverseJoinColumns = @JoinColumn(name= "competition") // 3 Nombre de la columna
)
private List<OtraEntidad> otraEntidad;

``` 
1. Nombre tabla intermedia

2. Define el nombre de la columna de clave foránea en la tabla intermedia (club_competitions) que apunta a la entidad actual (Club). En este caso, la columna club en la tabla intermedia referenciará el ID de la tabla Club.

3. Define el nombre de la columna de clave foránea inversa en la tabla intermedia que apunta a la entidad relacionada (Competition).


<br/>

### Herencia

🔧 En JPA (Spring Boot):

Usa @MappedSuperclass

-   Se mapean en la base de datos las clases Hijas con todos los atributos de la clase padre

-   No usar @Table ni @Entity ya que no se mapeara a la base de datos.
```jsx title="Clase Persona - Padre"

@MappedSuperclass
@Data
// NO USAR @Table
public abstrac class Persona {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
}

``` 



```jsx title="Clase Empleado - Hija"
@Entity
@Data
public class Empleado extends Persona {

    //Hereda id de persona

    private String legajo;
}

``` 

⚠️ IMPORTANTE:

-   No hay que declarar @Id en la clase hija (Empleado), ya lo hereda de Persona.

-   No necesitás poner manualmente la relación entre las tablas. JPA lo maneja.



![herencia-jpa.png](/img/herencia-jpa.png)


## --------------------------------------

## Controller
1. Colocar @RestController
2. Colocar ruta de controller

```jsx title="Ejemplo "
@RestController
@RequestMapping("/api/user") <--------------------------------
public class UserController {

    // Crear un usuario <--------------------------------
    @PostMapping    
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        // Implementación
    }

    // Eliminar un usuario por ID  <--------------------------------
    @DeleteMapping("/{id}") 
    public ResponseEntity<Void> deleteUser(@Valid @PathVariable Long id) {
        // Implementación
    }

    // Actualizar parcial un usuario  <--------------------------------
    @PatchMapping  
    public ResponseEntity<User> updateUser(@Valid @PathVariable Long id, @RequestBody User user) {
        // Implementación
    }

    // Buscar un usuario por ID  <--------------------------------
    @GetMapping("/{id}")  
    public ResponseEntity<User> getUserById(@NotNull @PathVariable ("id") Long idObjeto) {
        // Implementación
    }

    // Obtener todos los usuarios  <--------------------------------
    @GetMapping ("/all") 
    public ResponseEntity<List<User>> getAllUsers() {
        // Implementación
    }
}

```


3. Que tipo de método? get, post? y ruta
4. Que tipo de autorización requiere.
5. Que tipo de respuesta necesito dar?
6. Que dato necesito recibir y validaciones de entrada debe tener
7. Realizar la documentación.


### Path Variable

```jsx title=""
 @GetMapping("/localities/{id}")
    @PreAuthorize("hasAnyRole(@userRolesConfig.administradorRole," +
            "                 @userRolesConfig.secretariaRole)")
    public ResponseEntity<Response<List<LocalityResponseDTO>>> getLocalitiesByIdProvinces(@NotNull @PathVariable("id") Long idProvinces) {
        Response<List<LocalityResponseDTO>>response = geoService.getLocalitiesByIdProvinces(idProvinces);
        return new ResponseEntity<>(response, HttpStatus.OK);
    }
``` 

## --------------------------------------

## Service
1.  Colocar @Service en la clase
2.  Anotaciones de @Transactional para escritura en bd solo a **metodos públicos**
3.  Utilizar try catch para BD
4.  Pensar bien todas las Validaciones e intentar realizar método reutilizables.
5.  Nombre de métodos:
  
    ✔ Usa getXxx si el objeto siempre debe existir y lanzarás una excepción si no lo encuentras (Servicio)

    ✔ Usa findXxx si el objeto puede no existir y quieres que el llamador decida cómo manejarlo.(Repositorio)

    **Resumen**

    **Repositorios:** Los métodos del repositorio, como findById, están diseñados para realizar consultas y devolver resultados, pero no deben lanzar excepciones. Devuelven un Optional< T> o List< T>, ya que el recurso puede o no existir.
    -   find
    -   findAll
    -   exists
    -   etc.

    **Servicios:** En el servicio, los métodos deberían ser responsables de validar si los resultados del repositorio cumplen con las expectativas de la lógica de negocio. Si un recurso debe existir, entonces el método del servicio puede usar getXxx y lanzar excepciones si no se encuentra el recurso.
    -  get
    -  create
    -  update
    -  delete
    -  etc


## --------------------------------------

## Repository

Crear uno por cada entidad. En algunos casos podría hacerse un solo service (ej: geoService) que se conecte a cada repository (ej, pais, prov, localidad)


1. Poner annotation @Repository

2. Implementa JpaRepository < NombreEntidad, TipoDatoId >

```jsx title=""
@Repository
public interface IRefreshTokenConfigRepository extends JpaRepository<RefreshTokenConfig, Long>
``` 

Métodos básicos sin necesidad de agregar nada en Respository: 

-   findAll()

-   findById(ID id)

-   save(T entity)

-   saveAll(Iterable< T > entities)

-   deleteById(ID id)

-   delete(T entity)

-   existsById(ID id)

-   count()


## --------------------------------------

## DTO

### Validaciones

1. @NotNull

-   Parte de la especificación de validación de Java

-   Verifica que el valor de un atributo no sea null.

-    Usarlo en datos de tipo primitivo.


2. @NotEmpty

-   Parte de la especificación de validación de Java.

-   Verifica que un campo no esté vacío. Se aplica específicamente a strings, colecciones o listas. Asegura que el atributo no sea null ni esté vacío (una cadena vacía "" o una lista vacía []).

-   Usado en strings, colecciones, listas.


3. @NonNull

-   Similar a @NotNull, pero es parte de Lombok (no es parte de la especificación de validación de Java). @NonNull también asegura que el valor no sea null, pero genera una validación a nivel de código, no directamente en el framework de validación.

-   Verifica valores nulos en tiempo de ejecución.