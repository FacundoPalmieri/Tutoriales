---
sidebar_position: 1
---

# 1 - Extras


## **Nuevos desarrollos**

### Flujo de Autenticación

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

### Flujo del proceso -  Autenticación con Google OAuth2

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

----------------------------------------------------

### Model

#### -   Colocar @Entity en la clase


#### -   Como pensar las relaciones

-   En una relación OneToOne la clave foránea (FK) siempre va en la entidad dependiente de la relación.

Ejemplo, si tenemos 2 entidades:
    -   User
    -   Refresh Token

El usuario puede existir sin un refresh (usuario NO autenticado), en cambio para que el refresh se genere debe primero existir un usuario.

```jsx title="Entidad Refresh"
@OneToOne
    @JoinColumn(name = "user_id", nullable = false, unique = true)  // FK en RefreshToken
    private UserSec user;
``` 

<br/>

-   En una relación OneToMany o ManyToOne la FK SIEMPRE estará del lado de "Muchos", ejemplo:

Un empleado trabaja en un departamento (Empleado ↔ Departamento)
Un departamento tiene muchos empleados.

Un empleado solo puede pertenecer a un departamento.
🔹 ¿Quién depende de quién? → Empleado depende de Departamento.
🔹 ¿Dónde está el "muchos"? → Empleado.

![extra-relaciones](/img/extra-relaciones.png)

<br/>

- Para una relación ManyToMany se crea una tabla intermedia con ambas PK

<br/>


#### -  Cuando saber si un dato debe ser una tabla o un campo de otra tabla

![extra-model](../../static/img/extra-model.png)

-----------------------------------------------

### Controller
1. Colocar @RestController
2. Colocar ruta de controller

```jsx title="Ejemplo "
@RestController
@RequestMapping("/api/user") <--------------------------------
public class UserController {

    // Crear un usuario <--------------------------------
    @PostMapping    
    public ResponseEntity<User> createUser(@RequestBody User user) {
        // Implementación
    }

    // Eliminar un usuario por ID  <--------------------------------
    @DeleteMapping("/{id}") 
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        // Implementación
    }

    // Actualizar parcial un usuario  <--------------------------------
    @PatchMapping  
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        // Implementación
    }

    // Buscar un usuario por ID  <--------------------------------
    @GetMapping("/{id}")  
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
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


-----------------------------------------------

### Service
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


------------------------------------------------------------------

## **Desactivar copilot en VS Code**

![copilot-vs-1](/img/copilot-vs-1.png)

<br/><br/>

![copilot-vs-2](/img/copilot-vs-2.png)

<br/><br/>

![copilot-vs-3](/img/copilot-vs-3.png)


## **Elección de Nombres para todos los campos**

### groupId  (Identificador del grupo - POM.xml)

Este es generalmente el nombre del dominio de la empresa o de la organización invertido.
 
Si la empresa tiene dominio web odontologiaintegralfm.com, corresponde que vaya com.odontologiaintegralfm

```jsx title="Ejemplo"
<groupId>com.odontologiaintegralfm</groupId>

