---
sidebar_position: 10
---

# 10 - Proyecto


En este apartado intentaremos comprender los concpetos generales, mediante el paso a paso, en la creación de un proyecto utilizando las herramientas de **Spring boot + Spring Security + OAuth2.**

## Flujo del proceso -  Autenticación con credenciales (usuario y contraseña)

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



### *Funcionamiento Interno de la Autenticación*

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

-----------

## **Implementación - Spring Security**

## Creación del Proyecto
![inicio proyecto](/img/InicioProyecto.png)

### *Dependencias*
![dependencias](/img/dependencias.png)

### *Configuraciones en el POM.xml*
1. Agregamos la dependencia de JWT
Para obtener la más actual entramos a la siguiente web y buscamos la versión en el apartado "dependencias":

https://github.com/auth0/java-jwt

```jsx title="Agregar dependencia JWT"
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>4.4.0</version>
</dependency>
<dependency>
```

2. Agregamos la dependencia para la annotations de validación
```jsx title="Agregar dependencia Annotations"
<dependency>
    <groupId>jakarta.validation</groupId>
    <artifactId>jakarta.validation-api</artifactId>
    <version>3.0.2</version>
</dependency>
```


3. Agregamos la dependencia para envío de emails.
```jsx title="Agregar dependencia envío emails"
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>

<dependency>
	<groupId>com.sun.mail</groupId>
	<artifactId>jakarta.mail</artifactId>
	<version>2.0.1</version> 
</dependency>
```


### *Configuraciones en el application.properties*
- Base de datos mediante variables de entorno.

```jsx title="Configuraciones de BD"
#Configuraciones de BD
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=${BD_URL} // jdbc:mysql://localhost:3306/NombreBD?createDatabaseIfNotExist=true
spring.datasource.username=${BD_USER}
spring.datasource.password=${BD_PASSWORD}
```

- Clave privada para la firma del token (JWT).

```jsx title="Configuraciones de JWT"
#Config de JWT
security.jwt.private.key=${PRIVATE_KEY}
#Usuario generador del Token - Poner uno representativo
security.jwt.user.generator=${USER_GENERATOR}

```

- Configuración para oatuh2

```jsx title="oauth2"
#Configuración para Google
spring.security.oauth2.client.registration.google.client-id=${GOOGLE_CLIENT_ID}
spring.security.oauth2.client.registration.google.client-secret=${GOOGLE_CLIENT_SECRET}
```

- Configuración para envío de emails

```jsx title="Configuración para envío de emails"
# Configuración básica del servidor de correo
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=${EMAIL_USERNAME}
spring.mail.password=${EMAIL_PASSWORD}

# Propiedades adicionales para autenticación y cifrado
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.connectiontimeout=5000
spring.mail.properties.mail.smtp.timeout=5000
spring.mail.properties.mail.smtp.writetimeout=5000
spring.mail.properties.mail.smtp.ssl.trust=smtp.gmail.com
```

## Creación Package model

### *Clases* 
- Permission
- Role
- UsuarioSec

**Ejemplos**

```jsx title="Class Permission"
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Table(name="permissions")
public class Permission {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String permissionName;

}
```


```jsx title="Class Role"
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Table(name="roles")
public class Role {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String role;

    //Usamos Set porque no permite repetidos
    //List permite repetidos
    @ManyToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
    @JoinTable (name = "roles_permissions", joinColumns = @JoinColumn(name = "role_id"),
            inverseJoinColumns=@JoinColumn(name = "permission_id"))
    private Set<Permission> permissionsList = new HashSet<>();
}

```


```jsx title="UsuarioSec"
@Entity
@Table(name="users")
@Getter @Setter
@AllArgsConstructor
@NoArgsConstructor
public class UserSec {
    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String username;
    private String password;

    private boolean enabled;
    private boolean accountNotExpired;
    private boolean accountNotLocked;
    private boolean credentialNotExpired;

    //Usamos Set porque no permite repetidos
    //List permite repetidos
    @ManyToMany(fetch = FetchType.EAGER, cascade = CascadeType.ALL) //el eager me va  a cargar todos los roles
    @JoinTable (name = "user_roles", joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns=@JoinColumn(name = "role_id"))
    private Set<Role> rolesList = new HashSet<>();


}
```

## Creación Package repository
### *Interfaces* 

```jsx title="IPermissionRepository"
@Repository
public interface IPermissionRepository extends JpaRepository<Permission, Long> {

}

```

```jsx title="IRoleRepository"
@Repository
public interface IRoleRepository extends JpaRepository<Role, Long> {
}
```

```jsx title="IUseRepository"
@Repository
public interface IUserRepository extends JpaRepository<UserSec, Long> {

    Optional<UserSec> findUserEntityByUsername(String username);


}

```
:::info
Se realiza una consulta Personalizada para buscar un username, pasandole como parámetro el username que nos llega en la request. La consulta explicita la arma Spring ya que al estár en inglés entiende lo que tiene que buscar
:::

## Creación Package service
### *Interfaces*
- IPermissionService
- IRoleService
- IUsuarioSec


```jsx title="IPermissionService"
public interface IPermissionService {

    List<Permission> findAll();
    Optional<Permission> findById(Long id); // --- > TipoDato Optional
    Permission save(Permission permission);
    void deleteById(Long id);
    Permission update(Permission permission);

}
```

:::info
Desde Java 8 se incorporó el tipo de dato Optional, como una forma de manejar valores que podrían estar presentes o ausentes, reduciendo el riesgo de tener NullPointerException.
:::
<br/><br/>

```jsx title="IRoleService"
public interface IRoleService {
    List<Role> findAll();
    Optional<Role> findById(Long id);
    Role save(Role role);
    void deleteById(Long id);
    Role update(Role role);
}

```

