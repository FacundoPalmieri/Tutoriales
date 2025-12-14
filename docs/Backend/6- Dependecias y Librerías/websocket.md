---
sidebar_position: 5
---

# 5 -  Web Socket


## 1️⃣ Qué es un WebSocket

WebSocket es un protocolo de comunicación bidireccional sobre un único socket TCP.

Permite que cliente y servidor envíen mensajes en tiempo real, sin la necesidad de realizar peticiones HTTP repetidas.

A diferencia de HTTP tradicional (request/response), el WebSocket mantiene una conexión abierta, lo que reduce latencia y tráfico innecesario.

Es ideal para casos donde se necesita actualización en tiempo real, como:

-   Notificaciones instantáneas

-   Chats

-   Sistemas de colas o estados (ej. turnos en consultorios)

-   Dashboards en tiempo real

## 2️⃣ Cómo funciona un WebSocket en Spring Boot

### Handshake inicial:

-   El cliente envía una petición HTTP al endpoint del WebSocket para “abrir la conexión”.

-   Spring Boot verifica si la conexión puede abrirse.

-   Aquí es donde se pueden validar tokens JWT u otros mecanismos de autenticación.

### Conexión abierta:

-   Una vez aceptado el handshake, se establece un canal bidireccional persistente.

-   El cliente puede enviar mensajes al servidor y recibir mensajes en tiempo real sin reabrir la conexión.


### STOMP (Simple Text Oriented Messaging Protocol):

-   Es un protocolo de mensajería que se utiliza sobre WebSocket.

-   Permite organizar los mensajes en “topics” o “queues” y soporta suscripciones de manera sencilla.

-   Facilita la integración con Spring Boot usando controladores tipo @MessageMapping y suscripciones tipo @SubscribeMapping.

### Cierre de la conexión:

-   Cuando el cliente se desconecta o el servidor cierra la conexión, se libera el socket TCP.

-   Es posible manejar eventos de cierre y reconexión desde el lado del cliente.

## 3️⃣ Cómo funciona la clase de configuración (WebSocketConfig)

Su función es configurar Spring Boot para usar WebSocket con STOMP.

Componentes principales:

-   Registro de endpoint STOMP:

-   Define la URL a la que se conecta el cliente para abrir el WebSocket.

-   Permite habilitar SockJS, que actúa como fallback para navegadores que no soportan WebSocket.

-   Permite definir orígenes permitidos (CORS) y agregar interceptores como JWT.

Configuración del broker de mensajes:

-   El broker es un componente que recibe y distribuye mensajes.

-   Spring Boot ofrece un broker simple en memoria (SimpleBroker) para casos de baja complejidad.

-   Se configuran prefijos:

-   applicationPrefix: prefijo para rutas que el cliente envía a los controladores de Spring (@MessageMapping).

-   brokerPrefix: prefijo para rutas a las que el cliente se suscribe para recibir mensajes (@SubscribeMapping o /topic/...).

Interceptores de handshake:

-   Permiten ejecutar lógica antes de abrir la conexión, por ejemplo:

-   Validar tokens JWT

-   Guardar información del usuario en la sesión WebSocket

En conjunto, esta clase asegura que solo usuarios autenticados puedan abrir la conexión y define cómo se enviarán y recibirán los mensajes.




