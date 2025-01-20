---
sidebar_position: 7
---

# 7 - SEO y Servidores
---
## **Introducción a SEO**

El SEO (Search Engine Optimization) o Optimización para Motores de Búsqueda es el conjunto de técnicas y estrategias utilizadas para mejorar la visibilidad de un sitio web en los resultados orgánicos de motores de búsqueda como Google, Bing o Yahoo. El objetivo es aumentar el tráfico de calidad hacia tu sitio.

### **SEO On-Page**

Son las optimizaciones que realizas dentro de tu sitio web para que los motores de búsqueda lo entiendan mejor.

### *1.  meta descripción*

Es lo primero que ven los usuarios en los resultados. Debe ser atractivo, relevante y contener palabras clave.

Debe contar entre 70 a 320 caracteres y debe estar en todos los documentos HTML.

![description](/img/description.png)


```jsx title="Ejemplo"
<meta name="description" content="Aprende HTML y CSS desde cero con ejemplos prácticos y fáciles de seguir. ¡Mejora tus habilidades en diseño web!">

```

### *2.  keywords (Palabras claves)*

Identifica las palabras o frases que los usuarios buscan.

Debe contar con al menos 10 palabras

```jsx title="Ejemplo"
<meta name="keywords" content="panaderia,masitas,desayunos">
```


### *3. favicon*

Icono que se visualiza en la ventana del navegador

:::tip
https://www.favicon-generator.org/
:::

```jsx title="Ejemplo"
<link rel="shortcut icon" href="./img/favicon.ico" type="image/x-icon">
```

### *4.  Title*

Título que aparece en la ventana del navegador.

Debe contar entre 36 a 65 caracteres.

```jsx title="Ejemplo"
<title> BogUi | Panadería Artesanal </title>
```

### *5. Protocolo Open Graph*

Permite configurar el link al momento de compartirlo por redes sociales.

-   **og:title**
Define el título de la página que aparecerá en la publicación compartida.

```jsx title="Ejemplo"
<meta property="og:title" content="Aprende HTML y CSS desde Cero">

```

-   **og:description**
Proporciona una descripción breve del contenido.
```jsx title="Ejemplo"
<meta property="og:description" content="Un curso completo para aprender desarrollo web con ejemplos claros y prácticos.">

```

-   **og:image**
Especifica la URL de una imagen que se mostrará como miniatura.
```jsx title="Ejemplo"
<meta property="og:image" content="https://example.com/curso-html-css.jpg">
```

-   **og:url**
Define la URL canónica de la página compartida.
```jsx title="Ejemplo"
<meta property="og:url" content="https://example.com/curso-html-css">
```

-   **og:type**
Especifica el tipo de contenido. Algunos ejemplos comunes son:

    -   website: Para páginas web genéricas.

    -   article: Para contenido de blogs o noticias.

    -   video: Para videos.

    -   product: Para productos específicos.
```jsx title="Ejemplo"
<meta property="og:type" content="article">
```


### *6. Jerarquia de encabezados*

Respetar la jerarquia comenzando con un H1, luego H2 y así sucesivamente 

### *7.  Enlaces*

Solo para enlaces externos usar el target blank

### *8. Accesibilidad.*

Los "alt" en las imagénes deben ser descriptivos de la foto. Debe ser posible imaginar la imagen con solo leerlo. Responde a la pregunta ¿Qué es la imagen?

### *9. Estructura semántica.*

Respetar etiquetas y estructura semánticas. Usar los divs solo en casos necesarios de agrupamiento o distribución de elementos.



### **SEO off-Page**

Se refiere a las acciones fuera de tu sitio que mejoran su autoridad.

### *1. Backlinks*

-   Consigue enlaces desde otras páginas de calidad hacia tu sitio.

-   Los backlinks son como "votos de confianza".

Ejemplo: Un blog de tecnología que enlaza a tu tutorial de CSS.


### *2.Presencia en redes sociales*

-   Comparte tu contenido en plataformas sociales para aumentar su visibilidad y generar tráfico.


### *3.Autoridad del dominio*

-   Construir autoridad lleva tiempo, pero es fundamental para posicionarte mejor.

-   Factores como antigüedad, calidad de enlaces y reputación online afectan este aspecto.


### **SEO Técnico**

Son aspectos más técnicos que facilitan que los motores de búsqueda indexen y comprendan tu sitio.

### *1. Velocidad de carga*

-   Un sitio rápido mejora la experiencia del usuario y es un factor de ranking.

-   Usar herramientas como PageSpeed Insights.


### *2. Diseño Responsivo*

-   Asegurar de que tu sitio funcione correctamente en dispositivos móviles y tablets.

### *3. Sitemap XML*

-   Un archivo que guía a los motores de búsqueda sobre la estructura de tu sitio.

Ejemplo: tupagina.com/sitemap.xml

### *4. Certificado SSL (HTTPS):*

-   Mejora la seguridad y es un factor importante para Google.


-----

## **Introducción a Servidores**