```jsx title="IUserService"
public interface IUserService {

    public List<UserSec> findAll();
    public Optional<UserSec> findById(Long id);
    public UserSec save(UserSec userSec);
    public void deleteById(Long id);
    public void update(UserSec userSec);
    public String encriptPassword(String password);
}


```

### *Implementación*
- PermissionService
- RoleService
- UserService :
     - Se realiza el CRUD de usuario como en el resto de las implementaciones + Encriptado de pass.


```jsx title="PermissionService"
@Service
public class PermissionService implements IPermissionService {

    @Autowired
    private IPermissionRepository permissionRepository;

    @Override
    public List<Permission> findAll() {
        return permissionRepository.findAll();
    }

    @Override
    public Optional<Permission> findById(Long id) {
        return permissionRepository.findById(id);
    }

    @Override
    public Permission save(Permission permission) {
        return permissionRepository.save(permission);
    }

    @Override
    public void deleteById(Long id) {
        permissionRepository.deleteById(id);
    }

    @Override
    public Permission update(Permission permission) {
        return permissionRepository.save(permission);
    }
}

```


```jsx title="RoleService"
@Service
public class RoleService implements IRoleService {

    @Autowired
    private IRoleRepository roleRepository;

    @Override
    public List<Role> findAll() {
        return roleRepository.findAll();
    }

    @Override
    public Optional<Role> findById(Long id) {
        return roleRepository.findById(id);
    }

    @Override
    public Role save(Role role) {
        return roleRepository.save(role);
    }

    @Override
    public void deleteById(Long id) {
        roleRepository.deleteById(id);
    }

    @Override
    public Role update(Role role) {
        return roleRepository.save(role);
    }
}
```

```jsx title="UserService"
@Service
public class UserService implements IUserService {

    @Autowired
    private IUserRepository userRepository;

    @Override
    public List<UserSec> findAll() {
        return userRepository.findAll();}

    @Override
    public Optional<UserSec> findById(Long id) {
        return userRepository.findById(id);
    }

    @Override
    public UserSec save(UserSec userSec) {
        return userRepository.save(userSec);
    }

    @Override
    public void deleteById(Long id) {
        userRepository.deleteById(id);
    }

    @Override
    public void update(UserSec userSec) {
        save(userSec);
    }


    // Encriptado de password
    @Override
    public String encriptPassword(String password) {
        return new BCryptPasswordEncoder().encode(password);
    }
}

```

### *Creación del servicio UserDetailServiceImp*


Esta clase **extiende de la clase UserDetailService de SpringSecurity**. Será la encargada de recuperar todos los datos del usuario, y comunicárselo al Authentication Manager.

- Se inyecta la dependencia de IUserRepository
```jsx title="Inyección de dependencia"
@Autowired
    private IUserRepository userRepo;
```

- Se crea el método para buscar en la base de datos.
```jsx title="Método"
@Override
    public UserDetails loadUserByUsername (String username) throws UsernameNotFoundException {
```
:::info 

**loadUserByUsername**

Este método retorna un UserDetails, y nosotros contamos con el objeto UsuarioSec, por tal debemos recuperar primero todos los atributos de esa clase y transformalo a UserDetail para retornalo.
:::


```jsx title="loadUserByUsername"
@Service
public class UserDetailsServiceImp implements UserDetailsService {

    @Autowired
    private IUserRepository userRepo;

    @Autowired
    JwtUtils jwtUtils;

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public UserDetails loadUserByUsername (String username) throws UsernameNotFoundException {

        //Contamos con usuario de tipo  Usersec y necesitamos devolver un tipo UserDetails
        //Recuperamos el usuario de la bd
        UserSec userSec = userRepo.findUserEntityByUsername(username)
                .orElseThrow(()-> new UsernameNotFoundException("El usuario " + username + "no fue encontrado"));

        //Spring Security maneja permisos con GrantedAuthority
        //Creamos una lista de SimpleGrantedAuthority para almacenar los permisos
        List<SimpleGrantedAuthority> authorityList = new ArrayList<>();

        //Programación funcional

        //Obtenemos roles y los convertimos en SimpleGrantedAuthority para poder agregarlos a la authorityList
        userSec.getRolesList()
                .forEach(role -> authorityList.add(new SimpleGrantedAuthority("ROLE_".concat(role.getRole()))));


        //Obtenemos los permisos y los agregamos a la lista.
        userSec.getRolesList().stream()
                .flatMap(role -> role.getPermissionsList().stream()) //acá recorro los permisos de los roles
                .forEach(permission -> authorityList.add(new SimpleGrantedAuthority(permission.getPermissionName())));

        //Retornamos el usuario en formato Spring Security con los datos de nuestro userSec
        return new User(userSec.getUsername(),
                userSec.getPassword(),
                userSec.isEnabled(),
                userSec.isAccountNotExpired(),
                userSec.isCredentialNotExpired(),
                userSec.isAccountNotLocked(),
                authorityList);
    }
}

```



## Creación Package security.config
### *Clase*  
- SecurityConfig

### *Annotations*
-   **@Configuration:** Le comunica a Spring que es una clase de Configuración

-   **@EnableWebSecurity:** Spring Security habilita la configuración personalizada de seguridad web en una aplicación Spring.

-   **@EnableMethodSecurity:** Habilita la seguridad a nivel de métodos en tu aplicación. Es decir, que la securización será por cada endpoint.

### *Métodos*
- 	SecurityFilterChain
- 	AuthenticationManager
-   AuthenticationProvider
-   PasswordEncoder

#### Desarrollo de Métodos
-	### *SecurityFilterChain*

Contiene la configuración de los filtros de seguridad que se aplicarán a las solicitudes HTTP.


