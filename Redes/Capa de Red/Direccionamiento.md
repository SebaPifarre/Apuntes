
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
2) Permite el forwarding -> Los routers utilizan la máscara para procesar los paquetes. Cuando llega un datagrama, el router examina solo los bits de prefijo indicados por la máscara para buscar un coincidencia en si tabla de re envío y determinar la interfaz de salida correcta
3) Habilita la agregación de rutas -> Gracias a la máscara, un ISP puede anunciar un bloque completo de direcciones. Esto reduce considerablemente el tamaño de las tablas de re envío en los routers de la red troncal de Internet, ya que una sola entrada sirve para dirigir paquetes a cualquier destino dentro de esa organización.
4) Configuración de DHCP -> Protocolos como DHCP proporcionan automáticamente la máscara de subred a los hosts. Es vital que un host conozca su propia máscara para determinar si una dirección de destino está dentro de su misma subred o si el paquete debe ser enviado al router predeterminado para salir a otra red.

### ¿Cuál es la finalidad del campo Protocol en la cabecera IP? ¿A qué campos de la capa de transporte se asemeja en su funcionalidad?

El campo Protocol se utiliza exclusivamente cuando un datagrama IP alcanza su destino final. Su función principal es identificar el protocolo específico de la capa de transporte al que debe entregarse la parte de datos (carga útil) del datagrama. Funciona como un mecanismo de demultiplexación en el host receptor, permitiendo que la capa de red sepa si debe pasar el segmento contenido en el datagrama a TCP (valor 6), a UDP (valor 17) o a otro protocolo compatible.
El campo Protocol se asemeja directamente a los campos de número de puerto de la capa de transporte. Mientras que el número de puerto identifica el proceso o socket específico dentro de un host, el número de protocolo identifica el protocolo de transporte correcto dentro del sistema operativo del host receptor.

## CIDR

### ¿Qué es CIDR? ¿Por qué resulta útil?

CIDR (Class Interdomain Routing) es un método para asignar direcciones IP de forma más flexible, reemplazando el sistema antiguo de clases fijas (A,B,C).
* Ahorra direcciones IP -> das exactamente lo que necesitas
* Permite sumarización -> varios bloques se pueden anunciar como uno solo (supernetting)
* Reduce el tamaño de las tablas de enrutamiento -> los routers trabajan más eficientemente.

## VLSM

### ¿Qué es y para qué se usa VLSM?

Variable Length Subnet Mask es una técnica que permite dividir una red en subredes de distinto tamaño, asignando a cada una exactamente los hosts que necesita, sin desperdiciar direcciones.
Pasos:
1) Listar todas las subredes ordenadas de mayor a menor.
2) Asignar la primera subred: Tomás el bloque original y le asignás la máscara mínima ncesaria para cubrir los hosts requeridos.
3) Asignar la siguiente subred empezando justo donde termino la anterior (después del broadcast) y se le asigna su propia máscara según sus hosts necesarios.
4) Repetir hasta cubrir todas.

