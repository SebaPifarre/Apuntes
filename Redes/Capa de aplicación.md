
### ¿Cuál es la función de la capa de aplicación? ###

La capa de aplicación es donde residen las aplicaciones de red y sus protocolos correspondientes. Su función central es permitir que los programas (proceso) que se ejecutan en distintos sistemas finales (end systems) se comuniquen entre si mediante el intercambio de paquetes de información.

### Si dos procesos deben comunicarse:

* En máquinas diferentes los proceos se comunican mediante el intercambio de mensajes a través de una red informática. El proceso emisor crea y envía estos mensajes a la red, mientras que el proceso receptor los obtiene y puede responder enviando otros mensajes de vuelta. Para interactuar con la red, cada proceso utiliza un interfaz de software denominada socket que funciona de manera análoga a una "puerta" por la cuál salen y entran datos. Para identificar correctamente el proceso receptor en otra máquina, se requieren dos piezas de información: la dirección del host (dirección IP) y un identificador del proceso destino (comúnmente un número de puerto).
* En la misma máquina, cuando los procesos se ejecutan en el mismo end system, pueden comunicarse utilizando mecanismos de comunicación entre procesos (IPC). En este caso, las reglas y protocolos que rigen este intercambio de información están determinados y gobernados por el SO de la máquina.

### Explique brevemente cómo es el modelo Cliente/Servidor. Dé un ejemplo de un sistema Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación

En la arquitectura cliente-servidor existe un host siempre encendido llamado servidor que atiende solicitudes de muchos otros hosts llamados clientes. Un ejemplo clásico es una aplicación web donde un servidor web atiende solicitudes de navegadores corriendo en hosts de clientes. En la arquitectura cliente-servidor, los clientes no se comunican directamente entre ellos. Otra característica es que el servidor tiene un dirección fija llamada IP, como el servidor siempre está encendido y la dirección IP es fija, el cliente siempre puede enviar paquetes al servidor.
Existe también la arquitectura P2P, la cuál usa comunicación directa entre un par de hosts conectados intermitente mente llamados peers.

### Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”.

Un agente de usuario (user agent) es la entidad o programa informático que actúa como interfaz entre el usuario y una aplicación de red, permitiendo al usuario interactuar con los protocolos subyacentes. Esta entidad reside y se ejecuta exclusivamente en los hosts.

### ¿Qué son y en qué se diferencian HTML y HTTP?

* HTML es un estándar para el formato de documentos utilizado para crear páginas web. El navegador interpreta el archivo HTML para mostrar la página al usuario en su pantalla. 
* HTTP es el protocolo de la capa de aplicación de la web (Hyper Text Transfer Protocol). Define la estructura de los mensajes y cómo el cliente (navegador) y el servidor intercambian dichos mensajes para solicitar y transferir páginas web.

### HTTP tiene definido un formato de mensaje para los requerimientos y las respuestas

Lo que determina si un mensaje es de requerimiento o de respuesta es su primera línea.
La primera línea de un mensaje de requerimiento HTTP se llama request line (línea de solicitud), compuesta por tres campos: el método, el URL y la versión de HTTP.
En un mensaje de respuesta, la primera línea se denomina línea de status. Compuesta también por tres campos: la versión del protocolo, un código de estado y el correspondiente mensaje de estado.
Ambos mensajes están escritos en texto ASCII ordinario, para que cualquier humano ordinario con conocimientos de informática pueda leerlos.
Las líneas de cabecera sirven para proporcionar información adicional sobre el mensaje, el remitente o el manejo de conexión.


![[Solicitud HTTP.jpg]]

![[Respuesta HTTP.jpg]]


### ¿Qué indica la cabecera Date?

La cabecera Date indica la fecha y hora cuando la respuesta HTTP fue creada y enviada por el servidor. Notar que no es la fecha y hora cuando el objeto fue creado o modificado, es la cual el servidor obtiene el objeto en su filesystem, inserta el objeto en el mensaje de respuesta y envía el mensaje de respuesta.

### En HTTP/1.0, ¿cómo sabe el cliente que ya recibió todo el objeto solicitado de manera completa? ¿Y en HTTP/1.1?

* En HTTP 1.0 cada conexión TCP transporta exactamente un mensaje de solicitud y uno de respuesta. El cliente solicita el recurso base y cuando lo recibe, lo analiza para saber cuantas peticiones necesita para obtener el objeto solicitado de manera completa.
* En HTTP 1.1 la conexión es persistente por default, lo cual permite envíar una página web completa con una sola conexión TCP persistente. En general, el servidor HTTP cierra la conexión cuando no es usada por un cierto tiempo (intervalo configurable).
	* El servidor incluye la línea de encabezado Content-length en su mensaje de respuesta. Esta cabecera indica explicitamente el número de bytes que contiene el objeto enviado en el cuerpo de la entidad. El cliente cuenta los bytes recibidos y, una vez alcanzadas esa cifra indicada en Content-length, sabe que el objeto está completo y que la conexión sigue disponible para enviar nuevas peticiones.

### Investigue los distintos tipos de códigos de retorno de un servidor web y su significado. Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).

Los códigos de estado de respuesta HTTP indican si se ha completado satisfactoriamente una solicitud HTTP específica. Las respuestas se agrupan en cinco clases:
1) Respuestas informativas (100-199)
2) Respuestas satisfactorias (200-299)
3) Re-direcciones (300-399)
4) Errores del sistema (400-499)
5) Errores de los servidores (500-599)

### Investigue cuál es el principal uso que se le da a las cabeceras Set-Cookie y Cookie en HTTP y qué relación tienen con el funcionamiento del protocolo HTTP.

Cuando un cliente se conecta a un servidor por primera vez, el servidor puede incluir en la respuesta HTTP el encabezado set-cookie que contiene un número de identificación. 
Cuando el browser del cliente recibe la respuesta, ve el set-cookie header. El browser añade una línea al archivo especial de cookies que maneja, esta línea contiene el número de identificación de la cookie y el hostname del servidor.
Cada petición subsecuente a ese mismo servidor el browser extrae el identificador del archivo y lo agrega en un encabezado cookie. De esta manera el servidor puede hacer un seguimiento de las actividades del cliente en la página web sin necesariamente saber la información del cliente.

### ¿Cuál es la diferencia entre un protocolo binario y uno basado en texto? ¿De qué tipo de protocolo se trata HTTP/1.0, HTTP/1.1 y HTTP/2?

La diferencia fundamental entre un protocolo basado en texto y uno binario radica en la forma en que se estructuran y transmiten los mensajes.