#### Filtros del método
1. *csrf(csrf -> csrf.disable()):* Desactiva la protección CSRF (Cross-Site Request Forgery). Esto suele desactivarse en aplicaciones que usan tokens (como JWT) para manejar la autenticación, ya que la verificación CSRF no es necesaria.

2. *.formLogin:* Establece un formulario de inicio de sesión.

3. *.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)):* Configura la política de creación de sesiones a STATELESS, lo que significa que la aplicación no almacenará información de sesión en el servidor. Esto es típico en aplicaciones que usan autenticación basada en tokens (JWT), ya que la información de autenticación viaja con cada solicitud en el token.

:::info
El método retorna **HttpSecurity**, es el objeto que permite configurar las reglas de seguridad HTTP en Spring Security. Con esto, puedes configurar aspectos como la protección CSRF, los mecanismos de autenticación, la gestión de sesiones, etc.
:::


```jsx title="Método"
   @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(csrf -> csrf.disable())
                .formLogin(Customizer.withDefaults())
                .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .build();
    }
```




-	### *AuthenticationManager*


El AuthenticationManager en Spring Security es el componente central que gestiona la autenticación de los usuarios en la aplicación. Su función principal es validar las credenciales del usuario y determinar si son válidas para acceder al sistema.

Este método asegura que el AuthenticationManager de Spring Security pueda acceder automáticamente a todos los AuthenticationProvider registrados

```jsx title="Método"
@Bean
 public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
    return authenticationConfiguration.getAuthenticationManager();
}



Explicación:

-   ¿Qué es un BEAN?:
 Un bean es un objeto que forma parte del contexto de la aplicación gestionado por el contenedor de Spring. En términos sencillos, es un componente que Spring Framework crea, configura y gestiona a lo largo del ciclo de vida de la aplicación.

-   @Bean:
Esta anotación se usa en Spring para indicar que un método produce un bean que debe ser administrado por el contenedor de Spring. En este caso, el método authenticationManager está siendo registrado como un bean en el contexto de la aplicación Spring.

-   public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception:
Recibe un parámetro AuthenticationConfiguration, que es una clase proporcionada por Spring Security que contiene la configuración necesaria para gestionar la autenticación en la aplicación.
El método puede lanzar una excepción de tipo Exception.

-   return authenticationConfiguration.getAuthenticationManager():
Este método llama a getAuthenticationManager() del objeto authenticationConfiguration y devuelve un AuthenticationManager.
AuthenticationConfiguration es una clase que facilita la configuración de AuthenticationManager. Al llamar a getAuthenticationManager(), se está delegando la responsabilidad de construir y configurar el AuthenticationManager a AuthenticationConfiguration, que ya tiene toda la información necesaria para hacerlo (por ejemplo, detalles sobre los proveedores de autenticación configurados).


```





-   ### *AuthenticationProvider*

El AuthenticationProvider en Spring Security es una interfaz que define cómo se lleva a cabo la autenticación en la aplicación. Su propósito principal es validar las credenciales de los usuarios y devolver un objeto Authentication si la autenticación es exitosa.

```jsx title="Método"
@Bean
 public AuthenticationProvider authenticationProvider(UserDetailsService userDetailsService){
    DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
    provider.setPasswordEncoder(passwordEncoder());

    provider.setUserDetailsService(userDetailsService);

    return provider;
}

Explicación:

Recibe como parámetro UserDetailService.
    - Se crea un proveedor
    - Se establece el método para encriptar contraseña
    - Se setea el userDetailService
    - Se retorna el proveedor.


```


:::tip[Comunicación-entre-AuthenticationManager-y-AuthenticationProvider]

Flujo de comunicación:

**El AuthenticationManager coordina el proceso de autenticación:**

-   Cuando recibe una solicitud de autenticación (por ejemplo, usuario y contraseña), delega el trabajo a uno o más AuthenticationProvider.

-   El AuthenticationProvider realiza la autenticación:

    -   Valida las credenciales del usuario (por ejemplo, comparando la contraseña con la almacenada en la base de datos).

    -   Si las credenciales son válidas, devuelve un objeto Authentication con los detalles del usuario autenticado. Si no, lanza una excepción.


**Resultado devuelto al AuthenticationManager:**

-   Si un AuthenticationProvider logra autenticar al usuario, el AuthenticationManager devuelve el resultado (un objeto Authentication).

-   Si ninguno de los AuthenticationProvider registrados puede autenticar al usuario, lanza una excepción como BadCredentialsException.


**Cómo se comunican en el código:**

1. Registro de AuthenticationProvider en el contexto de seguridad:
Cuando defines el AuthenticationProvider como un @Bean, Spring Security lo detecta automáticamente y lo registra en el AuthenticationManager.

2. El AuthenticationManager utiliza los AuthenticationProvider:
Al usar el AuthenticationConfiguration.getAuthenticationManager(), Spring Security construye un AuthenticationManager que incluye todos los AuthenticationProvider registrados en el contexto.

**En este caso:**

El método authenticationProvider configura un DaoAuthenticationProvider para autenticar usuarios basados en el UserDetailsService (que recupera los datos de usuario, como nombre y contraseña).

El método authenticationManager asegura que Spring Security pueda usar los proveedores configurados.


**¿Cómo funciona en la práctica?**

Cuando el usuario intenta iniciar sesión, las credenciales (usuario y contraseña) se envían al AuthenticationManager.

El AuthenticationManager delega el trabajo al DaoAuthenticationProvider.

**El DaoAuthenticationProvider:**

Usa el UserDetailsService para buscar al usuario en la base de datos u otra fuente.

Valida la contraseña usando el PasswordEncoder.

Si la autenticación es exitosa, devuelve un objeto Authentication con los detalles del usuario autenticado.

Si falla, se lanza una excepción (como BadCredentialsException).

**Flujo visual**

[AuthenticationManager]

        ↓ Delegación

