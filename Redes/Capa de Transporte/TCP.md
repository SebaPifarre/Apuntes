
### ¿Cuál es la función de la capa de transporte?

Un protocolo de capa de transporte provee comunicación lógica entre procesos de aplicación corriendo en diferentes hosts. Comunicación lógica se refiere a que desde la perspectiva de la aplicación, es como que los hosts corriendo los procesos estuvieron conectados directamente; en realidad los hosts pueden estar en puntas distintas del planeta, conectados vía varios routers y un amplio rango de link types. Los procesos de aplicación usan la comunicación lógica que provee la capa de transporte para enviar mensajes entre ellos, liberandose de los detalles de la infraestructura física usada para transportar los mensajes.
Los protocolos de la capa de transporte son implementados en los end systems pero no en los routers de red. Del lado que envía, la capa de transporte convierte los mensajes de la capa de aplicación en paquetes de capa de transporte conocidos como segmentos.

### Describa la estructura del segmento TCP y UDP.

![[Estructura UDP.png]]

![[Estructura TCP.png]]

### ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

El puerto es el identificador numérico de 16 bits que se utiliza para diferenciar los distintos procesos (o específicamente sus sockets) dentro de un mismo host. Dado que una computadora puede ejecutar muchas aplicaciones de red al mismo tiempo, la dirección IP identifica al host, pero el número de puerto identifica a que socket específico dentro de ese host deben entregarse los datos. Los puertos de 0 al 1023 son llamados números de puertos conocidos y están restringidos. 

### Comparación entre UDP y TCP

* <mark style="background: #BBFABBA6;">Confiabilidad</mark><mark style="background: #BBFABBA6;">:</mark>
	* TCP utiliza una combinación de números de secuencia, ACKs, temporizadores y retrasmiciones para asegurar que los datos lleguen correctamente. Garantiza que los datos lleguen sin errores, en orden y sin duplicados ni faltantes.
	* UDP solo incluye un campo de suma de comprobación para la detección de errores de bits pero no hace nada para recuperarse de ellos. No garantiza que los mensajes lleguen a destino ni que lo hagan en el orden correcto.
* <mark style="background: #BBFABBA6;">Multiplexación:</mark>
	* TODO
	* TODO
* <mark style="background: #BBFABBA6;">Orientado a la conexión:</mark>
	* TCP - Antes de enviar datos, realiza un acuerdo de tres vías (TWH) para establecer los parámetros de conexión
	* UPD - No realiza un salido inicial, lo que permite empezar a transmitir datos inmediatamente sin retardos de configuración.
* <mark style="background: #BBFABBA6;">Controles de congestión:</mark>
	* TCP puede adaptarse al estado actual de la red, enviando los segmentos más rápido si la red lo permite y enviandolos con más retraso si está congestionado.
	* UDP no tiene mecanismos de control de gestión, lo que permite a las aplicaciones enviar datos a la velocidad que deseen, ideal para aplicaciones tolerantes a la perdida.
* <mark style="background: #BBFABBA6;">Utilización de puertos:</mark>
	* TODO
	* TODO

### La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos suele utilizarse el término datagrama. Indique cuando.

En el contexto de Internet, nos referimos al paquete de capa de transporte como segmento. Sin embargo, la literatura de internet también se refiere al paquete de UDP (User Datagram Protocol) como un datagrama. Pero esta misma literatura de Internet también usa el término datagrama para el paquete de la capa de red. Como libro introductorio es menos confuso referirse tanto a los paquetes de TCP como de UDP como segmentos.

### Describa el saludo de tres vías de TCP. UDP tiene está característica?

El proceso de aplicación del cliente primero le informa al cliente TCP que quiere establecer conexión con un proceso en el servidor.
1) El cliente TCP envía un segmento especial al servidor TCP. Este segmento no contiene datos de capa de aplicación. El flag SYN en la cabecera se setea a 1, además el cliente toma un número aleatorio como número de secuencia (ISN).
2) Al recibir el SYN, el servidor asigna buffers y variables, y responde con un segmento donde el bit SYN es 1, el campo ACK se setea en cliente-isn + 1 y finalmente el servidor elige su propio isn (server-isn) y pone el valor en el campo de número de secuencia.
3) Al recibir el SYN-ACK, el cliente asigna sus propios buffers y variables para la conexión. El cliente envía el último segmento del 3WH, con el campo ACK = server-isn +1. El flag SYN seteado a cero. Este último segmento ya puede enviar datos del cliente al servidor.

UDP solo envía información sin preliminales formales. De esta manera UDP no introduce ningún delay para establecer la conexión.

### Investigue qué es el MSS. ¿Cuándo y cómo se negocia?

