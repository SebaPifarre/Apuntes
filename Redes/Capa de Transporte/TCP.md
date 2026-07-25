
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
1) El cliente TCP envía un segmento especial al servidor TCP. Este segmento no contiene datos de capa de aplicación. El fla