[AuthenticationProvider]

        ↓

[UserDetailsService] → Valida usuario

[PasswordEncoder]   → Valida contraseña
:::


-  ### *PasswordEncoder*

```jsx title="Método"
 @Bean
    public PasswordEncoder passwordEncoder(){
         return new BCryptPasswordEncoder();
    }

Explicación:

Encripta la password

```



## Creación Package controller

### *Clases*  
- PermissionController.

- RoleController.

- UserController.

### *Annotations*
-   **@RestController** : Permite que Spring tome a la clase como controladora.

-   **@PreAuthorize("denyAll()")** : Esta anotación debe realizarse antes de comenzar a desarrollar la clase. Su finalidad es denegar el acceso a todos los endpoint con la finalidad de que nosotros podamos personalizar en cada uno las excpeciones de ingreso.

-   **@RequestMapping("/api/.....")**: Permite agregar una ruta especifica en la URL

```jsx title="Ejemplo"

@RestController
@PreAuthorize(“denyAll()”)
@RequestMapping("/api/permissions")
public class PermissionController {

    // endpoints.

}
```

### *Métodos*
Dentro de cada controller estarán los endpoints correspondientes para:
-   **PermissionController**
    -   Crear / Obtener / Eliminar, etc..

<br/>

-    **RoleController**
        -     Crear / Obtener / Eliminar, etc..

<br/>

-    **UserController**
        -   Crear / Obtener / Eliminar, etc..



<br/>

### *Securizar Endpoints*

Como se mencionó recientemente, con la annotation **@PreAuthorize("denyAll()")** se está denegando el acceso a todos los endpoints. Por tal en cada uno de ellos deberá personalizar los accesos.

Se podrá hacer por medios de Roles o de Roles y Permisos.

:::info[Ejemplo]
**@PreAuthorize("hasAnyRole('ADMIN', 'USER')")** :  Se permite ingresar solo los roles de ADMIN Y USER

**@PreAuthorize("hasRole('ADMIN') and hasPermission('CREATE')")** : Se permite ingresar solo los roles de ADMIN y quienes tengan permisos se creación.
:::

Ejemplo práctico.

```jsx title="Acceso por roles"
@RestController
@PreAuthorize("denyAll()")     // Denegamos acceso a todos los endpoints
@RequestMapping("/api/roles")
public class RoleController {

    @Autowired
    private IRoleService roleService;

    @Autowired
    private IPermissionService permiService;


    //Endpoint para obtener todos los roles
    @GetMapping
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')")  // Solo se permite a quienes poseen roles de "ADMIN" o "USER"
    public ResponseEntity<List<Role>> getAllRoles() {
        List<Role> roles = roleService.findAll();
        return ResponseEntity.ok(roles);
    }


```




```jsx title="Acceso por roles y permisos"
@RestController
@PreAuthorize("denyAll()")     // Denegamos acceso a todos los endpoints
@RequestMapping("/api/roles")
public class RoleController {

    @Autowired
    private IRoleService roleService;

    @Autowired
    private IPermissionService permiService;


    @PostMapping
    @PreAuthorize("hasRole('ADMIN') and hasPermission('CREATE')") // // Solo se permite a quienes poseen roles de "ADMIN" o permisos de CREACIÓN.
    public ResponseEntity<Role> createRole(@RequestBody Role role) {

        //Lógica
    

    }


```



<br/><br/>

- #### Creación de las clases



### *PermissionController*

```jsx title="clase"
@RestController
@RequestMapping("/api/permissions")
public class PermissionController {

    @Autowired
    private IPermissionService permissionService;


    //Endpoint para obtener todos los permisos
    @GetMapping
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<List<Permission>> getAllPermissions() {
        List<Permission> permissions = permissionService.findAll();
        return ResponseEntity.ok(permissions);
    }


    //Endpoint para obtener permisos por ID.
    @GetMapping("/{id}")
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<Permission> getPermissionById(@PathVariable Long id) {
        Optional<Permission> permission = permissionService.findById(id);
        return permission.map(ResponseEntity::ok).orElseGet(() -> ResponseEntity.notFound().build());
    }


    // Endpoint para crear permisos.
    @PostMapping
    @PreAuthorize("hasRole('ADMIN')") // Se permite ingresar solo a los roles de ADMIN
    public ResponseEntity<Permission> createPermission(@RequestBody Permission permission) {
        Permission newPermission = permissionService.save(permission);
        return ResponseEntity.ok(newPermission);
    }
}


```

### *RoleController*


```jsx title="clase"
@RestController
@RequestMapping("/api/roles")
public class RoleController {

        @Autowired
    private IRoleService roleService;

    @Autowired
    private IPermissionService permiService;


    //Endpoint para obtener todos los roles
    @GetMapping
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<List<Role>> getAllRoles() {
        List<Role> roles = roleService.findAll();
        return ResponseEntity.ok(roles);
    }

    //Endpoint para obtener roles por ID.
    @GetMapping("/{id}")
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<Role> getRoleById(@PathVariable Long id) {
        Optional<Role> role = roleService.findById(id);
        return role.map(ResponseEntity::ok).orElseGet(() -> ResponseEntity.notFound().build());
    }


    //Endpoint para crear roles
    @PostMapping
    @PreAuthorize("hasRole('ADMIN') and hasPermission('CREATE')") // Se permite ingresar solo los roles de ADMIN y quienes tengan permisos se creación.
    public ResponseEntity<Role> createRole(@RequestBody Role role) {

        //Declaro de  objetos y listas.
        Set<Permission> permiList = new HashSet<Permission>();
        Permission readPermission;

        // Recuperar la Permission/s por su ID
        for (Permission per : role.getPermissionsList()) {
            readPermission = permiService.findById(per.getId()).orElse(null);
            if (readPermission != null) {
                //si encuentro, guardo en la lista
                permiList.add(readPermission);
            }
        }

        //Asingo al objeto recibido en la request el permiso COMPLETO, ya que solo viene el ID.
        role.setPermissionsList(permiList);

        //Creo nueva variable para responder en el Entity
        Role newRole = roleService.save(role);
        return ResponseEntity.ok(newRole);
    }

    //Agregamos end-point de UPDATE
    @PatchMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')") // Se permite ingresar solo los roles de ADMIN
    public ResponseEntity<Role> updateRole(@PathVariable Long id, @RequestBody Role role) {

        Role rol = roleService.findById(id).orElse(null);
        if (rol!=null) {
            rol = role;
        }

        roleService.update(rol);
        return ResponseEntity.ok(rol);

    }

}


```