Cuando hablamos de servidores, nos referimos al ordenador que pone recursos a disposición a través de una red, o al programa que funciona en dicho ordenador.
En consecuencia, aparecer dos definiciones de servidor

1.  Hardware

2.  Software


### Servidor hardware.

Es una máquina física integrada en una red informática en la que, además del sistema operativo, funcionan uno o varios servidores basados en software.

Una denominación altenativa para un servidor basado en hardware es **"host"** (anfitrión). En principio, todo ordenador puede usarse como "host", con el correspondiente software para servidores.

### Servidor software.

Un servidor de software es un programa o conjunto de programas diseñados para proporcionar servicios específicos a otros programas o dispositivos, conocidos como clientes, a través de una red. A diferencia de un servidor físico (hardware), que es el equipo físico donde se ejecuta el servidor, un servidor de software es simplemente la aplicación que realiza la función de servir solicitudes.

La base de la comunicación es el modelo **cliente-servidor**, y en lo que concierne al intercambio de datos, entran en acción los protocolos de transimisión específicos del servicio.

### ¿Como funciona el servidor?

Un servidor funciona como un sistema que recibe solicitudes de los clientes, procesa esas solicitudes(request) y envía respuestas(response) apropiadas. Este proceso se basa en protocolos de red que regulan la comunicación entre el cliente y el servidor.

### Certificado SSL

SSL (Secure Sockets Layer) es un protocolo de seguridad que se utiliza para establecer una conexión cifrada entre un cliente (como un navegador) y un servidor (como un sitio web). Este cifrado asegura que los datos transmitidos entre ambas partes estén protegidos contra accesos no autorizados, garantizando confidencialidad e integridad.

Aunque SSL fue el estándar en el pasado, hoy en día ha sido reemplazado por TLS (Transport Layer Security), que es más seguro. Sin embargo, el término "SSL" todavía se usa comúnmente para referirse a ambos protocolos.

### *¿Qué hace SSL?*

SSL realiza las siguientes funciones clave:

#### Cifrado:

Los datos transmitidos (como contraseñas, números de tarjeta de crédito o información personal) se cifran, lo que impide que los hackers puedan leerlos si interceptan la comunicación.

#### Autenticación:

Verifica la identidad del servidor mediante un certificado SSL emitido por una entidad de confianza, como Let's Encrypt o DigiCert. Esto garantiza que el cliente se está comunicando con el servidor correcto.

#### Integridad de los Datos:

Asegura que los datos no sean alterados durante la transmisión.


### *¿Cómo funciona SSL?*

El funcionamiento de SSL/TLS se basa en un proceso llamado Handshake (apretón de manos), que incluye:

#### Conexión Inicial:

-   El cliente (por ejemplo, un navegador) se conecta al servidor e indica que quiere establecer una conexión segura.

##### Intercambio de Certificados:

El servidor envía su certificado SSL al cliente. Este contiene información como:
    -   La clave pública del servidor.
    -   El nombre del dominio.
    -   La entidad que emitió el certificado.
    -   La fecha de expiración.

#### Verificación:

El cliente verifica que el certificado sea válido y que corresponda al servidor con el que desea conectarse.


#### Establecimiento de una Clave de Sesión:

-   Ambos (cliente y servidor) negocian una clave secreta para cifrar y descifrar los datos durante la sesión.


#### Inicio de la Conexión Cifrada:

-   Los datos se transmiten cifrados usando la clave de sesión.


### Servidores Webs

Un servidor web es un sistema cuya función principal es alojar sitios web y hacerlos accesibles a través de Internet. Para que un sitio sea visible en la web, no solo se necesita un servidor, sino también un dominio, que es el nombre que identifica la página en la URL (por ejemplo, www.ejemplo.com).

**Servidor:** El host o máquina (física o virtual) donde se aloja la aplicación o el sitio web. Este servidor se encarga de procesar las solicitudes de los usuarios y entregar los recursos necesarios, como páginas web, imágenes o datos.

**Dominio:** Es el nombre único que identifica a un sitio web en Internet, facilitando el acceso de los usuarios. 

Por ejemplo, en www.ejemplo.com, "ejemplo.com" es el dominio que apunta al servidor donde está alojada la aplicación


### Adquirir Dominios y host

Es importante tener en cuenta que los paquetes posea:

-   Hosting
-   Dominio
-   SSL


#### Hostinger

https://www.hostinger.com.ar/?utm_campaign=Brand-Exact|NT:Se|LO:AR&utm_medium=ppc&gad_source=1&gclid=Cj0KCQiA4rK8BhD7ARIsAFe5LXJ_maGk0tNXXpf5EUZFdfZuPGmm4-Itf1RbwXJOPEY0_CL0vLer3n0aAs5gEALw_wcB


### Servidor Gratutito

1. Ingresar a https://www.netlify.com/

2.  Logearse con GitHub

3.  Seleccionar el proyecto 

4.  Colocarle el nombre, verificar la rama de donde lo va a tomar y realizar el deploy. 

