---
sidebar_position: 16
---

# 16 - Exception y Validation

## **Exception**

El manejo de excepciones es una parte crucial de cualquier aplicación, ya que permite gestionar errores de manera eficiente y proporcionar feedback claro al usuario. En el contexto de una aplicación Spring Boot, es recomendable implementar un enfoque centralizado para manejar excepciones.


## **Logs**

Loggear excepciones es fundamental en el desarrollo de software, ya que permite registrar información detallada sobre errores que ocurren en el sistema durante su ejecución. Esta práctica es crucial por varias razones:

1.  **Diagnóstico rápido:** Los logs proporcionan información precisa sobre el tipo de excepción, el mensaje asociado, y el stack trace, facilitando la identificación del problema.

2.  **Resolución eficiente de errores:** Con registros claros, los desarrolladores pueden entender el contexto en el que ocurrió el error y aplicar correcciones más rápidamente.

3.  **Auditoría y seguimiento:** Los logs sirven como un historial de eventos del sistema, ayudando a rastrear problemas recurrentes o intermitentes.

4.  **Prevención de problemas futuros:** Analizar patrones de errores en los logs ayuda a identificar áreas vulnerables del código y prevenir incidentes similares.

5.  **Monitoreo en producción:** En entornos en vivo, los logs son esenciales para detectar y responder rápidamente a fallos antes de que afecten a los usuarios.

En resumen, loggear excepciones no solo ayuda a solucionar problemas, sino que también contribuye a la estabilidad, mantenibilidad y mejora continua del sistema.

### *Configuracion Inicial*

```jsx title="pom.xml"
  <!-- Spring Boot Logger -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </dependency>
```

<br/><br/>

```jsx title="application.properties"
# Configuración del nivel de logs (por ejemplo, ERROR, WARN, INFO)
logging.level.org.springframework.web=ERROR
logging.level.com.tu.paquete=ERROR

# Establecer la ubicación del archivo de logs (si lo deseas)
logging.file.name=logs/app.log

# Configuración de logback (opcional, si necesitas personalizar el formato)
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
```


<br/><br/>


## Tipos de exception


Podremos crear clases de excepción personalizadas para aquellos casos que no estén incluídos por las annotation de validación y debamos realizar la validación manual en nuestro service, o bien manejar las excepciones de forma centralizada en caso que de ocurra alguna no prevista.


### *MethodArgumentNotValidException*

-   Se lanza cuando se valida un objeto completo en el cuerpo de la solicitud **(@RequestBody) usando @Valid.**


-   Esta excepción ocurre porque Spring valida el objeto JSON enviado por el cliente contra las anotaciones de validación del DTO (como @NotNull, @NotBlank, etc.).

**Contexto: Esto aplica únicamente para validaciones de tipo Bean Validation en DTOs.**


### *ConstraintViolationException*

-   Se lanza cuando las validaciones se realizan directamente en los parámetros del método, como en los casos de **@PathVariable** o **@RequestParam**.

-   Acá, Spring no está validando un objeto completo, sino un valor simple (por ejemplo, un número o una cadena). Este tipo de validación es manejado por la API de validación de Jakarta (anteriormente Javax).

**Contexto: Se utiliza en validaciones individuales de parámetros con @Valid o anotaciones como @NotNull.**

<br/><br/>

## Manejo de Exception por annotation validation

Para el manejo de exception por annotarion, deberemos crear un package **exception** y dentro una clase **GlobalExceptionHandler**. Esta clase deberá tener la annotation **@ControllerAdvice** para que sea reconocida por Spring como tal.

### *MethodArgumentNotValidException (@RequestBody)*

1. Creamos la clase DTO donde estarán las validaciones

```jsx title="CursoDTO"
public record CursoDTO(
    @NotBlank(message = "El nombre del curso no puede estar vacío")
    String nombre,

    @NotBlank(message = "La modalidad no puede estar vacía")
    String modalidad,

    @NotNull(message = "La fecha de finalización no puede ser nula")
    String fechaFinalizacion,

    @NotNull(message = "La lista de temas no puede ser nula")
    List<TemaDTO> listaDeTemas
) {
    // Constructor, getters, equals, hashCode y toString son generados automáticamente
}



```

<br/><br/>

