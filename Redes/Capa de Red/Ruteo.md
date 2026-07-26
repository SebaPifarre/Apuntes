
### ¿Qué es el ruteo? ¿Porqué es necesario?

Es el proceso a nivel de red que determina la trayectoria de extremo a extremo que siguen los paquetes desde un host emisor hasta un host receptor. Mientras que el forwarding es una acción local dentro de un solo router, el ruteo involucra a todos los routers de una red, los cuales interactúan mediante protocolos para establecer los caminos globales.
Sin ruteo, no habría una forma sistemática de encontrar un camino eficiente a través de la compleja malla de enlaces y routers que conectan a miles de millones de dispositivos.
Los protocolos de ruteo permiten configurar las tablas de re envío de forma automática.
El ruteo jerárquico (mediante sistemas autónomos) permite reducir la complejidad y la memoria necesaria para almacenar información de alcance.
Permite que diferentes organizaciones (como empresas y ISPs) administren sus redes internas a su gusto mientras mantienen la capacidad de conectarse y hablar con otras redes externas mediante protocolos estándar como BGP.

### Ruteo estático y dinámico

<mark style="background: #BBFABBA6;">Estático</mark>
Ventajas:
* Técnicamente es factible configurar todas las tablas de re envío manualmente, lo que eliminaría la necesidad de ejecutar protocolos de ruteo complejos.
Desventajas:
* Requiere que un operador edite manualmente las tablas de re envío de los routers
* Propenso a errores
* Lento al responder a cambios en la topología de la red.

<mark style="background: #BBFABBA6;">Dinámico</mark>
Ventajas:
* Utilizan protocolos especiales para configurar automáticamente las tablas de re envío de cada router.
* Rápidos para adaptarse a cambios en la red.
Desventajas:
* Susceptibles a problemas de ruteo. (routing loops)
* Pueden sufrir de oscilaciones en las rutas.
* A medida que la red crece, el intercambio de información de ruteo puede generar un gasto excesivo de memoria y ancho de banda si no se gestiona de forma jerárquica.

## DHCP

Dynamic Host Configuration Protocol permite a un host obtener (ser alocado) un dirección IP automáticamente. Se puede configurar DHCP para que a un host dado se le asigne la misma IP cada vez que se conecta a la red o se le puede asignar una dirección IP temporal que será distinta cada vez.
Por su capacidad de automatizar la conexión de un host a la red, se le suele referir como un plug-and-play-protocol.
DHCP es un protocolo cliente-servidor. Un cliente es normalmente un nuevo host esperando para obtener información de configuración de red. Cada sub red suele tener un servidor DHCP, si no hay servidor DHCP presente en la sub red, un DHCP relay agent (generalmente un router) que conoce  la dirección de un servidor DHCP es necesario.
En general el relay agent envía mensajes Offer de manera unicast, pero como pueden existir equipos que no procesan mensajes unicast antes de tener configurada la dirección IP completa, se podrían enviar por broadcast.
Para un nuevo host, el protocolo DHCP es un proceso de cuatro pasos:
1) <u>DHCP server <b>discovery</b></u>: La primera tarea del host es encontrar un servidor DHCP para interactuar. Esto se logra usando un mensaje DHCP discover, enviado por el cliente en un paquete UDP por el puerto 67. El cliente crea un datagrama IP que contiene su mensaje discover DHCP junto con la dirección destino de broadcast 255.255.255.255 y una "dirección origen" 0.0.0.0.
2) <u>DHCP server offer(s)</u>: Un servidor DHCP recibiendo el mensaje discover responde con un mensaje DHCP offer a todo los nodos de la sub red, usando nuevamente la IP de broadcast. Dado que varios servidores DHCP pueden estar presentes en una sub red, el cliente está en la posición privilegiada de poder elegir entre todas las ofertas. <mark style="background: #FFF3A3A6;">Cada mensaje de oferta contiene la transacción ID del mensaje discover recibido, la IP propuesta para el cliente, la máscara de la red y el tiempo de contrato (IP address lease time), el tiempo por el cual la dirección IP será válida. </mark>
3) <u>DHCP request</u>: El nuevo cliente deberá elegir y responder a la oferta elegida con un mensaje DHCP request, enviando nuevamente los parámetros de configuración.
4) <u>DHCP ACK</u>: El servidor responde al mensaje request con un mensaje DHCP ACK, confirmando los parámetros solicitados.

## NAT

Network Address Translation, traslación de direcciones de un espacio privado (no "enrutable" en internet) a un espacio público.
Como las IP privadas no son únicas generan problemas.
* Las rutas pueden ser confundidas.
* Habitualmente son filtradas por routers de borde.
* Algunos protocolos no funcionan adecuadamente, FTP, VolP, etc.

El router con NAT habilitado no se "ve" como un router al resto del mundo, se comporta como un único dispositivo con una única dirección. 
El router obtiene su dirección IP del servidor DHCP de su ISP y el router tiene su propio servidor DHCP para proveer las direcciones a los hots dentro de la red.
Como todos los datagramas que llegan al router NAT tienen la misma IP de destino, se utiliza la tabla NAT de traslación que contiene tanto direcciones IP como números de puertos. 

## ICMP

Internet Control Message Protocol es usado por hots y routers para comunicar información de capa de red entre ellos. 
El uso típico de ICMP es para reporte de errores.
Suele ser considerado parte de IP pero en la arquitectura yace justo arriba de IP, dado que los mensajes ICMP son llevados dentro de los datagramas IP.
No es un protocolo de transporte ya que no fue concebido para llevar datos de usuario.
Los mensajes ICMP tiene campos de tipo y de código, también contienen el header y los primeros 8 bytes del datagrama IP que causó la generación del mensaje en un principio.

![[Formato Mensaje ICMP.png]]

### ICMP PING (Echo)

* Pensado para probar conectividad IP entre dos hosts.
* Sirve para medir RTT min/avg/max/dev y loss, de esta forma poder diagnosticar problemas.
* Packet INternet Gopher, el nombre basado en el sonido de un sonar de un submarino al escanear.
* Si un nodo recibe un ICMP Echo Request, debe responder copiando el contenido con un Echo Replay (PONG) 
* Actualmente, muy desprestigiado. Se filtra.

![[ICMP PING.png]]

### ICMP Destino Inalcanzable

Para indicar que una red, un host, un puerto es inalcanzable, diferentes causas:
* Host Inalcanzable (Host Unreachable)
	* No está encendido el host, no responde ARP, etc
* Red Inalcanzable (Network Unreachable)
	* No tiene el router una ruta en la tabla de ruteo a esta red.
* Puerto Inalcanzable (Port Unreachable)
	* No hay un proceso UDP en el puerto
* Los mensajes requieren fragmentación
* El mensaje fue filtrado (admin)
* Otros.
![[ICMP Destino Inalcanzable.png]]

### ICMP TTL Expirado

El tiempo de vida ha expirado. En realidad es el hop count con el cual salió el mensaje ha expirado.
Time exceeded:
* Tiempo excedido en viaje
* Tiempo excedido en re-ensamblado
TTL en IP, no solo ICMP
Valor máximo de TTL=255
Puede salir con otro valor
Si TTL == 0, pero ya llegó a la red destino debería enviarse
Utilizando 