### ¿Qué función cumple la capa de enlace? Indique qué servicios presta esta capa.

#### Terminología
* Hosts y routers son <i>nodes</i>
* Los canales de comunicación que conectan nodos adyacentes a través de caminos de comunicación son <i>links</i>
	* Enlaces cableados
	* Enlaces inalámbricos
	* LANs
* La PDU de capa 2 es el <i>frame</i>, que encapsula un datagrama.

La capa de enlace de datos tiene la responsabilidad de transferir datagramas desde un nodo a otro nodo adyacente, a través de un link.
Mientras que la capa de red tiene el trabajo end-to-end de mover segmentos de la capa de transporte de un host origen a un host destino, un protocolo de capa de enlace tiene el trabajo node-to-node de mover datagramas de la capa de red por un único link en el camino.
Un datagrama puede ser llevado por distintos protocolos de capa de enlace en el mismo camino. 

#### Servicios
* Entramado (framing): 
	* Encapsulado del datagrama en la trama, agregando encabezado (header) y cola (trailer). Distintos protocolos pueden tener distintas estructuras de frames. 

* Acceso al enlace:
	* Acceso al canal si es un medio compartido (Medium Access Control)
	* Direcciones "MAC" utilizadas en los encabezados de las tramas para identificar el origen y destino 

* Entrega confiable:
	* Entre nodos adyacentes
	* Rara vez utilizado en enlaces de pocos errores (fibra óptica, coax)
	* Usado normalmente en enlaces inálambricos con alta tasa de error, con el objetivo de corregir el error de manera local, en vez de usar retrasmiciones

* Control de flujo:
	* Acuerdo entre los nodos emisor y receptor
	* Frame buffers

* Detección de errores:
	* Errores causados por atenuación de la señal, por ruido
	* El receptor detecta presencia de errores
		* Señaliza al emisor para una retransmisión o descarta la trama
	* Similar al de capa de transporte y red pero más sofisticado ya que está implementado en hardware

* Corrección de errores:
	* El receptor identifica y corrige el/los error/es en bit/s sin necesidad de retransmisión

* Half-duplex and full-duplex:
	* Con full-duplex los nodos de ambos lados del link pueden transmitir paquetes al mismo tiempo
	* Con half-duplex, un nodo no puede transmitir y recibir al mismo tiempo

### Compare los servicios de la capa de enlace con los de la capa de transporte.

Ambas capas proveen entrega confiable, control de flujo y detección de errores. La diferencia es que la capa de transporte lo provee entre dos procesos end-to-end y la capa de enlace lo provee entre dos nodos conectados por un único link.

### ¿Cómo se identifican dos máquinas en una red Ethernet?

Una dirección de capa de enlace suele llamarse dirección LAN, dirección física o dirección MAC; siendo la última la más popular (Media Access Control).
Las direcciones MAC tienen un longitud de 6 bytes, normalmente expresada en notación hexadecimal. A pesar de ser diseñadas para ser permanentes, es posible cambiar la dirección MAC de un adaptador via software.
La dirección de broadcast de la capa de enlace es FF-FF-FF-FF-FF-FF, la cuál es necesaria cuando se quiere que todos los adaptadores de la LAN reciban y procesen el frame (ARP).

### Dispositivos de la capa de enlaces

#### Hubs
* Repetidores de la capa física ("tonto")
* los bits que llegan en un link salen por todos los otros links a la misma velocidad
* todos los nodos conectados al hub pueden colisionar con los otros
* no existe buffering the tramas
* no hay CSMA/CD en el hub: la NIC del host detecta las colisiones

#### Switch
* Dispositivo de interconexión que opera exclusivamente en la capa 2
* Más "inteligente" que los hubs, tienen un rol activo
* Recibe tramas, las almacena y las reenvía hacia el enlace de salida correspondiente basándose en la dirección MAC de destino
* Elimina las colisiones al usar buffers para almacenar tramas y no transmitir más de una trama a la vez en un mismo segmento
* Es transparente para los hosts, quienes no saben que un switch está mediando su comunicación.
* Construye su tabla de conmutación de forma automática y dinámica (plug-and-play) sin intervención del administrador.

#### Adaptador de red NIC (Network Interface Card)
* Es el componente de hardware donde se implementa la mayor parte de la capa de enlace dentro de un host
* Encapsula datagramas de la capa de red en tramas de la capa de enlace, añade bits de detección de errores y gestiona el acceso al medio físico.
* Mientras que el switch interconecta múltiples dispositivos, el adaptador es el punto terminal de un nodo que le permite comunicarse físicamente con el canal.

