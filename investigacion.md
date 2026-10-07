**# Investigación: Arquitectura Web, Frontend, Backend y Servicios de Hospedaje

## 1. Arquitectura Web: Separación entre Frontend y Backend

En el desarrollo de sistemas web modernos, la arquitectura se divide conceptualmente en dos grandes capas interactuantes: el *frontend* (o lado del cliente) y el *backend* (o lado del servidor). Esta distinción determina no solo qué equipo ejecuta cada conjunto de instrucciones, sino también los lenguajes, herramientas y protocolos de seguridad involucrados.

El *frontend* comprende todo lo que se ejecuta directamente en la máquina del usuario final a través del navegador web (como Google Chrome, Mozilla Firefox o Safari). Su responsabilidad principal es presentar la interfaz de usuario, gestionar las interacciones visuales y capturar los eventos del usuario. Los lenguajes fundamentales que componen esta capa son HTML (que proporciona la estructura semántica), CSS (que define la presentación visual) y JavaScript o TypeScript (que aportan lógica de interacción e hiperactividad). Sistemas comunes como Spotify Web o Google Maps ejecutan una porción masiva de código en el navegador para permitir que un usuario arrastre un mapa o reproduzca una lista sin necesidad de recargar la página completa.

Por otro lado, el *backend* es la lógica de negocio que se ejecuta en servidores remotos o en la nube. Esta capa procesa peticiones, valida reglas de negocio, gestiona sesiones de usuario y administra la comunicación persistente con las bases de datos. A diferencia del navegador, el entorno del servidor no está restringido a un motor JS cliente, por lo que permite utilizar un abanico diverso de lenguajes como Python, Java, C#, Node.js, Go o PHP. Siguiendo el ejemplo de un sistema bancario o la tienda de Amazon, cuando un cliente presiona el botón de "Comprar", el *frontend* únicamente envía la orden; es el *backend* el encargado de verificar si el usuario tiene saldo suficiente, descontar el inventario en la base de datos y comunicarse con la pasarela de pagos.

---

## 2. El Flujo de Ejecución Web Paso a Paso

El proceso que ocurre desde que un usuario digita una dirección URL en la barra del navegador (por ejemplo, `https://www.ejemplo.com/tienda`) hasta que la página se despliega completamente involucra múltiples capas de red y negociación de protocolos:

1. **Resolución de Nombres de Dominio (DNS):** El navegador no comprende nombres de dominio legibles como `ejemplo.com`, sino direcciones IP. Primero busca en la memoria caché local del sistema operativo; si no la encuentra, realiza una consulta recursiva a los servidores DNS de Internet para traducir `ejemplo.com` a una dirección IP pública (por ejemplo, `192.0.2.1`).
2. **Establecimiento de Conexión TCP e Intercambio TLS (Handshake):** Una vez obtenida la IP, el cliente abre un socket de conexión hacia el puerto 443 del servidor mediante el protocolo TCP (*Transmission Control Protocol*). Al tratarse de un sitio seguro bajo HTTPS, se realiza un *TLS Handshake*, en el cual el servidor envía su certificado digital, se autentica la identidad y cliente y servidor acuerdan un cifrado simétrico mediante llaves criptográficas.
3. **Petición HTTP/HTTPS (Request):** El navegador construye y envía un paquete de petición HTTP (habitualmente `GET /tienda HTTP/1.1`) que incluye encabezados (*headers*) con información como el agente de usuario, tipos de contenido aceptados y galletas de sesión (*cookies*).
4. **Procesamiento en el Servidor:** El servidor web (como Nginx, Apache o el servidor de aplicaciones del backend) recibe la solicitud, ejecuta la ruta correspondiente en el código, consulta la base de datos si es necesario y emite una respuesta HTTP.
5. **Respuesta del Servidor (Response) y Renderizado:** El servidor responde con un código de estado (como `200 OK`) y entrega un documento HTML. El navegador analiza (*parsea*) el HTML de arriba a abajo. Al encontrar referencias externas a hojas de estilo (`styles.css`), imágenes o scripts (`app.js`), el navegador inicia peticiones secundarias paralelas para descargar esos recursos adicionales. Finalmente, el motor de renderizado del navegador combina el DOM (*Document Object Model*) y el CSSOM (*CSS Object Model*) para pintar la página completa en pantalla (*paints*).

---

## 3. Interfaces de Programación de Aplicaciones (API) y Seguridad en Bases de Datos

Una **Interfaz de Programación de Aplicaciones** (API, por sus siglas en inglés) es un conjunto de reglas, especificaciones y contratos que permiten que dos aplicaciones de software se comuniquen entre sí. En el desarrollo web, las API REST o GraphQL actúan como el puente de comunicación entre el *frontend* y el *backend*, utilizando el protocolo HTTP como medio de transporte mediante peticiones codificadas habitualmente en formato JSON.

Existe una razón técnica imperativa por la cual una página estática (publicada únicamente en el cliente) **nunca puede conectarse directamente a una base de datos**: la exposición irreversible de credenciales y la falta de control de acceso. Cuando un sitio web estático se sirve en un navegador, todo su código fuente (archivos HTML, CSS y JavaScript) se descarga íntegramente en la máquina del cliente y puede ser inspeccionado mediante las Herramientas de Desarrollador (*DevTools*). Si se incluyera una cadena de conexión a la base de datos con contraseña dentro del código JavaScript del cliente, cualquier usuario podría leerla, obtener acceso directo al motor de base de datos (como MySQL o PostgreSQL) y ejecutar sentencias maliciosas como `DROP TABLE` o extraer datos sensibles de otros usuarios.

Por esta razón, la arquitectura web impone el uso del *backend* como una capa intermedia (*middleware* de seguridad). El *frontend* realiza una petición HTTP hacia un punto de acceso (*endpoint*) de la API. El *backend*, ejecutándose en un entorno seguro e inaccesible para el cliente, valida si la petición cuenta con una sesión válida, aplica las políticas de autorización, sanitiza las entradas para prevenir inyecciones SQL y realiza la consulta a la base de datos utilizando credenciales almacenadas de manera privada en variables de entorno.

---

## 4. Servicios de Hospedaje y Tipos de Infraestructura

Un servicio de hospedaje web (*web hosting*) es una infraestructura de servidores conectados permanentemente a Internet que almacena los archivos y ejecuta los procesos de una aplicación para que sea accesible públicamente.

### Hospedaje Estático vs. Servidores de Aplicaciones
Un **hospedaje estático** (como GitHub Pages, Vercel o Netlify**
