---
sidebar_position: 19
---

# 19 - Spring Boot Admin

Spring Boot Admin es una herramienta de código abierto que facilita la administración y monitorización de aplicaciones Spring Boot. Fue diseñada para ofrecer una interfaz visual centralizada que permite a los desarrolladores y equipos de operaciones gestionar múltiples instancias de aplicaciones basadas en Spring Boot en tiempo real.

Con Spring Boot Admin, se puede monitorear aspectos como el estado de las aplicaciones, métricas de rendimiento, logs, e información general sobre la configuración de cada instancia. Es particularmente útil en entornos de microservicios donde hay múltiples aplicaciones distribuidas que necesitan ser gestionadas y supervisadas de manera eficiente.


## Características principales de Spring Boot Admin

-   **Estado en tiempo real:** Proporciona información sobre si las aplicaciones están up (en ejecución) o down (caídas).

-   **Métricas y salud:** Muestra métricas como uso de CPU, memoria, y datos de salud como conectividad a bases de datos y servicios externos.

-   **Logs en vivo:** Permite acceder a los logs generados por las aplicaciones, útiles para diagnóstico y depuración.

-   **Notificaciones:** Ofrece integración con sistemas de notificación como Slack, Microsoft Teams, y correos electrónicos para alertar sobre cambios en el estado de las aplicaciones.

-   **Gestión de endpoints:** Expone los endpoints de Actuator (como /health, /metrics, etc.) para su consulta desde una interfaz amigable.

-   **Seguridad:** Soporte para autenticación y autorización, asegurando que solo usuarios autorizados puedan acceder a la consola de administración.

-   **Escalabilidad:** Puede manejar múltiples instancias de aplicaciones, lo que es ideal para arquitecturas distribuidas.


## Cómo funciona Spring Boot Admin

**Cliente (Aplicaciones monitoreadas)**

Las aplicaciones Spring Boot que se desean monitorear necesitan incluir el cliente de Spring Boot Admin y configurar un registro con el servidor de Spring Boot Admin.
Esto se logra mediante Actuator y una configuración sencilla en el archivo application.properties o application.yml.


**Servidor (Consola de administración)**
El servidor de Spring Boot Admin centraliza la monitorización y muestra la información de todas las aplicaciones registradas.
Es una aplicación Spring Boot que se despliega como un proyecto independiente.


## Configuración

### *Agregar dependencias al proyecto existente*

Debes asegurarte de que tu proyecto Spring Boot tenga las siguientes dependencias en el archivo pom xml:

**Spring Boot Admin Client:** Esto permitirá que tu aplicación se registre en el servidor.

**Spring Boot Actuator:** Esto habilita los endpoints necesarios para el monitoreo.

```jsx title="pom.xml"
<!-- Spring Boot Admin -->
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>3.0.2</version> <!-- Reemplaza por la versión adecuada -->
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### *Configurar el application.properties*

```jsx title="application.properties"
## SpringBoot Admin Configuration

# Configura la URL del servidor de Spring Boot Admin, que se utiliza para monitorear y gestionar aplicaciones de Spring Boot.
spring.boot.admin.client.url=http://localhost:8081

# Expone todos los endpoints de administración a través de la web, lo que incluye la información de salud, métricas, etc.
management.endpoints.web.exposure.include=*

# Muestra siempre detalles completos de la salud de la aplicación (como el estado de los componentes, bases de datos, etc.).
management.endpoint.health.show-details=always

# Habilita la exposición del archivo de logs a través de los endpoints de administración, permitiendo consultar los logs de la aplicación.
management.endpoint.logfile.enabled=true

```

### *Verificar los endpoints Actuator*

Inicia tu aplicación y abre en el navegador

```jsx title="url"
http://localhost:8080/actuator
```

Respuesta

```jsx title="url"
{
    "_links": {
        "self": { "href": "/actuator", "templated": false },
        "health": { "href": "/actuator/health", "templated": false },
        "info": { "href": "/actuator/info", "templated": false }
    }
}

```

### *Inicia el servidor Spring Boot Admin*

Generar un nuevo proyecto en Spring Boot que funcionará como servidor de todas las aplicaciones que necesitemos monitorear.

### Dependencias necesarias:

-   Spring Boot Admin Server

-   Spring Web

-   Spring Boot Actuator

![springboot-admin0](/img/springbootAdmin0.png)

![springboot-admin1](/img/springbootAdmin1.png)



### *Configurar el application.properties*

```jsx title="application.properties"
server.port=8081
spring.application.name=admin-server

```





### *Agregar la anotación @EnableAdminServer en la clase principal*
```jsx title="Ejemplo"
@SpringBootApplication
@EnableAdminServer
public class SpringBootAdminApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringBootAdminApplication.class, args);
	}

}
```

### Ejecutar proyectos.

Es necesario que ambos proyectos (cliente y servidor) estén corriendo a la vez.



### Ingresar al Dashboard

Ingresar para monitorear la aplicación

http://localhost:8081