```


### artifactId (Identificador del artefacto - POM.xml)
Este es el nombre del proyecto y generalmente está en minúsculas y separado por guiones (-).
Si el proyecto es solo un componente (como una aplicación web que se comunica con un backend, por ejemplo), aplicacion-web es un buen nombre. Es claro y directo.

```jsx title="Ejemplo"
<artifactId>api</artifactId>
```

### name (Nombre del proyecto - POM.xml)
Este es el nombre completo del proyecto, utilizado en interfaces de usuario o documentación.
El nombre debe ser más descriptivo y completo que el artifactId. Puede incluir espacios o mayúsculas.

```jsx title="Ejemplo"
<name>odontologia-integral-fm-api</name>
<description>API REST para la aplicación web de Odontología Integral FM</description>
```


### modulename (Nombre del Módulo - Carpeta principal del proyecto)
Si tu proyecto tiene varios módulos (por ejemplo, un proyecto multi-módulo), el nombre del módulo generalmente debe seguir el mismo patrón que el artifactId, pero puede estar más enfocado en su funcionalidad específica.

**Si el proyecto es monolítico, este nombre podría coincidir con el artifactId.**

![module-name](/img/module-name.png)


### spring.application.name (Nombre para spring - application.properties)
Este valor es utilizado por Spring Boot para identificar la aplicación y puede ser útil en varias situaciones, como cuando tienes un sistema de microservicios o cuando se integran varios componentes, ya que se utiliza para dar un nombre a la aplicación en logs, métricas, y otras configuraciones.

```jsx title="Ejemplo"
spring.application.name= odontologiaintegralfm-api
```


### Renombra Package de proyecto (Estructura carpetas)

Debe contener el mismo nombre que el groupId

![package-rename](/img/package-rename.png)

### Refactor de Main

Nombre de la empresa, seguido de "application"

```jsx title="Ejemplo"
@SpringBootApplication
public class OdontologiaIntegralFmApiApplication {

	public static void main(String[] args) {
		SpringApplication.run(OdontologiaIntegralFMApplication.class, args);
	}
}
```
-------------------------------------------------------


## **Streams y Lambdas**

### *Sintaxis de .map()*

El método map() toma una función como argumento. Esa función define cómo transformar cada elemento de la lista original en un nuevo valor.

La estructura general es:

```jsx title="Ejemplo"
.map(elemento -> transformación)

```

#### 1.  elemento:

-   Es el objeto individual que estás procesando en el stream.

-   Corresponde a un elemento de la lista original.

-   Tú decides el nombre de esta variable (por ejemplo, tema, n, etc.).


#### 2.  transformación:

Es la operación que le haces al elemento.

Puede ser:

-   Acceder a un atributo.

-   Aplicar un método.

-   Realizar una operación matemática o lógica.



#### Regla para .map()

Piensa en .map() como un "transformador". Por cada elemento de la lista:

-   Nombramos al elemento (ejemplo: tema, numero, palabra).

-   Le hacemos algo (ejemplo: acceder a un atributo, aplicar una operación, llamar un método).

-   El resultado de esa transformación es lo que entra a la nueva lista.

#### Ejemplo Simple

Tienes una lista de números y quieres multiplicar cada número por 2:


```jsx title="Ejemplo"
List<Integer> numeros = List.of(1, 2, 3);

List<Integer> resultado = numeros.stream()
        .map(numero -> numero * 2) // "numero" es el elemento, y lo multiplicamos por 2.
        .collect(Collectors.toList());

System.out.println(resultado); // Salida: [2, 4, 6]

```

#### ¿Qué hay dentro del .map()?

**numero:** El elemento individual del stream (en este caso, cada número).

**numero * 2:** La transformación (multiplicar por 2).


### *Ejemplo real*

Tienes una lista de objetos Tema y quieres obtener los nombres (String) de esos temas. La transformación será "convertir un Tema en su nombre".

```jsx title="Ejemplo"
List<String> nombres = listaDeTemas.stream()
        .map(tema -> tema.getNombre()) // "tema" es el elemento, "tema.getNombre()" es la transformación.
        .toList();

```

#### ¿Qué hay dentro del .map()?

**tema:** Es el objeto actual del stream (cada Tema en la lista).

**tema.getNombre():** Es cómo convertimos un Tema en su nombre (String).


### *Referencias de Método*

Si estás llamando directamente a un método (como getNombre()), puedes usar una referencia de método en lugar de una expresión lambda. Es más corto pero hace lo mismo.

```jsx title="Ejemplo"
.map(Tema::getNombre)

```

Esto es equivalente a:

```jsx title="Ejemplo"
.map(tema -> tema.getNombre())

```


### *Convertir una lista de un tipo de Objeto a otro Tipo de Objeto (.map)*

```jsx title="Tema"
private List<Tema> listaDeTemas;