```jsx title="WebSocketConfig"

/**
 * Configuración principal de WebSocket para la aplicación.
 *
 * <p>
 * Esta clase habilita y configura el soporte de WebSocket con STOMP en Spring Boot.
 * Define el endpoint de conexión, permite configurar CORS, habilitar SockJS y registra
 * un broker simple de mensajes en memoria.
 * </p>
 *
 * <p>
 * La configuración se basa en propiedades externas (WebSocketProp) para no hardcodear valores
 * y permitir que cambien según el entorno (desarrollo, producción, multi-tenant SaaS).
 * </p>
 *
 * <p>
 * Se agregan interceptores de handshake, como {@link JwtHandshakeInterceptor}, que permiten
 * autenticar usuarios antes de abrir la conexión WebSocket.
 * </p>
 *
 * <p>
 * Uso de prefijos:
 * <ul>
 *     <li>Application Destination Prefix: prefijo para mensajes enviados a controladores Spring (@MessageMapping)</li>
 *     <li>Broker Prefix: prefijo para rutas de suscripción donde los clientes reciben mensajes (@SubscribeMapping o /topic/...)</li>
 * </ul>
 * </p>
 *
 * @author Facundo Palmieri
 * @see org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker
 * @see JwtHandshakeInterceptor
 */
@Configuration
@EnableWebSocketMessageBroker // Habilita soporte WebSocket + protocolo STOMP
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    private final WebSocketProp props;
    private final JwtHandshakeInterceptor jwtHandshakeInterceptor;

    @Autowired
    public WebSocketConfig(WebSocketProp props, JwtHandshakeInterceptor jwtHandshakeInterceptor) {
        this.props = props;
        this.jwtHandshakeInterceptor = jwtHandshakeInterceptor;
    }



    /**
     * Registra el endpoint STOMP para el WebSocket.
     *
     * <p>
     * Este endpoint es la URL a la que los clientes se conectarán para iniciar la sesión
     * WebSocket. Se aplica configuración de CORS, se habilita SockJS si corresponde y
     * se agregan interceptores para autenticación.
     * </p>
     *
     * @param registry Registro de endpoints STOMP
     */
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {

        // 1) Registra el endpoint del WebSocket (ej: /ws)
        //    Es el punto donde el frontend se conecta por primera vez
        StompWebSocketEndpointRegistration reg = registry
                .addEndpoint(props.getEndpoint())

                // Permitimos CORS según configuración externa
                .setAllowedOrigins(props.getAllowedOrigins().split(","));

        // 2) Opcionalmente habilita SockJS para cuando el navegador NO soporta WebSocket real
        if (props.isUseSockjs()) {

            // Añade el interceptor JWT también a SockJS
            reg.withSockJS().setInterceptors(jwtHandshakeInterceptor);

        } else {
            // Sin SockJS: agregamos el interceptor directamente
            reg.addInterceptors(jwtHandshakeInterceptor);
        }
    }






    /**
     * Configura el broker de mensajes que se utilizará para enviar y recibir mensajes.
     *
     * <p>
     * Se habilita un broker simple en memoria y se definen prefijos para diferenciar
     * mensajes enviados a controladores de Spring y mensajes de suscripción de clientes.
     * </p>
     *
     * @param registry Registro de broker de mensajes
     */
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {

        // 3) Definimos a qué prefijo el servidor enviará mensajes
        //    Ej: "/topic" → el profesional se suscribe a /topic/consultas
        registry.enableSimpleBroker(props.getBrokerPrefix());

        // 4) Prefijo de los mensajes que el cliente envía al servidor
        //    Ej: "/app" → los clientes envían mensajes a /app/consultarEstado
        registry.setApplicationDestinationPrefixes(props.getAppPrefix());
    }
}


```



## 4️⃣ Cómo funciona la clase JwtHandshakeInterceptor

Es un interceptor que se ejecuta durante el handshake inicial de un WebSocket.

Funciones principales:

-   Extracción del token JWT:

-   Obtiene el JWT enviado por el cliente, ya sea como:

-   Query parameter (?token=...)

-   Header (Sec-WebSocket-Protocol), utilizado por STOMP.js cuando no se pueden enviar headers personalizados

Validación del token:

Usa un servicio centralizado (JwtUtils) para:

-   Verificar la firma del token

-   Validar el emisor

-   Revisar la expiración

Si el token es inválido, el handshake se rechaza y el cliente no se conecta.

Almacenamiento de información de usuario en attributes:

-   Guarda datos como username y authorities en un mapa asociado a la sesión WebSocket.

-   Esto permite que los controladores STOMP identifiquen al usuario en cada mensaje sin tener que validar el token nuevamente.

Equivale al SecurityContextHolder de Spring Security en HTTP, pero para WebSocket.

### Método beforeHandshake:

Se ejecuta antes de que se abra la conexión.

Devuelve true si el token es válido → permite la conexión

Devuelve false si el token es inválido → bloquea la conexión

### Método afterHandshake:

Se ejecuta después de abrir la conexión.

