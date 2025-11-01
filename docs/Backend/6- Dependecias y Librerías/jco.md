---
sidebar_position: 3
---

# 3 -  Jco (Java Connector par SAP) 



### Paso 1: Identificar los archivos de JCo en el proyecto que te pasaron

-   Abrí el proyecto que te pasaron en IntelliJ o Eclipse.

-   Buscá dentro de las carpetas:

    -   JAR → normalmente sapjco3.jar

    -   Librerías nativas →

        -   Windows: sapjco3.dll

        -   Linux: libsapjco3.so o similar

-   Tip: Muchas veces están dentro de una carpeta libs o lib.

-   Copiá estos archivos a tu proyecto Spring Boot:

-   sapjco3.jar → crear carpeta libs/ en la raíz del proyecto.

-   Librería nativa → crear carpeta libs/native/ según el SO.

Resumen conceptual
Proyecto Spring Boot
 ├─ libs/sapjco3.jar         → agregado como dependencia
 ├─ libs/native/sapjco3.dll  → configurado en VM options






### Paso 2: Agregar el JAR como librería en IntelliJ

-   Abrí tu proyecto Spring Boot en IntelliJ.

-   File → Project Structure → Modules → Dependencies

-   Clic en + → JARs or directories

-   Seleccioná libs/sapjco3.jar

-   Marcá Scope: Compile

-   Aplicá y OK

Ahora tu proyecto puede usar las clases de JCo (com.sap.conn.jco.*).





### Paso 3: Configurar la librería nativa

-   En IntelliJ, Run → Edit Configurations

-   Seleccioná tu configuración de Spring Boot

-   En VM options(Modify options) agregá:  -Djava.library.path=libs/native --enable-native-access=ALL-UNNAMED


Esto indica a la JVM dónde buscar el .dll o .so.

No necesitás moverlo al sistema ni tocar variables de entorno.


Agregar en el POM:

```jsx title="Dependencia JCo"

   <!-- SAP - Java Connector -->
        <dependency>
            <groupId>com.sap.conn</groupId>
            <artifactId>sapjco3</artifactId>
            <version>3.1.2</version>
            <scope>system</scope>
            <systemPath>${project.basedir}/libs/sapjco3.jar</systemPath>
        </dependency>
``` 





-   Paso 4: Crear un bean de conexión JCo

Dentro de package sapConfig crear un manejador de conexiones SAP:

```jsx title="MyDestinationDataProvider"

import com.sap.conn.jco.ext.DataProviderException;
import com.sap.conn.jco.ext.DestinationDataEventListener;
import com.sap.conn.jco.ext.DestinationDataProvider;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

/**
 * Implementación de DestinationDataProvider para manejar conexiones SAP en memoria.
 * Permite registrar destinos SAP y devolver sus propiedades cuando JCo los necesita.
 */
public class MyDestinationDataProvider implements DestinationDataProvider {

    private final Map<String, Properties> destination = new HashMap<>();

    public void addDestination(String name, Properties props) {
        destination.put(name, props);
    }


    @Override
    public Properties getDestinationProperties(String destinationName) throws DataProviderException {
        return destination.get(destinationName);
    }

    @Override
    public boolean supportsEvents() {
        return false;
    }

    @Override
    public void setDestinationDataEventListener(DestinationDataEventListener destinationDataEventListener) {

    }
}

``` 


En el mismo package y como hermana de la clase anterior creada, agregar esta clase:

crear un bean para manejar la conexión a SAP:

```jsx title="SapConfig"
import com.sap.conn.jco.JCoDestination;
import com.sap.conn.jco.JCoDestinationManager;
import com.sap.conn.jco.JCoException;
import com.sap.conn.jco.ext.DestinationDataProvider;
import com.sap.conn.jco.ext.Environment;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import java.util.Properties;


/**
 * Clase para configurar la conexión SAP
 *
 * Registra un proveedor de destinos SAP en memoria y expone el bean
 * {@link JCoDestination} para ser inyectado en servicios que llamen RFCs.
 *
 */
@Configuration
public class SapConfig {

    @Value("${sap.config.host}")
    private String host;

    @Value("${sap.config.nsystem}")
    private String system;

    @Value("${sap.config.client}")
    private String client;

    @Value("${sap.config.lang}")
    private String lang;

    @Value("${sap.config.pool}")
    private String pool;

    @Value("${sap.config.limit}")
    private String limit;

    @Value("${sap.config.user}")
    private String user;

    @Value("${sap.config.pass}")
    private String pass;

    @Value(" ${sap.config.dest}")
    private String destination;



    @Bean
    public JCoDestination jcoDestination() throws JCoException {

        Properties connectProperties = new Properties();

        /** Host SAP */
        connectProperties.setProperty(DestinationDataProvider.JCO_ASHOST, host);

        /** Número de sistema */
        connectProperties.setProperty(DestinationDataProvider.JCO_SYSNR, system);

        /** Cliente SAP */
        connectProperties.setProperty(DestinationDataProvider.JCO_CLIENT, client);

        /** Usuario */
        connectProperties.setProperty(DestinationDataProvider.JCO_USER, user);

        /** Pass */
        connectProperties.setProperty(DestinationDataProvider.JCO_PASSWD, pass);

        /** Idioma */
        connectProperties.setProperty(DestinationDataProvider.JCO_LANG, lang);

        /** Pool de conexiones */
        connectProperties.setProperty(DestinationDataProvider.JCO_POOL_CAPACITY, pool);

        /** Máximo de conexiones */
        connectProperties.setProperty(DestinationDataProvider.JCO_PEAK_LIMIT, limit);


        /** Registrar el proveedor de destino */
        MyDestinationDataProvider provider = new MyDestinationDataProvider();
        provider.addDestination(destination, connectProperties);

        if(!Environment.isDestinationDataProviderRegistered()) {
            Environment.registerDestinationDataProvider(provider);
        }
        return JCoDestinationManager.getDestination(destination);
    }
}
``` 