```

```jsx title="TemaDto"
private List<TemaDto> listaDeTemas,
```

```jsx title="Conversión"
List<TemaDto> temaDtos = temas.stream()                           // Fuente: Stream de objetos Tema.
        .map(tema -> new TemaDto(tema.getId(), tema.getNombre())) // Transformación: Tema -> TemaDto
        .toList();                                               // Recolectamos como lista

```


#### Paso a Paso Explicado

1.  **Fuente de datos:** .stream()

    -   Creamos un flujo (stream) desde la lista original **temas.**

    -   **temas.stream()** significa que cada elemento del stream será un objeto de tipo Tema.


2.  **Transformación: .map()**

    -   El **.map()** toma cada objeto Tema y lo transforma en un objeto **TemaDto.**

    -   La expresión lambda **tema -> new TemaDto(tema.getId(), tema.getNombre())**

        -   **tema:** Es un elemento individual de tipo Tema.

        -   **new TemaDto(tema.getId(), tema.getNombre()):** Crea un nuevo objeto TemaDto usando los datos de 
        tema.

3-  Recolectar en una lista:  **.toList();**

    -   Convierte el stream procesado (que ahora contiene objetos TemaDto) en una lista de tipo List< TemaDto>.




### *Builder con Map*



```jsx title="DTO"
@Data
@Builder
public class LoginResponseDTO {

    private Long id;
    private String username;
    private Set<Role> roles; // Lista de Roles
    private String jwt;
    private String refreshToken;
}

```


```jsx title="Ejemplo"
  LoginResponseDTO.builder()
                    .id(userSec.getId())
                    .username(userSec.getUsername())
                    .roles(userSec.getRolesList().stream() // 1. 
                           .map(role -> new Role(role.getId(), role.getRole(), role.getPermissionsList ())) // 2
                            .collect(Collectors.toSet()) // 3
                    )
                    .jwt(accessToken)
                    .refreshToken(refreshToken.getRefreshToken())
                    .build();                   
```

<br/>

#### Explicación.

1. stream() permite recorrer la colección y aplicar transformaciones (map()) sin necesidad de un bucle manual.

2. El método .map() en Java Stream transforma cada elemento de una colección en otro valor.
En cada role, creamos una nueva instancia y asignamos valores en el constructor.

3. El método .collect(Collectors.toSet()) en Java convierte un Stream en una colección (como Set, List, Map, etc.)

<br/>


#### Ejemplo con Referencia de Método


```jsx title="Ejemplo"


 private CursoDto converToDto (Curso curso) {

        return CursoDto.builder()
                .id(curso.getId())
                .nombre(curso.getNombre())
                .modalidad(curso.getModalidad())
                .fecha_finalizacion(curso.getFecha_finalizacion())
                .listaDeTemas(curso.getListaDeTemas().stream() // Que Tipo de dato tiene la lista? RTA = TEMA
                        .map(Tema::getId) // De ese tema, obtengo el ID
                        .toList())
        .build();


    }
```



### *Convertir una lista de un tipo de Objeto a otro Tipo de Objeto (referencia de método)*

Esto también puede realizarse de manera mas limpia

1.  En mi clase a al cual yo quiero convertir realizo un método:

```jsx title="Class TemaDto"
public class TemaDto {
    private Long id;
    private String nombre;

