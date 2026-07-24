
### ¿Cuál es la función de la capa de aplicación? ###

La capa de aplicación es donde residen las aplicaciones de red y sus protocolos correspondientes. Su función central es permitir que los programas (proceso) que se ejecutan en distintos sistemas finales (end systems) se comuniquen entre si mediante el intercambio de paquetes de información.

### Si dos procesos deben comunicarse:

* En máquinas diferentes los proceos se comunican mediante el intercambio de mensajes a través de una red informática. El proceso emisor crea y envía estos mensajes a la red, mientras que el proceso receptor los obtiene y puede responder enviando otros mensajes de vuelta. Para interactuar con la red, cada proceso utiliza un interfaz de software denominada socket que funciona de manera análoga a una "puerta" por la cuál salen y entran datos. Para identificar correctamente el proceso receptor en otra máquina, se requieren dos piezas de información: la dirección del host (dirección IP) y un identificador del proceso destino (comúnmente un número de puerto).
* En la misma máquina, cuando los procesos se ejecutan en el mismo end system, pueden comunicarse utilizando mecanismos de comunicación entre procesos (IPC). En este caso, las reglas y protocolos que rigen este intercambio de información están determinados y gobernados por el SO de la máquina.

### Explique brevemente cómo es el modelo Cliente/Servidor. Dé un ejemplo de un sistema Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación

En la arquitectura 

