---
sidebar_position: 1
---

# 1 - Java Mail Sender

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

### Configurar usuario y contraseña

✅ Opción recomendada: Usar una contraseña de aplicación

-   Ve a https://myaccount.google.com/

-   Inicia sesión con la cuenta de Gmail que vas a usar.

-   Asegúrate de tener habilitada la verificación en dos pasos (es obligatorio).

-   Luego ve a Seguridad > Contraseñas de aplicaciones.

-   Crea una nueva contraseña de aplicación (elige "Correo" y "Otro (nombre personalizado)", como por ejemplo "Spring App").

-   Google te generará una contraseña de 16 caracteres. Esa será tu Password