2. Creamos el endpoint donde recibimos la petición. Dentro de los parametros utilizamos la annotation **@Valid** para activar el entorno de validaciones.


```jsx title="Controller"
@PostMapping("/curso")
public ResponseEntity<?> crearCurso(@Valid @RequestBody CursoDTO cursoDTO) {

    // Aquí puedes transformar tu DTO a la entidad correspondiente si es necesario
    Curso curso = new Curso(
        cursoDTO.nombre(),
        cursoDTO.modalidad(),
        cursoDTO.fechaFinalizacion()
    );

    // Llamada al servicio para guardar el curso
    Map<String, Object> response = IcursoService.saveCurso(curso);

    return ResponseEntity.ok(response); // Devuelve 200 OK
}
```



<br/><br/>

3. Creamos el package **exception** y dentro la clase **GlobalExceptionHandler** 

4. Definimos el método para capturar la exception y mostrar el mensaje.


```jsx title="GlobalExceptionHandler"
@ControllerAdvice // Anotación para manejar excepciones globalmente en todos los controladores
public class GlobalExceptionHandler {

    // Manejo de excepción por validaciones de anotaciones en los DTOs
    @ExceptionHandler(MethodArgumentNotValidException.class)  // Esta excepción se lanza cuando hay una violación de validación de un objeto (por ejemplo, DTO)
    public ResponseEntity<?> handleValidationExceptions(MethodArgumentNotValidException ex) {
        
        // Crear un mapa para almacenar los errores de validación
        Map<String, String> errors = new HashMap<>();
        
        // Iteramos sobre los errores de cada campo que falló en la validación
        ex.getBindingResult().getFieldErrors().forEach(error ->
                // Guardamos el nombre del campo (error.getField()) y el mensaje de error correspondiente (error.getDefaultMessage()) en el mapa
                errors.put(error.getField(), error.getDefaultMessage())
        );

        // Devolvemos una respuesta con un código de estado HTTP 400 (Bad Request) y el mapa de errores como cuerpo de la respuesta
        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)  // Establecemos el estado HTTP a 400 (petición incorrecta)
                .body(errors);  // Enviamos los errores de validación como cuerpo de la respuesta
    }




    // Método Global para el resto de las exception 
    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleGeneralException(Exception e) {
        // Respuesta para cualquier otra excepción no prevista
        System.err.println("Excepción no controlada: " + e.getMessage());
        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("Manejando InvalidCursoException: " + e.getMessage());
    }



}
```

<br/><br/>

### *ConstraintViolationException (@PathVariable o @RequestParam)*

```jsx title="GlobalExceptionHandler"
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<Map<String, String>> handleConstraintViolation(ConstraintViolationException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getConstraintViolations().forEach(violation -> {
            String fieldName = violation.getPropertyPath().toString();
            String errorMessage = violation.getMessage();
            errors.put(fieldName, errorMessage);
        });
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }

}


```
<br/><br/>


## Manejo de Exception personalizadas y generales

###  *Configuraciones iniciales*

1. Crearemos el package "exception".
2. Creamos las clase de tipo **exception** según necesitemos para personalizar las mismas.

A continuación se verán ejemplos de exception personalizadas que usaremos en el próximo apartado **@Service**, donde estableceremeos los mensajes personalizados.



```jsx title="CursoNotFoundException"
public class CursoNotFoundException extends RuntimeException {
    public CursoNotFoundException(String message) {

        super(message);
    }
}
```

```jsx title="InvalidCursoException"
public class InvalidCursoException extends RuntimeException {
    public InvalidCursoException(String message) {

        super(message);
    }
}
```

```jsx title="TemaException"
public class TemaException extends RuntimeException {
    public TemaException(String message) {

        super(message);
    }
}
```
<br/><br/>

3. Crearemos una clase común *GlobalExceptionHandler* para manejar de manera global todas las exception.
:::info[Importante]
Para que Spring boot identifique esta interfaz como tal, debemos colocar la annotation @ControllerAdvice
:::



