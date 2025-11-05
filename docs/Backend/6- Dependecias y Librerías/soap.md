---
sidebar_position: 4
---

# 4 -  SOAP


## Conceptos

### 1️⃣ Web Service

Qué es: un servicio que se expone por Internet para que otras aplicaciones lo consuman.

Qué hace: permite que un programa llame a otro programa (aunque sean de distintos lenguajes o plataformas) y obtenga datos o ejecute operaciones.

Ejemplo:  Puede ser API REST (Json) o SOAP (xml)

💡 Analogía: el Web Service es la cocina del restaurante: recibe pedidos (requests) y devuelve platos (responses).

### 2️⃣ SOAP (Simple Object Access Protocol - Protocolo simple de acceso a objetos.)

Qué es: un protocolo de comunicación que define cómo se estructuran los mensajes entre tu aplicación y el Web Service.

Qué hace:

Define cómo enviar datos en XML.

Describe errores, encabezados, seguridad.

Relación con Web Service: es solo la forma de hablarle al Web Service.

💡 Analogía: SOAP es el lenguaje que usan los camareros para transmitir tu pedido a la cocina.

### 3️⃣ WCF (Windows Communication Foundation - Fundación de Comunicación de Windows.)

Qué es: un framework de .NET para crear Web Services.

Qué hace:

Te permite exponer un Web Service SOAP o REST.

Genera automáticamente el WSDL(Web Services Description Language -Lenguaje de descripción de servicios web.) y los proxies cliente.

Maneja la comunicación, seguridad y bindings.

Ejemplo :
-   Java – Spring Boot → Web Service API REST

-   .NET – WCF         → Web Service SOAP (también puede exponer REST)

💡 Analogía: WCF es la cocina completa equipada que te permite implementar el restaurante y recibir pedidos con un protocolo definido (SOAP).

### 4️⃣ WSDL(Web Services Description Language -Lenguaje de descripción de servicios web.)

Un archivo XML que describe:

Qué operaciones tiene el servicio.

Qué mensajes acepta y devuelve.

Qué tipos de datos usa.

Dónde está publicado el servicio (la URL del endpoint).

Es el contrato del servicio SOAP.
Java, .NET, y otros lenguajes lo usan para generar automáticamente las clases necesarias para consumir el servicio.


```jsx title="Ejemplo WSDL"
<wsdl:definitions name="ServiceRestriccionServicio"
    targetNamespace="http://tempuri.org/">
  
  <wsdl:portType name="IServiceRestriccionServicio">
     <wsdl:operation name="ConsultarRestricciones">
        <wsdl:input message="tns:ConsultarRestriccionesRequest"/>
        <wsdl:output message="tns:ConsultarRestriccionesResponse"/>
     </wsdl:operation>
  </wsdl:portType>

</wsdl:definitions>

``` 

![soap](/img/soap.png)

Web Service = API REST o SOAP → es el concepto de “servicio web”.

SOAP → XML + WSDL → protocolo formal para Web Services.

REST → HTTP + JSON (o XML) → estilo ligero para Web Services.

Spring Boot → te permite crear APIs REST fácilmente en Java.

WCF → framework de .NET para crear Web Services SOAP (y REST si querés).


Un WSDL (Web Services Description Language) es un archivo XML que describe de forma estructurada cómo interactuar con un servicio web SOAP.


📌 Relación SOAP ↔ WSDL

SOAP: es el protocolo (basado en XML) usado para enviar y recibir mensajes entre cliente y servidor.

WSDL: es la descripción técnica de ese servicio SOAP.

Es decir:
👉 SOAP = el "mensaje"
👉 WSDL = el "manual" para entender cómo armar y enviar esos mensajes.


📑 ¿Qué contiene un WSDL?

Dentro de su XML encontrarás secciones como:

Types → Define los tipos de datos (generalmente en XML Schema).

Messages → Describe la estructura de los mensajes de entrada y salida.

PortType → Define las operaciones (métodos) disponibles en el servicio.

Binding → Indica cómo se transportan esos mensajes (por ejemplo SOAP sobre HTTP).

Service → La dirección (endpoint) donde se encuentra el servicio.



SOAP no lo inventás vos, viene definido en un WSDL (Web Services Description Language).
Ese archivo describe:

-   qué operaciones tiene el servicio,

-   qué parámetros espera,

-   qué devuelve.



## Dependencias

En tu pom.xml pusiste:

JAX-WS API → la especificación Java para SOAP.

JAX-WS Implementation (jaxws-rt) → la implementación real (de Sun/Oracle).

JAXB API + runtime → se usa para convertir entre XML ↔ Objetos Java.

SOAP API + Implementation (saaj) → otra parte necesaria para manejar mensajes SOAP a bajo nivel.

Plugin jaxws-maven-plugin → clave: a partir del WSDL que le das, te genera automáticamente las clases Java del cliente (los param, response, service, port, etc.).

👉 Por eso decís “se me descargan un montón de clases que no sé para qué son”:
son los objetos que representan el contrato SOAP. Ejemplo: WsRenderParam, WsPDFSecurityParam, WsRenderPages → todos fueron generados por ese plugin a partir del WSDL.




```jsx title="Dependencias"
<!-- JAX-WS API - especificación  de Java para trabajar con servicios web basados en SOAP.-->
        <dependency>
            <groupId>jakarta.xml.ws</groupId>
            <artifactId>jakarta.xml.ws-api</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- JAX-WS Implementation real de JAX-WS. -->
        <dependency>
            <groupId>com.sun.xml.ws</groupId>
            <artifactId>jaxws-rt</artifactId>
            <version>3.0.2</version>
        </dependency>

        <!-- JAXB Java Architecture for XML Binding -  mapea XML a Objetos Java -->
        <dependency>
            <groupId>jakarta.xml.bind</groupId>
            <artifactId>jakarta.xml.bind-api</artifactId>
            <version>3.0.1</version>
        </dependency>

        <!-- jaxb-runtime Java implementación concreta de JAXB -->
        <dependency>
            <groupId>org.glassfish.jaxb</groupId>
            <artifactId>jaxb-runtime</artifactId>
            <version>3.0.2</version>
        </dependency>

        <!-- Jakarta SOAP API -->
        <dependency>
            <groupId>jakarta.xml.soap</groupId>
            <artifactId>jakarta.xml.soap-api</artifactId>
            <version>3.0.2</version>
        </dependency>

        <!-- Jakarta SOAP Implementation -->
        <dependency>
            <groupId>com.sun.xml.messaging.saaj</groupId>
            <artifactId>saaj-impl</artifactId>
            <version>3.0.4</version>
        </dependency>
``` 


```jsx title="Plugin"
 <!--Plugin wsimport genera automáticamente el cliente Java a partir del WSDL -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>jaxws-maven-plugin</artifactId>
                <version>2.6</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>wsimport</goal>
                        </goals>
                        <configuration>
                            <wsdlUrls>
                                <wsdlUrl>http://10.40.1.174:8080/E2VaultWS/E2VaultWS?wsdl</wsdlUrl>
                                <!-- ✅ URL del WSDL del servicio SOAP que querés consumir (No es la URL de los endpoints) -->
                            </wsdlUrls>

                            <packageName>dti.aysa.autogestion.soapclient</packageName> 
                            <!-- ✅ Nombre del package en el que se van a generar las clases Java (stubs/proxies para invocar el servicio). -->

                            <sourceDestDir>${project.basedir}/src/main/java</sourceDestDir> 
                            <!-- ✅ Carpeta de destino donde se generan las clases. 
                            ${project.basedir} apunta a la raíz del proyecto. -->

                         <xnocompile>true</xnocompile> 
                         <!-- ✅ Opción para que no intente compilar automáticamente los .java generados.
                         Solo los genera, y luego Maven los compila en la fase normal del ciclo. -->

                        </configuration>
                    </execution>
                </executions>
            </plugin>

``` 


### Actualizar dependencias

-   Hacés click derecho sobre el pom.xml → "Maven" → "Reload project".


- Plugin:

    -   Abrí la ventana Maven (derecha → ícono con la taza de café).

    -   Expandí tu proyecto → Lifecycle.

    -   Buscá Install.

    -   Genera las clases en la carpeta indicada.



🔹 ¿Qué es E2VaultWS?

E2VaultWS no es algo genérico de SOAP ni de Java ni de Spring Boot.

Es un servicio web concreto que alguien (en tu caso, la app .NET de la empresa) expuso a través de SOAP.

Cuando corrés el wsimport contra el WSDL de esa aplicación, se generan clases con nombres basados en lo que el WSDL declara.

En tu caso el WSDL define un servicio llamado E2VaultWS.

Por eso las clases generadas se llaman E2VaultWS_Service, E2VaultWS, etc.



