
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
* Lento al responder a cambios