    // Constructor
    public TemaDto(Long id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    // ¡¡¡¡  Método estático que convierte un Tema a TemaDto!!!!!!!!
    public static TemaDto fromTema(Tema tema) {
        return new TemaDto(tema.getId(), tema.getNombre());
    }
}
```

2.  Luego, en el map(), usas la referencia de método:


```jsx title="Ejemplo"
List<TemaDto> temaDtos = temas.stream()
        .map(TemaDto::fromTema)  // Usamos el método estático fromTema
        .collect(Collectors.toList());


```

#### ¿Cuándo usarlo?

-   Usar la referencia de método es más limpio y reutilizable, especialmente si la conversión es más compleja o si necesitas reutilizarla en varios lugares.

-   Ideal cuando la conversión está definida de manera independiente en un método estático o de instancia, lo cual puede ser más fácil de mantener si la lógica es más compleja.

![comparacion-lambdas](/img/comparacion-lambas.png)



---------


## **Patrón Builder**

### *Construir builder simple*

```jsx title="Ejemplo"
    private Tema buildTemaDto(TemaDto temaDto, Curso curso) {

        return Tema.builder()
                .id(temaDto.id_Tema())
                .nombre(temaDto.nombre())
                .descripcion(temaDto.descripcion())
                .curso(curso)
                .build();
    }

```


### *Construir builder compuesto*

```jsx title="Ejemplo"
private Tema buildTemaDto(TemaDto temaDto, Curso curso) {
    return Tema.builder()
            .id(temaDto.id_Tema())
            .nombre(temaDto.nombre())
            .descripcion(temaDto.descripcion())
            .curso(Curso.builder() // Objeto Curso dentro de Objeto Tema
                    .id(temaDto.curso().getId())
                    .build()) // Construir el curso correctamente
            .build();

}

```

----

## **Return seguido de una operación**

En Spring Boot (y en Java en general), puedes utilizar un return seguido de una operación cuando deseas devolver directamente el resultado de una operación o la creación de un objeto. Esto es muy común en el uso de patrones de diseño como el Builder Pattern o en flujos funcionales como los proporcionados por las Streams. El ejemplo que proporcionaste encaja perfectamente en esta situación.

#### Casos donde es válido y útil realizar un return seguido de una operación:

1.  **Creación de Objetos Complejos:**

Usas un Builder (como en tu ejemplo) para construir un objeto de manera fluida y devolverlo inmediatamente. Esto es útil para mantener el código limpio y evitar la necesidad de variables temporales.
```jsx title="Ejemplo"
return Tema.builder()
    .id(temaDto.id_Tema())
    .nombre(temaDto.nombre())
    .descripcion(temaDto.descripcion())
    .curso(Curso.builder()
        .id(temaDto.curso().getId())
        .build())
    .build();

```


2.  **Streams y Operaciones Funcionales:**

Si estás trabajando con colecciones o flujos de datos, puedes devolver directamente el resultado de una operación funcional.


```jsx title="Ejemplo"
return temas.stream()
    .filter(tema -> tema.isActivo())
    .toList();

```

## **.orElseThrow**

En Spring Data, puedes usar .orElseThrow(() -> ...) en los métodos que devuelven un objeto del tipo Optional < T >, ya que este método es específico de la clase Optional. Aquí te explico cuáles métodos proporcionados por Spring Data son compatibles y en qué casos podrías usar .orElseThrow()

### *Métodos personalizados en el repository*

Acá el repositorio ya devuelve un opcional, por tal, en el servicio podemos:

-   Hacer un return directo de la clase que devuelve

-   Asignar a un objeto de la clase.

Em ambos casos se hará uso del .orElseThrow(() -> ...)


```jsx title="Repository"
@Repository
public interface IRoleRepository extends JpaRepository<Role, Long> {
    Optional<Role> findRoleEntityByRole(String role);
}

``` 



```jsx title="Servicio"
protected Role findbyRole(String role){
    return roleRepository.findRoleEntityByRole(role).orElseThrow(()-> new RoleNotFoundException("", 0L, role, "RoleService", "findbyRole"));
}

``` 

### *Algunos métodos de Spring Data que devuelven Optional*

1. **findbyId() :** Devuelve un Optional< T > donde T es la entidad que estás buscando.

```jsx title="Ejemplo"
return cursoRepository.findById(cursoId)
        .orElseThrow(() -> new CursoNotFoundException("El curso no existe"));

```
<br/><br/>

2. **findOne(Example< S > example)**: Busca una entidad que coincida con un Example. Si la encuentra, la devuelve como Optional< T >.

```jsx title="Ejemplo"
Example<Tema> example = Example.of(new Tema("Java Básico"));

return temaRepository.findOne(example)
        .orElseThrow(() -> new TemaNotFoundException("El tema no existe"));
```
<br/><br/>



### *Métodos que NO son compatibles directamente*

1. **findAll():** Devuelve una lista (List< T >), no un Optional. No puedes usar .orElseThrow directamente.

```jsx title="Ejemplo"
List<Curso> cursos = cursoRepository.findAll();
if (cursos.isEmpty()) {
    throw new CursoNotFoundException("No hay cursos disponibles");
}
return cursos;

```
<br/><br/>

2.  **save():** Devuelve la entidad guardada (T), no un Optional. No tiene sentido usar .orElseThrow

3. **deleteById(ID id)**: No devuelve ningún valor, simplemente realiza la operación de borrado.

4. **existsById(ID id)**: Devuelve un booleano (true o false), no un Optional.


-------------------------------------------------------------------------------------------------------------------------------------------

## **CORS  (Cross-Origin Resource Sharing)**

Es un mecanismo de seguridad implementado por los navegadores para restringir solicitudes HTTP entre diferentes orígenes (diferente dominio, puerto o protocolo).

Por defecto, los navegadores bloquean las solicitudes hechas desde un origen distinto al del servidor que responde. Esto impide que, por ejemplo, una aplicación web alojada en http://frontend.com haga peticiones a http://api.backend.com sin la autorización explícita del backend.

### ¿Cómo funciona CORS?

Cuando el frontend intenta hacer una petición a otro dominio (cross-origin request), el navegador envía una solicitud preflight (OPTIONS) al servidor para preguntar si permite la comunicación.

El backend debe responder con los encabezados adecuados para permitir la solicitud, como:

**Access-Control-Allow-Origin: http://frontend.com** → Permite solicitudes desde ese origen.

**Access-Control-Allow-Methods: GET, POST, PUT, DELETE** → Define qué métodos están permitidos.

**Access-Control-Allow-Headers: Content-Type, Authorization** → Indica qué encabezados personalizados pueden enviarse.

Si el backend no responde con estos encabezados, el navegador bloquea la solicitud y lanza un error de CORS en la consola.

### ¿Cómo solucionar errores de CORS?

Configurando CORS en el backend:

En Spring Boot, puedes usar @CrossOrigin en los controladores o configurar un filtro global.
En Express (Node.js), puedes usar el paquete cors.
Proxy en desarrollo: Configurar un proxy en el frontend para evitar el problema (Ej: en Angular con proxy.conf.json).

CORS en APIs públicas: Algunas APIs permiten CORS agregando * en Access-Control-Allow-Origin, pero esto es inseguro.

------------------------------------------------------------

## **Certificado SSL (Secure Sockets Layer)**

Un certificado SSL (Secure Sockets Layer) sirve para cifrar la comunicación entre el navegador del usuario y el servidor web, asegurando que los datos enviados y recibidos no puedan ser interceptados ni modificados por terceros.

#### ¿Por qué es importante el SSL?

**Cifrado 🔒:** Protege información sensible como contraseñas, datos personales y tarjetas de crédito.

**Autenticación ✅:** Garantiza que el usuario se está conectando al servidor correcto y no a un sitio fraudulento.

**Confianza 🏆:** Los navegadores muestran un candado en la barra de direcciones cuando un sitio usa SSL, lo que da confianza a los visitantes.

**SEO 📈:** Google favorece en los resultados de búsqueda a los sitios con SSL habilitado (HTTPS).

Hoy en día, el protocolo SSL ha sido reemplazado por TLS (Transport Layer Security), que es una versión más segura, pero aún se le sigue llamando SSL de manera común.

<br/>


------------------------------------------------------------

## **Certificado SSL (Secure Sockets Layer)**