#### Punto de acceso inálambrico
* Es la estación base de una red inálambrica de infraestructura (como el wifi)
* Actúa como un relé de capa de enlace ente los hosts inálambricos y el resto de la red cableada
* El AP debe realizar una conversión de formato entre los protocolos de cpaa de enlace para mover los datos hacia un router

#### Router 
* Aunque es un dispositivo de capa 3, el router también funciona como un nodo de capa de enlace
* Un conmutador reenvía tramas usando direcciones MAC, mientras que un router reenvía datagramas usando direcciones IP
* Los routers no están restringidos a una topología de árbol de expansión y pueden encontrar caminos óptimos entre origen y destino a través de protocolos de enrutamiento.

### ¿Qué es una colisión?

Dos o más transmisiones simultáneas: interferencia
* Si un nodo recibe dos o más señales al mismo tiempo
* Simultaneidad en el tiempo y en la frecuencia de dos o más tramas en el mismo medio físico

<mark style="background: #FF5582A6;">¿Qué dispositivos dividen dominios de broadcast?
¿Qué dispositivos dividen dominios de colisión?</mark>

### Protocolos de acceso múltiple

* Único canal broadcast compartido
* Algoritmo distribuido que determina cómo los nodos comparten el canal, y determinan cuándo el nodo puede transmitir
* La comunicación acerca de compartir el canal debe utilizar el mismo canal

#### Protocolo de acceso múltiple ideal
* Cuando un solo nodo tiene datos para enviar , ese nodo lo hará a una velocidad R
* Cuando M nodos quieren transmitir, cada uno enviará a una velocidad promedio de R/M
* El protocolo es descentralizado, sin nodos maestro que puedan fallar y tirar todo el sistema abajo
* El protocolo es simple para no ser costoso de implementar


### ARP

Como hay tanto direcciones de capa de red (IP) y direcciones de capa de enlace (MAC) hay una necesidad de traducirlas entre ellas. Para internet, este es el trabajo de  Address Resolution Protocol (ARP)
Para enviar un datagrama, el nodo fuente debe proporcionarle al adaptador no solo sel datagrama IP sino también la dirección MAC del nodo destino. Dado el datagrama y la dirección MAC, el adaptador del nodo transmisor construirá un frame de capa de enlace conteniendo la dirección MAC del nodo destino y lo enviará a la LAN.

#### ¿Como determina el nodo transmisor la dirección MAC del nodo destino con la IP?

Usando ARP. Un módulo ARP en el nodo transmisor toma cualquier dirección IP de la misma LAN como input y retorna la correspondiente dirección MAC.
Vemos que ARP resuelve una dirección IP a una dirección MAC, similar a como DNS resuelve un hostname en una dirección IP. Sin embargo, la importante diferencia entre los dos resolvers es que DNS resuelve hostnames para hosts en cualquier lugar de internet, mientras que ARP resuelve direcciones IP solo para los nodos de la misma sub red.

#### Funcionamiento ARP

Cada nodo tiene en su RAM una tabla ARP, que contiene los mapeados de direcciones IP a direcciones MAC. La tabla ARP también contiene un valor de tiempo de vida (TTL), el cual indica cuando cada mapeado será eliminado de la tabla. 
Cuando un nodo quiere enviar un datagrama a un nodo destino del cual aún no tiene su correspondiente entrada en la tabla, primero deberá construir un paquete especial llamado paquete ARP. Este paquete contiene las direcciones IP y MAC de origen y destino. Tanto el ARP query como el response tienen el mismo formato. El propósito del ARP query es consultar a todos los nodos de la red para determinar que dirección MAC corresponde a la dirección IP que está siendo resuelta.
El nodo le pasa el ARP query al adaptador junto con la indicación que lo envíe con dirección MAC destino de broadcast. El adaptador encapsula el paquete en un frame y transmite el frame en la sub red. 
El frame que contiene el ARP query es recibido por todos los adaptadores de la sub red y se lo pasan al módulo ARP en el nodo. Cada nodo verifica si su dirección IP coincide con la dirección IP destino del paquete ARP. El nodo con match envía un paquete ARP response con el mapeado deseado. El nodo transmisor ahora puede actualizar su tabla ARP y puede enviar el datagrama en un frame con MAC destino a la obtenida gracias al ARP query.
Notas:
* El paquete ARP query es enviado dentro de un frame de broadcast mientras que el paquete ARP response es enviado dentro de un frame standard
* ARP es plug-and-play, la tabla ARP del nodo se arma automáticamente. Si un nodo se desconecta de la sub red, su entrada se borrará eventualmente de las tablas.
