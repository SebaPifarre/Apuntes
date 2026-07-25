
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
* Controles d