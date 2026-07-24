
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