```jsx title="GlobalExceptionHandler"
@ControllerAdvice
public class GlobalExceptionHandler {

    // Manejo de excepción personalizada para Curso no encontrado
    @ExceptionHandler(CursoNotFoundException.class)
    public ResponseEntity<?> handleCursoNotFoundException(CursoNotFoundException e) {
        // Respuesta personalizada para CursoNotFoundException
        return ResponseEntity
                .status(HttpStatus.NOT_FOUND)
                .body(e.getMessage());
    }





    // Manejo de excepción personalizada para Curso inválido
    @ExceptionHandler(InvalidCursoException.class)
    public ResponseEntity<?> handleInvalidCursoException(InvalidCursoException e) {
        // Respuesta personalizada para InvalidCursoException
        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(e.getMessage());
    }






    // Manejo de excepción personalizada para errores de Tema
    @ExceptionHandler(TemaException.class)
    public ResponseEntity<?> handleTemaException(TemaException e) {
        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(e.getMessage());
    }






    //Manejo de exception por Annotation
     @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
                errors.put(error.getField(), error.getDefaultMessage())
        );

        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(errors);
    }





    // Método Global para el resto de las exception 
    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleGeneralException(Exception e) {
        // Respuesta para cualquier otra excepción no prevista
        System.err.println("Excepción no controlada: " + e.getMessage());
        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("Manejando InvalidCursoException: " + e.getMessage());
    }



}
```



## Manejo de Exception Base de datos

### *Creación Excpetion personalizada*

Este exception nos permitirá detallar:
    - **Mensaje al usuario**
        -   Con información que se decida mostrar del objeto.
    
    - **Archivo Log**
        -   ID del objeto.
        -   Entidad.
        -   Nombre.
        -   Operación de persistencia.
        -   Causa Raíz del error.


```jsx title="DataBaseException"
@Getter
public class DataBaseException extends RuntimeException {
  private final Long entityId;      // ID de la entidad (Curso, Tema, etc.)
  private final String entityType;  // Tipo de la entidad (Curso, Tema, etc.)
  private final String entityName;  // Nombre de la entidad (por ejemplo, "Curso de Java")
  private final String Operation; // Operación (Ejemplo: Save, update)
  private final String rootCause;   // Causa raíz del error (mensaje de excepción original)

  // Constructor con detalles de la entidad y causa raíz
  public DataBaseException(String message, String entityType, Long entityId, String entityName, String operation, String rootCause) {
    super(message);
    this.entityId = entityId;
    this.entityType = entityType;
    this.entityName = entityName;
    this.Operation = operation;
    this.rootCause = rootCause;
  }

}
```

<br/><br/>

### *Capturar exception en el service*

Envolvemos la operación principal en el bloque try - catch

```jsx title="CursoService"
    @Override
    public Response<CursoDto> saveCurso(Curso curso) {
        try{
            // Validaciones previas.
            validateNameNotExist(curso.getNombre());
            validateModality(curso.getModalidad());

            // Guarda el curso
            Curso cursoAux = cursoRepository.save(curso);

            // Construye el DTO para devolver
            CursoDto cursoDto = buildCursoDtoCreate(cursoAux);

            return new Response<>(true, "Se ha guardado correctamente",cursoDto);
        }catch (Exception ex) {
            // Si ocurre un error, se lanza la DataBaseException con la información necesaria

            //Se guarda el motivo de la causa raíz
            String rootCause = ex.getCause() != null ? ex.getCause().toString() : "Desconocida";

            // Concatenar el nombre del curso para el mensaje al usuario
            String userMessage = "Error al guardar el curso '" + curso.getNombre() + "'. Por favor, inténtelo nuevamente.";

            throw new DataBaseException(
                    userMessage,       // Mensaje al usuario
                    "Curso",           // Tipo de entidad
                    curso.getId(),     // ID del curso
                    curso.getNombre(), // Nombre del curso
                    "Save",            // Operation
                    rootCause);        // Causa raíz del error
        }
    }
```


### *Manejador Global*

Incorporamos el método en el manejador global.

Desde aquí haremos: 

    - Registrar los logs en nuestro archivo para analisis.

    - Enviar el mensaje al usuario.


1.  Agregamos la annotation **@Slf4j**


```jsx title="GlobalExceptionHandler"
@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {

}

```

2. Incorparamos un método que gestione la exception.