🔹 ¿Y si mañana tengo que usar SOAP con otra empresa?

Vas a tener otro WSDL (por ejemplo, http://empresaX.com/services/Facturacion?wsdl).

Cuando corras el wsimport contra ese nuevo WSDL:

Se generará otra clase de servicio (ej: Facturacion_Service).

Se generará otra interfaz de port (ej: FacturacionPortType).

Y otras clases de request/response (ej: FacturaRequest, FacturaResponse).


Entonces, si mañana trabajás con otra empresa que también usa SOAP, no vas a usar E2VaultWS.
Vas a usar el nombre del servicio que esa empresa exponga en su propio WSDL, que puede llamarse distinto, tener operaciones distintas y estructuras distintas.

👉 O sea: cada WSDL define su propio “mundo” de clases Java.
Vos no usás siempre E2VaultWS.
Mañana será FacturaWS, ClientesWS, PagosWS, lo que sea que defina el otro servicio.




🔹 ¿Qué es lo “genérico” entonces?

Lo genérico es la tecnología SOAP (y las librerías que usás: JAX-WS, JAXB).
Siempre vas a repetir el mismo patrón de trabajo:

Tener el WSDL.

Generar las clases con wsimport (cada una con nombres distintos según el WSDL).

Configurar el cliente (_Service, getPort()).

Armar la request con los objetos generados.

Ejecutar la operación (port.metodoX(request)).

Leer la respuesta.

Pero las clases (E2VaultWS, WsRenderParam, WsRenderPages…) cambian 100% según el WSDL.


## Configuración de SOAP   

Definimos el puerto de conexión a SOAP


```jsx title="properties"
# Perfil activo

//Toma el valor de la variable de entorno, sino está definido toma dev por default
spring.profiles.active= ${APP_PROFILE:dev}


# URLs SOAP según entorno
vault.soap.endpoint.dev=${VAULT_SOAP_ENDPOINT_DEV} 
vault.soap.endpoint.prod=${VAULT_SOAP_ENDPOINT_PROD}
``` 





```jsx title="SoapClientConfig"

@Configuration
public class SoapClientConfig {

    @Value("${spring.profiles.active}") String profile;
    @Value("${vault.soap.endpoint.dev}") String devEndpoint;
    @Value("${vault.soap.endpoint.prod}") String prodEndpoint;

    @Bean // Se define un bean que spring guarda internamente
    @Lazy
    public E2VaultWS e2VaultWSService() throws MalformedURLException {
        String endpointUrl = profile.equalsIgnoreCase("PROD") ? prodEndpoint : devEndpoint;
        E2VaultWS_Service service = new E2VaultWS_Service(new URL(endpointUrl));
        return service.getE2VaultWSPort();
    }
}
``` 



Cuando Spring construye CommercialDataService, busca un bean que implemente/interfaze con E2VaultWS.

Encuentra el que vos definiste en SoapClientConfig.

Entonces te lo inyecta automáticamente en el constructor. 

```jsx title="Service"
@Service
public class CommercialDataService implements ICommerialDataService {
   private final E2VaultWS soapPort;


    public CommercialDataService(E2VaultWS soapPort) {

        this.soapPort = soapPort; // Inyecta el Bean automáticamente cuando detecta el tipo de dato E2VaultWS. El bean tiene el puerto
    }

   @Override
    public byte[] getResource(HttpServletRequest request, AccountInternalRequestDTO accountInternalRequestDTO) throws Exception{

        // Obtener IP del cliente
         String clientIp = getClientIp(request);

        WsRenderParam param = new WsRenderParam();
        param.setDbname("fc");
        param.setDate("");
        param.setFile("");
        param.setPointer("");
        param.setAccount(accountInternalRequestDTO.numberInternal());
        param.setOutputformat(WsOutputFormat.PDF);
        param.setResolution(300);

        WsPDFSecurityParam security = new WsPDFSecurityParam();
        security.setPdfsecuritymode(1);
        security.setPermissionPrint(1);
        security.setPermissionCopy(-1);
        security.setPermissionModifycontents(-1);
        security.setPermissionModifyannotationform(-1);
        param.setPdfsecurity(security);

        // llamada SOAP
        WsRenderPages  response = soapPort.renderTransform(param);

        if (response == null || response.getPagedatabytes() == null) {
            throw new Exception("No se recibió PDF del servicio SOAP");
        }

        return response.getPagedatabytes();
    }
}