### *UserController*
    

```jsx title="Ejemplo"
@RestController
@RequestMapping("/api/users")
public class UserController {

        @Autowired
    private IUserService userService;

    @Autowired
    private IRoleService roleService;


    //Endpoint para Obtener  todos los usuarios.
    @GetMapping
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<List<UserSec>> getAllUsers() {
        List<UserSec> users = userService.findAll();
        return ResponseEntity.ok(users);
    }


    //Endpoint para Obtener usuario por ID.
    @GetMapping("/{id}")
    @PreAuthorize("hasAnyRole('ADMIN', 'USER')") // Se permite ingresar solo los roles de ADMIN Y USER
    public ResponseEntity<UserSec> getUserById(@PathVariable Long id) {
        Optional<UserSec> user = userService.findById(id);
        return user.map(ResponseEntity::ok).orElseGet(() -> ResponseEntity.notFound().build());
    }



    // Endpoint para crear usuarios
    @PostMapping
    @PreAuthorize("hasRole('ADMIN')") // Se permite ingresar solo los roles de ADMIN.
    public ResponseEntity<UserSec> createUser(@RequestBody UserSec userSec) {

        //Declaración de objetos y listas
        Set<Role> roleList = new HashSet<Role>();
        Role readRole;

        //encriptado contraseña
        userSec.setPassword(userService.encriptPassword(userSec.getPassword()));

        // Recuperar los Roles por su ID
        for (Role role : userSec.getRolesList()){
            readRole = roleService.findById(role.getId()).orElse(null);
            if (readRole != null) {

                //si se encuentra, se guarda en la lista
                roleList.add(readRole);
            }
        }

        //Si la lista no está vacía guardo los roles en el objeto que vino por parámetro.
        if (!roleList.isEmpty()) {
            userSec.setRolesList(roleList);

            // Generamos una nueva isntancia de UserSec para poder enviar ese objeto en el ResponseEntity
            UserSec newUser = userService.save(userSec);
            return ResponseEntity.ok(newUser);
        }
        return null;
    }


}


```






## - **Implementación - JWT (Tokens)**
Los token se componen de un header, payload y signature.
Debemos generar una clave privada para firmar los token, eso lo hacemos ingresando a la siguiente página para generalo : 
- https://tools.keycdn.com/sha256-online-generator

### *Application Properties.*
Agregamos la clave privada y el usuario generador del TOKEN.

La clave privada se utilizará para firmar los token asegurando su autenticidad. El userGeneration identifica quien genera los tokens. Si no ocurre nada extraño todos los token deberían ser generados por el usuario que configuremos.


```jsx title="Configuraciones de JWT"
#Config de JWT
security.jwt.private.key=${PRIVATE_KEY}
#Usuario generador del Token
security.jwt.user.generator=${USER_GENERATOR}

```

## Creación Package util


La clase JwtUtils se utiliza para gestionar tokens JWT en una aplicación Spring Boot. Contiene métodos para crear, validar y extraer información de los tokens.


### *Clases*
- Class JwtUtil.


### *Annotarions*

-    @Component : Permite identificar que es una clase Componente

### *Atributos de la clase*
- **privatekey:** Es la clave secreta utilizada para firmar y validar los tokens. Se inyecta desde las configuraciones de la aplicación (application.properties).
- **userGenerator:** Es el identificador de la entidad que genera los tokens

```jsx title="JwtUtils"
@Component
public class JwtUtils {

    //Con estas configuraciones aseguramos la autenticidad del token a crear
    @Value("${security.jwt.private.key}")
    private String privateKey;

    @Value("${security.jwt.user.generator}")
    private String userGenerator;

    #Metodos.....

}
```



### *Métodos*

- **createToken :** Genera un JWT firmado con la clave secreta. Contiene información de Usuario, Permisos, Emisor, Expiración e ID. 
- **validateToken :** (decodificar y validar nuestro token)
- **extractUsername:** (obtener el usuario del token)
- **getSpecificClaim:** (Claim es la info dentro del token, ej Permisos)
- **returnAllClaims:** (Devuelve un mapa con todos los claims almacenados en el token)


