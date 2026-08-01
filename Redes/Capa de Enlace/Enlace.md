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