```jsx title="GlobalExceptionHandler"
    @ExceptionHandler(DataBaseException.class)
    public ResponseEntity<Response<String>> handleDataBaseException(DataBaseException ex) {
        // Loguear el error para diagnóstico
        log.error("Error al acceder a la base de datos: [ENTIDAD: {}] - [ID {}] -  [NOMBRE:{}] - [OPERACIÓN:{}] -  [CAUSA RAÍZ: {}] - [MENSAJE USUARIO: {}]",
                ex.getEntityType(),  ex.getEntityId(), ex.getEntityName(), ex.getOperation(), ex.getRootCause(), ex.getMessage());

        // Mensaje para el usuario final
        Response<String> response = new Response<>(false, ex.getMessage(), null);
        return new ResponseEntity<>(response, HttpStatus.INTERNAL_SERVER_ERROR);
    }

```


<br/><br/>

### *Códigos de estado*

**400 Bad Request:**: Solicitud mal formada (datos faltantes o inválidos).

Ejemplo: Faltan campos requeridos en el cuerpo de la solicitud.

<br/>

**401 Unauthorized**: Se usa cuando el usuario no está autenticado.

Ejemplo: Intentar acceder a un curso sin un token válido.

 <br/>

**403 Forbidden**: Se usa cuando el usuario no tiene permisos suficientes.

Ejemplo: Un usuario sin rol de administrador intenta borrar un curso.

<br/>

**404 Not Found**: Se usa cuando un recurso no existe.

Ejemplo: Solicitar un curso con un ID inexistente.

<br/>

**409 Conflict**: Indica un conflicto, como intentar crear un recurso duplicado.

Ejemplo: Crear un curso con un ID que ya existe.

<br/>

**503 Service Unavailable:** Se usa cuando el servidor no está disponible.

Ejemplo, durante mantenimiento.




<br/><br/>

## **Validation**

La validación en Spring se refiere al proceso de verificar que los datos proporcionados por un usuario o sistema cumplan con ciertos criterios antes de ser procesados o almacenados en una base de datos. Spring ofrece una forma poderosa de realizar esta validación usando anotaciones (annotations) junto con la API de validación de Java, JSR-303/JSR-380 (bean validation), que define un conjunto de reglas estándar para la validación de objetos Java.


### *¿Cómo se implementa la validación en Spring?*

En Spring, la validación se puede realizar utilizando la anotación @Valid(generalmente en los parámetros del controller) junto con las anotaciones de validación proporcionadas por JSR-303/JSR-380. Estas anotaciones se colocan en las propiedades de las clases o en los parámetros de los métodos para asegurarse de que los datos sean válidos antes de ser procesados.


Estas son algunas de las anotaciones estándar de validación que ofrece Bean Validation:

**@NotNull:** Asegura que el valor no sea null.

**@NotEmpty:** Verifica que una colección o cadena de texto no esté vacía. 

**@NotBlank:** Verifica que una cadena no esté vacía y no sea solo espacios.

**@Size(min = X, max = Y):** Valida que la longitud de una cadena o colección esté dentro de un rango.

**@Email:** Asegura que el valor sea una dirección de correo electrónico válida.

**@Min y @Max:** Valida que un número esté dentro de un rango específico.

**@Pattern(regexp = "regex"):** Valida que una cadena cumpla con una expresión regular.


### *Validación personalizada con anotaciones*

En ocasiones, las validaciones estándar no son suficientes para cubrir todas las necesidades de la aplicación. Es posible crear validaciones personalizadas con anotaciones propias. Esto se hace de la siguiente manera:

1. Tener implementada la clase **GlobalExceptionHandler** con el método **MethodArgumentNotValidException** para capturar las excepciones.



```jsx title="GlobalExceptionHandler"
@ControllerAdvice
public class GlobalExceptionHandler {

    //Manejo de exception por Annotation
     @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
                errors.put(error.getField(), error.getDefaultMessage())
        );

        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(errors);
    }





    // Método Global para el resto de las exception 
    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleGeneralException(Exception e) {
        // Respuesta para cualquier otra excepción no prevista
        System.err.println("Excepción no controlada: " + e.getMessage());
        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("Manejando InvalidCursoException: " + e.getMessage());
    }



}
```