```jsx title="clase"
@Component
public class JwtUtils {

    //Con estas configuraciones aseguramos la autenticidad del token a crear
    @Value("${security.jwt.private.key}")
    private String privateKey;

    @Value("${security.jwt.user.generator}")
    private String userGenerator;

    //Para encriptar, vamos a necesitar esta clave secreta y este algoritmo
    public String createToken (Authentication authentication) {
        Algorithm algorithm = Algorithm.HMAC256(this.privateKey);

        //esto está dentro del security context holder
        String username = authentication.getPrincipal().toString();

        //también obtenemos los permisos/autorizaciones
        //la idea es traer los permisos separados por coma
        String authorities = authentication.getAuthorities()
                .stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.joining(","));

        //a partir de esto generamos el token
        String jwtToken = JWT.create()
                .withIssuer(this.userGenerator) //acá va el usuario que genera el token
                .withSubject(username) // a quien se le genera el token
                .withClaim("authorities", authorities) //claims son los datos contraidos en el JWT
                .withIssuedAt(new Date()) //fecha de generación del token
                .withExpiresAt(new Date(System.currentTimeMillis() + 1800000)) //fecha de expiración, tiempo en milisegundos
                .withJWTId(UUID.randomUUID().toString()) //id al token - que genere una random
                .withNotBefore(new Date (System.currentTimeMillis())) //desde cuando es válido (desde ahora en este caso)
                .sign(algorithm); //nuestra firma es la que creamos con la clave secreta

        return jwtToken;
    }

    //método para decodificar
    public DecodedJWT validateToken(String token) {

        try {
            Algorithm algorithm = Algorithm.HMAC256(this.privateKey); //algoritmo + clave privada
            JWTVerifier verifier = JWT.require(algorithm)
                    .withIssuer(this.userGenerator)
                    .build(); //usa patrón builder

            //si está todo ok, no genera excepción y hace el return
            DecodedJWT decodedJWT = verifier.verify(token);
             return decodedJWT;
        }
        catch (JWTVerificationException exception) {
            throw new JWTVerificationException("Invalid token. Not authorized");
        }
    }

    public String extractUsername (DecodedJWT decodedJWT) {
        //el subject es el usuario según establecimos al crear el token
        return decodedJWT.getSubject().toString();
    }

    //devuelvo un claim en particular
    public Claim getSpecificClaim (DecodedJWT decodedJWT, String claimName) {
        return decodedJWT.getClaim(claimName);
    }

    //devuelvo todos los claims
    public Map<String, Claim> returnAllClaims (DecodedJWT decodedJWT){
        return decodedJWT.getClaims();
    }
}


```



### JWT Token Validator (Package Security.config)

####  Se crea un subpackage "filter"

### *Clases*
- JwtTokenValidator (hereda OncePerRequestFilter.)

:::info
Al heredar de esa clase significa que siempre que haya una request, se va a aplicar este filtro.
:::

### *Atributos*
- jwtUtils

### *Métodos*
- Constructor
- doFilterInternal (Filtro interno)


```jsx title="JwtTokenValidator"
public class JwtTokenValidator extends OncePerRequestFilter {

    private JwtUtils jwtUtils;


    // Constructor.
    public JwtTokenValidator(JwtUtils jwtUtils) {

        this.jwtUtils = jwtUtils;
    }

    // Método
    @Override
    //importante: el nonnull debe ser de sringframework, no lombok
    protected void doFilterInternal(@NonNull HttpServletRequest request,
                                    @NonNull HttpServletResponse response,
                                    @NonNull FilterChain filterChain) throws ServletException, IOException{

    #Desarrollo del método doFilterInternal....
    
    }

}
```

Método "doFilterInternal"
- Delante de cada parámetro se debe colocar la annotation @NonNull de Spring Security, NO de Lombok
- Recibe como parámetros:
    - HTTPServletRequest
    - HTTPServletResponse
    - FilterChain
- Se extrae el token de la cabecera
- Delante del Token viene la palabra “bearer” entonces se deben quitar esos caracteres para que quede el token "limpio".
-   Se valida el token y se almacena en una variable decodedJWT
-   Se extrae el nombre de usuario a partir del token decodificado
-   Se extrae los permisos y roles a partir del claim
-   Se convierten los datos en GrandAuthorities para almacernaos en el context Holder
-   Si todo es correcto a la cadena de filtro se la pasa la request y la response.


```jsx title="Desarrollo del método"
    protected void doFilterInternal(@NonNull HttpServletRequest request,
                                    @NonNull HttpServletResponse response,
                                    @NonNull FilterChain filterChain) throws ServletException, IOException {

            String jwtToken = request.getHeader(HttpHeaders.AUTHORIZATION);

            if(jwtToken != null) {
                //en el encabezado antes del token viene la palabra bearer (esquema de autenticación)
                //por lo que debemos sacarlo
                 jwtToken = jwtToken.substring(7); //son 7 letras + 1 espacio
               DecodedJWT decodedJWT = jwtUtils.validateToken(jwtToken);

               //si el token es válido, le concedemos el acceso
                String username = jwtUtils.extractUsername(decodedJWT);
                //me devuelve claim, necesito pasarlo a String
                String authorities = jwtUtils.getSpecificClaim(decodedJWT, "authorities").asString();

                //Si todo está ok, hay que setearlo en el Context Holder
                //Para eso tengo que convertirlos a GrantedAuthority
                Collection<? extends GrantedAuthority> authoritiesList = AuthorityUtils.commaSeparatedStringToAuthorityList(authorities);

                //Si se valida el token, le damos acceso al usuario en el context holder
                SecurityContext context = SecurityContextHolder.getContext();
                Authentication authentication = new UsernamePasswordAuthenticationToken(username, null, authoritiesList);
                context.setAuthentication(authentication);
                SecurityContextHolder.setContext(context);

            }

            // Si hay algun error arroja una exception
            filterChain.doFilter(request,response);
    }
```


## SecurityFilterChain (Package SecurityConfig)

Se agrega a la cadena de filtros (SecurityFilterChain) el nuevo filtro de JwtTokenValidator.

```jsx title="método SecurityFilterChain"
  @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(csrf -> csrf.disable())
                .formLogin(Customizer.withDefaults())
                .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
Se agrega --->  .addFilterBefore(new JwtTokenValidator(jwtUtils), BasicAuthenticationFilter.class)   
            .build();
    }
```



## Controller de Autenticación (Package Controller)

Crearemos un controller que se encargue de autenticar. Utilizaremos una clase DTO para transferir datos. Este DTO lo crearemos en el siguiente apartado.

- Se crea la clase “AuthenticationController”. Esta clase recibirá las credenciales y validarla.
- Se inyecta la dependencia del UserDetailServiceImp



