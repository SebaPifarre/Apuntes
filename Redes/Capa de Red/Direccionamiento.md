
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

