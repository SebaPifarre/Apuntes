
## Introducción

### ¿Qué servicios presta la capa de red? ¿Cuál es la PDU en esta capa? ¿Qué dispositivo es considerado sólo de la capa de red?

La función fundamental dela capa de red es mover paquetes desde un host emisor hasta un host receptor. Para lograr esto, realiza dos funciones clave:
* <u>Reenvío (Forwarding)</u>: Es la acción local de transferir un paquete desde una interfaz de enlace de entrada a la interfaz de enlace de salida apropiada dentro de un solo router.
* <u>Enrutamiento (Routing)</u>: Es el proceso a nivel de red que determina la ruta o camino de extremo a extremo que siguen los paquetes desde el origen al destino, utilizando algoritmos de enrutamiento.

La Unidad de Datos de Protocolo (UDP) se denomina datagrama y su dispositivo característico es el router.
Conocidos como dispositivos de capa 3.
A diferencia de los hosts (que implementan las 5 capas de protocolos), los routers tienen una pila truncada que incluye únicamente las capas inferiores: física, de enlace y de red.
No ejecutan protocolos de las capas superiores, centrándose exclusivamente en los campos de la capa de red. 

### ¿Por qué se lo considera un protocolo de mejor esfuerzo?

La capa de red de Internet ofrece un único servicio denominado best-effort-service el cuál no garantiza la entrega de los paquetes, ni que lleguen en orden, ni que se mantenga el tiempo entre ellos.


### ¿Cuántas redes clase A, B y C hay? ¿Cuántos hosts como máximo pueden tener cada una?

![[Clases de Direcciones IP.png]]

### ¿Qué son las subredes? ¿Por qué es importante siempre especificar la máscara de subred asociada?

Una subred es una red aislada que interconecta interfaces de hosts y de routers.

<u>Interconexión</U>: Es una subred, las interfaces están conectadas entre si mediante un medio que no contiene routers (como un switch de Ethernet o un punto de acceso inalámbrico).
<u>Direccionamiento</u>: Todos los dispositivos conectados a una misma sub red comparten la misma porción inicial de su dirección IP.

La máscara de sub red es fundamental porque define la jerarquía de la dirección IP y permite el funcionamiento del enrutamiento en Internet.
1) Define el prefijo de red -> La máscara indica cuántos de los 32 bits de una dirección IP corresponden a la porción de red.
2) Permite el forwarding -> Los routers utilizan la máscara para procesar los paquetes. Cuando llega un datagrama, el router examina s