### *Endpoints*
- Login
    - Recibe AuthLoginRequestDTO en el cuerpo de la solicitud
    - Se llama al userDetailService para validar
    - Se responde con AuthResponseDTO

El método login en el controlador AuthenticationController gestiona las solicitudes de autenticación de usuarios. Recibe un objeto JSON con las credenciales del usuario (nombre de usuario y contraseña) en el cuerpo de la solicitud, deserializado en un AuthLoginRequestDTO. Luego, llama al método loginUser del servicio UserDetailsServiceImp para verificar las credenciales y generar un token JWT en caso de ser válidas. Finalmente, devuelve una respuesta con un objeto AuthResponseDTO, que incluye el token generado y, posiblemente, información adicional del usuario, junto con un estado HTTP 200 (OK) para indicar que la solicitud fue procesada correctamente.

```jsx title="“AuthenticationController”"
@RestController
@RequestMapping("/auth")
public class AuthenticationController {

    @Autowired
    private UserDetailsServiceImp userDetailsService;

    @PostMapping("/login")
    public ResponseEntity<AuthResponseDTO> login(@RequestBody @Valid AuthLoginRequestDTO userRequest) {
        return new ResponseEntity<>(this.userDetailsService.loginUser(userRequest), HttpStatus.OK);
    }

}

```



## Creación DTO

Estas clases seran del tipo **record**. Esto permite identificarla como DTO, ya que no será necesario los get, set y constructores.

### *Clases*

**- AuthLoginRequestDTO**
    - username
    - password

```jsx title="AuthLoginRequestDTO"
public record AuthLoginRequestDTO (@NotBlank String username, 
                                   @NotBlank String password) {
}
```


**- AuthResponseDTO**
    - Username
    - Message
    - Jwt
    - status

```jsx title="AuthResponseDTO"
@JsonPropertyOrder({"username", "message", "jwt", "status"}) // Establece el orden de los atributos en el armado del JSON (NO SIEMPRE ES NECESARIO)
public record AuthResponseDTO (String username, 
                               String message, 
                               String jwt, 
                               boolean status) {
}
```



## Login User (UserDetailsServiceImp)

- loginUser
    - Se recupera usuario y contraseña.
    - Se autentica, por medio del método authenticate.
    - Se almacena en el contextHolder
    - Se crea el token.
    - Se genera la responseDTO


```jsx title="loginUser"  
    public AuthResponseDTO loginUser (AuthLoginRequestDTO authLoginRequest){

        //Se recupera nombre de usuario y contraseña
        String username = authLoginRequest.username();
        String password = authLoginRequest.password();


        // Se llama al método authenticate.
        Authentication authentication = this.authenticate (username, password);

        //si es autenticado correctamente se almacena la información SecurityContextHolder y se crea el token.
        SecurityContextHolder.getContext().setAuthentication(authentication);
        String accessToken =jwtUtils.createToken(authentication);
        AuthResponseDTO authResponseDTO = new AuthResponseDTO(username, "login ok", accessToken, true);
        return authResponseDTO;

    }
```


## authenticate (UserDetailsServiceImp)

- authenticate
    - Recibe como parámetros usuario y contraseña.
    - Recuperamos el usuario y contraseña por medio del método loadUserByUsername.
    - Retornamos la autenticación.

```jsx title="authenticate"  
    public Authentication authenticate (String username, String password) {
        //Recupero información del usuario por el username
        UserDetails userDetails = this.loadUserByUsername(username);

        // En caso que sea nulo, se informa que no se pudo encontrar al usuario.
        if (userDetails==null) {
            throw new BadCredentialsException("Invalid username or password");
        }
        // En caso que no coincidan las credenciales se informa que la password es incorrecta
        if (!passwordEncoder.matches(password, userDetails.getPassword())) {
            throw new BadCredentialsException("Invalid username or password");
        }
        return new UsernamePasswordAuthenticationToken(username, userDetails.getPassword(), userDetails.getAuthorities());
    }
```





## **Implementación - OAtuh2**

## SecurityFilterChain (Package SecurityConfig)

### *Método*
- Se agrega a la cadena de filtro:  .oauth2Login(Customizer.withDefaults())

```jsx title="Ejemplo"
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity
                .csrf(csrf -> csrf.disable())
                .formLogin(Customizer.withDefaults())
                .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                .addFilterBefore(new JwtTokenValidator(jwtUtils), BasicAuthenticationFilter.class)
 Se agrega ---> .oauth2Login(Customizer.withDefaults()) 
            .build();
    }
```


## Securización de los endpoints(Package Controller)

Se debe agregar el @PreAuthorize("isAuthenticated() and hasRole('ADMIN')")

:::tip
**@PreAuthorize("isAuthenticated() and hasRole('ADMIN')")** :
Permite acceso SOLO a quienes están autenticados y posean roles ADMIN. 

:::

```jsx title="Ejemplo"
  @GetMapping("/holaseg")
    @PreAuthorize("isAuthenticated() and hasRole('ADMIN')")
    public String secHelloWorld() {

        return "Hola Mundo TodoCode con seguridad";
    }
```


## Configuración a Google como proveedor de Autenticación

### *ApplicationProperties*
Se agrega las siguientes variable de entorno :

```jsx title="Variable Entorno OAuth2"
#Configuración para Google
spring.security.oauth2.client.registration.google.client-id=${GOOGLE_CLIENT_ID}
spring.security.oauth2.client.registration.google.client-secret=${GOOGLE_CLIENT_SECRET}
```


### *Consola Google*

Se ingresa a la siguiente URL:
- https://console.cloud.google.com/welcome?project=springsecurity0auth2


#### Creción proyecto
![google uno](/img/google1.png)

![google dos](/img/google2.png)

