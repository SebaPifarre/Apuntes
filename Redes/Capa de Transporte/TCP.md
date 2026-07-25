
### ¿Cuál es la función de la capa de transporte?

Un protocolo de capa de transporte provee comunicación lógica entre procesos de aplicación corriendo en diferentes hosts. Comunicación lógica se refiere a que desde la perspectiva de la aplicación, es como que los hosts corriendo los procesos estuvieron conectados directamente; en realidad los hosts pueden estar en puntas distintas del planeta, conectados vía varios routers y un amplio rango de link types. Los procesos de aplicación usan la comunicación lógica que provee la capa de transporte para enviar mensajes entre ellos, liberandose de los detalles de la infraestructura física usada para transportar los mensajes.
Los protocolos de la capa de transporte son implementados en los end systems pero no en los routers de red. Del lado que envía, la capa de transporte convierte los mensajes de la capa de aplicación en paquetes de capa de transporte conocidos como segmentos.

### Describa la estructura del segmento TCP y UDP.

![[Estructura UDP.png]]

![[Estructura TCP.png]]

### ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

El puerto es el identificador númerico 