2. Creamos un package **annotation**

    - Creamos una interfaz con el nombre de nuestra annotation seguido de la entidad a la que corresponde.

    - Agregamos annotation de configuraciones.
        -   Dentro de la annotation **Constraint** enlazamos nuestra interfaz con nuestra futura clase donde estará la logica. 
        -   El nombre de esa clase deberá ser el mismo que la interfaz pero al final debera tener Validator.

    - Colocamos el @ delante de la palabra interface. 
 


Supongamos que queremos validar que al crear un Curso, el ID no exista en la base de datos.

- **Nombre de la interfaz:** IdNotExistCurso


```jsx title="IdNotExistCurso"
@Documented
@Constraint(validatedBy = IdNotExistCursoValidator.class) -- > Reemplazarlo por el nombre de la clase con la lógica de validación. 
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.METHOD}) // Puede usarse en atributo y Método

public @interface IdNotExistCurso {


}
```


Ahora debemos agregar 3 atributos para que funcione la interfaz. El primero de ellos es que nos va a importar ya que ahí vamos a poder configurar un mensaje por default en caso que la validación no pasé.
Este mensaje estará enlazado a un archivo de configuración que haremos próximamente.


```jsx title="IdNotExistCurso"
@Documented
@Constraint(validatedBy = IdNotExistCursoValidator.class) -- > Reemplazarlo por el nombre de la clase con la lógica de validación.
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.METHOD}) // Puede usarse en atributo y Método

public @interface IdNotExistCurso {
   String message() default "{IdNotExistValidator.message}"; // Esta es la referencia al mensaje

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

}
```

Dentro del package **resources**
    -   Iremos al application.properties y agregaremos lo siguiente :

```jsx title="application.properties"
    #Mensajes Validación
    Spring.messages.basename=messages
```

- Creamos un nuevo archivo, desde la carpeta **resources** llamado **messages.properties** y configuraremos el mensaje para la annotation.

```jsx title="*messages.properties"
IdNotExistValidator.message= El ID ya existe en la base de datos.

```

3. Creamos un package **validator**

    - Creamos una clase con la palabra **Validator al final**
    - Esta clase debe implementar la interfaz **ConstraintValidator< nombre Interfaz, TipoDato del atributo a validar >**
    - Implementamos los métodos que nos sugiere.

```jsx title="IdNotExistCursoValidator"
public class IdNotExistCursoValidator implements ConstraintValidator<IdNotExistCurso, Long> {

 
    @Override
    public void initialize(IdNotExistCurso constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);
    }

    @Override
    public boolean isValid(Long value, ConstraintValidatorContext constraintValidatorContext) {

    }
}

```


4. Implementamos la lógica de validación dentro del método **isValid**

Este método deberá retorna true, si todas las reglas de validación pasan correctamente. Caso contrario deberá retornar un false.

-   Agregamos la inyección de dependencias al repositorio.
-   Realizamos todas las validaciones necesarias.

```jsx title="IdNotExistCursoValidator"
public class IdNotExistCursoValidator implements ConstraintValidator<IdNotExistCurso, Long> {

    @Autowired
    CursoRepository cursoRepository;

    @Override
    public void initialize(IdNotExistCurso constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);
    }

    @Override
    public boolean isValid(Long value, ConstraintValidatorContext constraintValidatorContext) {

        // Si es nulo retorna false
        if(value == null){
            return false;
        }

        // Retornadá false si existe en base de datos o true si no existe.
       return !cursoRepository.existsById(value);
    }
}

```

5. Finalmente podemos agregar nuestra annotation dentro del atributo de DTO con un mensaje personalizado (diferente al por defecto)


```jsx title="CursoDto"
public record CursoDto(


        @IdNotExistCurso(message = "Ya existe un ID para ese curso - DTO")
        long id_curso,

){

}
```

<br/><br/>

## Conclusión

**Validación estándar (@NotNull, @Min, @Max):** Perfecta para validaciones simples y sin acceso a la base de datos.

**Validación personalizada:** Útil cuando tienes una regla compleja que no se puede resolver con las validaciones estándar (como verificar unicidad o lógica específica del negocio).

**Validaciones con consulta a la base de datos:** La validación que depende de la base de datos (como verificar unicidad de un campo) debe hacerse en la capa de servicio con métodos privados y no en las anotaciones del DTO.