![google tres](/img/google3.png)


#### Creción Credenciales
![google cuatro](/img/google4.png)


#### Configurar pantalla de consentimiento
![google cinco](/img/google5.png)

![google seis](/img/google6.png)

![google siete](/img/google7.png)


Guardamos y continuamos en las siguientes dos pantallas


#### Continuamos con la creción Credenciales
![google ocho](/img/google8.png)

![google nueve](/img/google9.png)


:::important
Obtendremos las credenciales para colocar en las variables de entorno del applicationProperties.
:::

![google diez](/img/google10.png)


## **Implementación envío de emails para recuperar contraseña**

#### Flujo del proceso.

1. El usuario hace clic en "Restablecer Contraseña":

    -   El usuario navega a la página de restablecimiento de contraseña y proporciona su email.


2. El endpoint **/auth/request-reset-password** es llamado:

    -   Se envía una solicitud POST a http://localhost:8080/auth/request-reset-password con el parámetro email.


3.  El controlador **AuthenticationController** maneja la solicitud:

4.  El servicio **UserService** genera un token y envía un email:

    -   createPasswordResetTokenForUser(email) en UserService genera un token y lo guarda en la base de datos.

    -   Envía un email al usuario con el enlace de restablecimiento de contraseña que incluye el token.


5.  El usuario hace clic en el enlace del email:

    -   El enlace lleva al usuario a una página para ingresar una nueva contraseña y envía una solicitud POST a **/auth/reset-password** con el token y la nueva contraseña.

6. Se llama al endpoint **/auth/reset-password**

    -   Se envía una solicitud POST a http://localhost:8080/auth/reset-password con los parámetros token y newPassword.


7. El controlador **AuthenticationController** maneja la solicitud.

8. El servicio UserService valida el token y actualiza la contraseña:

    -   **validatePasswordResetToken(token)** valida el token.

    -  **updatePassword(token, newPassword)** actualiza la contraseña y borra el token de la base de datos.





----------------


### *Incorporar dependencia*

```jsx title=""
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>

<dependency>
	<groupId>com.sun.mail</groupId>
	<artifactId>jakarta.mail</artifactId>
	<version>2.0.1</version>
</dependency>

```


### *Configurar Application.properties*

```jsx title=""
# Configuración básica del servidor de correo
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=${EMAIL_USERNAME} --> email.
spring.mail.password=${EMAIL_PASSWORD} --> Se obtiene en el paso siguiente.

# Propiedades adicionales para autenticación y cifrado
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.connectiontimeout=5000
spring.mail.properties.mail.smtp.timeout=5000
spring.mail.properties.mail.smtp.writetimeout=5000
spring.mail.properties.mail.smtp.ssl.trust=smtp.gmail.com

```

### *Obtener password*

1. Ingresar a la cuenta de Gmail.

2. Ir a la foto --> "Gestionar  tu cuenta de Google"

3. Ir al apartado de seguridad.

4. Activar la verificación en dos pasos.

5. Crer una "Contraseña de aplicación" [Ver a la barra derecha del paso 4]

6. Se generará la password para configurar en el application properties. 


### *Obtener certificado de seguridad*

1. Ingresar a la cuenta de Gmail.

2. En barra de navegación ir al candado y exportar el certificado.

![ssl-1](/img/ssl-1.png)

![ssl-2](/img/ssl-2.png)

![ssl-3](/img/ssl-3.png)

![ssl-4](/img/ssl-4.png)

3. Incorporarlo desde la interfaz gráfica de Intellij

![ssl-5](/img/ssl-5.png)

![ssl-6](/img/ssl-6.png)


### *Modificar entity UserSec*

```jsx title="UserSec"
@Column(length = 500) // Ajustar este valor según sea necesario
private String resetPasswordToken;

```


### *Crear Interfaces y servicio*

```jsx title="IEmailService"
public interface IEmailService {
    void sendEmail(String to, String subject, String body);
}

```

```jsx title="EmailService"
@Service
public class EmailService implements IEmailService {

    @Autowired
    private JavaMailSender  mailSender;

    public void sendEmail(String to, String subject, String body) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(body);
        mailSender.send(message);
    }
}

```

### *Incorporar métodos dentro de UserService*

```jsx title="UserService"
  public boolean validatePasswordResetToken(String token) {
        try {
            DecodedJWT decodedJWT = jwtUtils.validateToken(token);
            String username = jwtUtils.extractUsername(decodedJWT);
            UserSec usuario = userRepository.findByResetPasswordToken(token);
            return usuario != null && usuario.getUsername().equals(username);
        } catch (Exception e) {
            return false;
        }
    }

    public void updatePassword(String token, String newPassword) {
        UserSec usuario = userRepository.findByResetPasswordToken(token);
        String passwordEncrypted = encriptPassword(newPassword);
        if (usuario != null) {
            usuario.setPassword(passwordEncrypted);
            usuario.setResetPasswordToken(null);
            userRepository.save(usuario);
        }
    }

```

### *Agregar endpoints en controller*

```jsx title="AuthenticationController"
@PostMapping("/request-reset-password")
public ResponseEntity<String> requestResetPassword(@RequestParam String email) {
    userService.createPasswordResetTokenForUser(email);
    return ResponseEntity.ok("Solicitud de restablecimiento de contraseña procesada. Por favor, revisa tu correo electrónico.");
}

@PostMapping("/reset-password")
public ResponseEntity<String> resetPassword(@RequestParam String token, @RequestParam String newPassword) {
    boolean isTokenValid = userService.validatePasswordResetToken(token);
    if (!isTokenValid) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body("Token de restablecimiento de contraseña no válido o expirado.");
    }
    userService.updatePassword(token, newPassword);
    return ResponseEntity.ok("Contraseña restablecida exitosamente.");
}
```