El MSS (Maximum Segment Size) es la cantidad máxima de datos que TCP puede tomar del buffet de envío para insertar en un segmento. (Sin tener en cuenta los headers)
El MSS se fija basandose en la MTU (Maximum Transmission Unit), que es la longitud de la trama más grande que puede enviar el host local en su capa de enlace.
El MSS se negocia exclusivamente en el 3WH utilizando el campo de opciones de la cabecera del segmento TCP, el cuál tiene una longitud variable.

### ¿Cuál es el puerto por defecto que se utiliza en los siguientes servicios?

* Web: HTTP puerto por defecto 80
* DNS: Puerto 53 (Generalmente sobre UDP)
* Web Seguro: HTTPS puerto 443
* POP3: Puerto 110
* SMTP: Puerto 25
* FTP: Puerto 21

Linux = /etc/services
Windows = C:\Windows\System32\drivers\etc\services

### Multicast

El multicast es un servicio de red que permite que un único nodo de origen envíe una copa de un paquete a un subconjunto específico de los demás nodos de red. Funciona habitualmente sobre UDP, dado que es un protocolo sin conexión, ideal para enviar datos a múltiples destinos de forma simultánea sin la necesidad de establecer un acuerdo previo con cada uno.
No funciona sobre TCP ya que esta es siempre punto a punto, necesita el mantenimiento de variables de estado especificas para ese par de hosts.

### Protocolo de aplicación FTP

El protocolo FTP (File Transfer Protocol) funciona de una manera particular utilizando dos conexiones TCP paralelas entre el cliente y el servidor.
El usuario interactúa con FTP a través de un agente de usuario. Primero el cliente establece una conexión de control con el servidor en el puerto 21. A través de esta conexión se envían la identificación de usuario, la contraseña y comandos para cambiar directorios o solicitar la transferencia de archivos.
Cuando el servidor recibe un comando para transferir un archivo, abre una conexión de datos independiente para el envío de dicho archivo. FTP crea una nueva conexión de datos para cada archivo transferido, mientras que la conexión de control permanece abierta durante toda la sesión del usuario.
<u>Modo Activo</u>: Tras recibir el comando de transferencia por la conexión de control, es el lado del servidor el que inicia la conexión de datos TCP hacia el lado del cliente.

### ¿Qué restricción existe sobre el tamaño de ventanas en el protocolo Selective Repeat?

El tamaño de ventana debe ser menor o igual a la mitad del tamaño del espacio de números de secuencia. Esta limitación es necesaria para evitar una situación en la que el receptor no puede distinguir entre la retransmisión de un paquete antiguo y la primera transmisión de un paquete nuevo.

### ¿Qué es el RTT y cómo se calcula? Investigue la opción TCP timestamp y los campos TSval y TSecr

El RTT (Round-Trip Time) es el tiempo que transcurre desde que un host envía un paquete pequeño hasta que recibe el ACK.

* <u>Tsval (Timestamp value)</u>: Es un campo de 32 bits donde el emisor del segmento coloca el valor actual de su reloj en el momento de envío.
* <u>Tsecr (Timestamp echo replay)</u>: Es un campo donde el receptor del segmento "refleja" el valor Tsval que recibió en el último segmento del emisor.

Estos campos permiten que el emisor original, al recibir el ACK, calcule el RTT simplemente restando el valor de Tsecr de su reloj actual. Esto permite realizar mediciones de RTT para cada segmento enviado, lo que proporciona una estimación mucho más dinámica y precisa que el método tradicional de una medición por ventana.

### Control de flujo

<mark style="background: #CACFD9A6;">¿Quién lo activa? ¿De que forma lo hace?</mark>
El mecanismo es controlado y "activado" por el receptor de los datos de una conexión TCP. El receptor mantiene una variable denominada ventana de recepción (rwnd), que representa la cantidad de espacio libre disponible en su búfer de recepción en un momento dado. Para comunicar este límite al emisor, el receptor coloca el valor actual de rwnd en el campo ventana de recepción de la cabecera de cada segmento TCP que envía de vuelta al emisor. De este modo el emisor sabe cuántos datos puede enviar sin saturar al receptor

<mark style="background: #CACFD9A6;">¿Que problema resuelve?</mark>
Resuelve el problema del desbordamiento del búfer del receptor. Ocurre cuando un emisor envía datos demasiado rápido en comparación con la velocidad a la que la aplicación receptora procesa o lee esos datos de su búfer. Funciona como un servicio de ajuste de velocidades, equilibrando la tasa de envío del emisor con la tasa de lectura de la aplicación en el destino.

<mark style="background: #CACFD9A6;">¿Cuanto tiempo dura activo y que situación lo desactiva?</mark>
Es un mecanismo dinámico que se mantiene activo durante toda la vida de la conexión TCP. Si el búfer receptor se llena por completo, este envía un valor de rwnd=0, lo que obliga al emisor a detenerse. 
El emisor permanece a la espera hasta que la aplicación receptora libere espacio en el búfer.
Para evitar quedar bloqueado indefinidamente si el mensaje