En muchos casos, no se necesita lógica aquí.

Se puede usar para logging o limpieza de recursos, pero no es obligatorio.



```jsx title="JwtHandshakeInterceptor"


/**
 * Interceptor de handshake WebSocket que valida JWT.
 *
 * <p>
 * Esta clase intercepta la apertura de la conexión WebSocket (handshake) y valida
 * la autenticidad del token JWT enviado por el cliente. Si el token es válido,
 * se permite abrir la conexión; si no, se rechaza.
 * </p>
 *
 * <p>
 * Además, extrae información del usuario (username y authorities) y la almacena
 * en los atributos de sesión WebSocket, para que los controladores STOMP puedan
 * identificar al usuario en cada mensaje.
 * </p>
 *
 * <p>
 * No se utiliza la cadena de filtros HTTP de Spring Security, ya que el WebSocket
 * no pasa por ella. La autenticación se realiza exclusivamente en el handshake.
 * </p>
 * @see org.springframework.web.socket.server.HandshakeInterceptor
 * @see JwtUtils
 */
@Component
public class JwtHandshakeInterceptor implements HandshakeInterceptor {

    private final JwtUtils jwtUtils;

    @Autowired
    public JwtHandshakeInterceptor(JwtUtils jwtUtils) {
        this.jwtUtils = jwtUtils;
    }


    /**
     * Método que se ejecuta antes de abrir el WebSocket (handshake).
     *
     * <p>
     * Extrae el token JWT de los parámetros de la request o del header
     * "Sec-WebSocket-Protocol", valida el token y almacena el username y las
     * authorities en los atributos de sesión.
     * </p>
     *
     * @param request Request HTTP del handshake
     * @param response Response HTTP del handshake
     * @param wsHandler Handler del WebSocket
     * @param attributes Mapa de atributos de la sesión WebSocket donde se guardan datos del usuario
     * @return true si el token es válido y se permite la conexión, false si se rechaza
     * @throws Exception Si ocurre un error en la validación del token
     */
    @Override
    public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Map<String, Object> attributes) throws Exception {

        // 1. Obtener JWT desde query param o headers
        String token = null;

        if (request instanceof ServletServerHttpRequest servletRequest) {
            HttpServletRequest httpReq = servletRequest.getServletRequest();
            token = httpReq.getParameter("token");

            if (token == null) {
                token = httpReq.getHeader("Sec-WebSocket-Protocol");
            }
        }

        if (token == null) {
            return false; // handshake rechazado
        }

        try {
            // 2. Validar token
            DecodedJWT decoded = jwtUtils.validateToken(token);

            // 3. Guardar info del usuario para que STOMP la lea
            attributes.put("username", jwtUtils.extractUsername(decoded));
            attributes.put("authorities",
                    jwtUtils.getSpecificClaim(decoded, "authorities").asString());

            return true;

        } catch (Exception e) {
            return false; // rechazar handshake
        }
    }





    /**
     * Método que se ejecuta después de abrir el WebSocket (handshake).
     *
     * <p>
     * Se puede usar para logging, métricas o limpieza de recursos. En esta implementación,
     * no se requiere ninguna acción posterior al handshake.
     * </p>
     *
     * @param request Request HTTP del handshake
     * @param response Response HTTP del handshake
     * @param wsHandler Handler del WebSocket
     * @param exception Excepción que ocurrió durante el handshake, si la hay
     */
    @Override
    public void afterHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler, Exception exception) {
    }
}

```


## 5️⃣ Resumen conceptual

WebSocket: comunicación bidireccional y persistente para mensajes en tiempo real.

Spring Boot + STOMP: organiza los mensajes en rutas (/app/..., /topic/...) y permite controladores que actúan como endpoints de mensajes.

WebSocketConfig: define la URL de conexión, el broker de mensajes y agrega interceptores de seguridad.

JwtHandshakeInterceptor: autentica la conexión antes de abrirla, valida el JWT y guarda información del usuario para su uso en controladores.

Todo esto permite construir un sistema en tiempo real seguro, donde los usuarios deben estar autenticados y los controladores saben exactamente quién envía cada mensaje.