Este bean se puede inyectar en cualquier servicio de tu API REST.

Ya podés llamar funciones RFC de SAP desde tu código.






### Paso 5: Probar la conexión desde tu API REST

Ejemplo de un endpoint simple que llame SAP:



``` jsx title= "CommercialDataController"
@RestController
@RequestMapping("/api/commercial")
public class CommercialDataController {

    @Autowired
    private ICommerialDataService comerialDataService;



    @Operation(summary ="Obtiene datos de la cuenta." , description = "Recupera la información de la cuenta por su número.")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "200", description = "Datos obtenidos correctamente."),
            @ApiResponse(responseCode = "404", description = "Datos de la cuenta no encontrados.")
    })
    @GetMapping("/account-data")
    public ResponseEntity<Response<AccountResponseDTO>> getAccountData(@Valid HttpServletRequest request,  AccountRequestDTO accountRequestDTO){
        Response<AccountResponseDTO> response = comerialDataService.getAccountData(request, accountRequestDTO);
        return new ResponseEntity<>(response, HttpStatus.OK);
    }
``` 

```jsx title="SapController"
@Service
public class CommercialDataService implements ICommerialDataService {

    @Autowired
    private MessageSource messageSource;

    @Value("${rfc.account}")
    private String rfcAccount;

    @Value("${rfc.document}")
    private String rfcDocument;

    private final JCoDestination destination;
    private final E2VaultWS soapPort;


    public CommercialDataService(JCoDestination destination, E2VaultWS soapPort) {
        this.destination = destination;
        this.soapPort = soapPort;
    }


    /**
     * Método para obtener datos de la cuenta
     *
     * @param request
     * @param accountRequestDTO
     * @return
     */
    @Override
    public Response<AccountResponseDTO> getAccountData(HttpServletRequest request, AccountRequestDTO accountRequestDTO) {
        // Obtener IP del cliente
        String clientIp = getClientIp(request);

        try{
            JCoFunction function = destination.getRepository().getFunction(rfcAccount);

            if(function == null){
                throw new NotFoundException("exception.commercialDataService.rfcNotFound.user", null, "exception.commercialDataService.rfcNotFound.log", new Object[]{accountRequestDTO, "CommercialDataService","getAccountData"}, LogLevel.WARN);
            }

            function.getImportParameterList().setValue(SapParametersAccount.PI_VKONT.getLabel(), accountRequestDTO.account());

            function.execute(destination);

            JCoStructure account = function.getExportParameterList().getStructure(SapParametersAccount.PE_INFO_CTA_CTO.getLabel());

            AccountResponseDTO accountResponseDTO = new AccountResponseDTO(
                    account.getString(SapParametersAccount.OWNERSHIP.getLabel()),
                    account.getString(SapParametersAccount.STREET.getLabel()),
                    account.getString(SapParametersAccount.NUMBER.getLabel()),
                    account.getString(SapParametersAccount.FLOOR.getLabel()),
                    account.getString(SapParametersAccount.DEPARTMENT.getLabel()),
                    account.getString(SapParametersAccount.FUNCTIONAL_UNIT.getLabel()),
                    account.getString(SapParametersAccount.ZIP_CODE.getLabel()),
                    account.getString(SapParametersAccount.LOCALITY.getLabel()),
                    account.getString(SapParametersAccount.ZZCOD_ADHE.getLabel()),
                    account.getString(SapParametersAccount.ZZMAILFD.getLabel())
            );

            String messageLog = messageSource.getMessage("commercialDataService.getMetadata.log", null, LocaleContextHolder.getLocale());

            return new Response<>(true, messageLog, accountResponseDTO);

        }catch (JCoException e){
            throw new SapConnectionException("exception.commercialDataService.catch.user", null, "exception.commercialDataService.catch.log", new Object[]{accountRequestDTO.account(),"accountRequestDTO","getAccountData"}, LogLevel.ERROR);

        }
    }
``` 



Esto devuelve los atributos de la conexión SAP para verificar que JCo funciona.



--------------------

## Información importante

SAP JCo3 no está 100% hecho en Java.

Parte Java (sapjco3.jar)

Está escrita en Java puro.

Define las clases que vos usás en tu código, por ejemplo:

```jsx title=""
JCoDestination destination = JCoDestinationManager.getDestination("SAP_CONFIG");

```





Parte Nativa (sapjco3.dll / .so)

Está escrita en C/C++.

Es la parte que realmente hace el “trabajo pesado”:
hablar con SAP a nivel de red, manejar sockets, autenticación RFC, etc.

Java no puede hacer eso directamente, por eso necesita pedirle ayuda al sistema operativo a través de esa DLL.

En resumen:

JCo3.jar es como la “cara Java” visible,
sapjco3.dll es el “motor oculto” en C/C++ que hace las operaciones de bajo nivel.