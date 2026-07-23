
#### ¿Qué es una red? ¿Cuál es el principal objetivo para construir una red?

Una red es un sistema que permite compartir información. Es un conjunto de equipos, dispositivos o entidades inter conectadas entre sí que comparten información, recursos y servicios mediante protocolos de comunicación, ya sea de forma física (cables) o inalámbrica. El principal objetivo para construir una red es compartir recursos e información entre dispositivos de forma eficiente, rápida y segura.

#### ¿Qué es Internet? Describa los principales componentes que permiten su funcionamiento.

Internet es una red de computadoras que inter conecta billones de dispositivos en el mundo. Todos los dispositivos son hosts o end systems. Los end systems están conectados por una red de communication links y packet switches. Diferentes links pueden transmitir datos a diferentes transmission rate. Cuando un end system tiene datos para mandar a otro end system, el end system que envía segmenta los datos y agrega los header bytes a cada segmento. Los resultantes "paquetes" de información son enviados a través de la red al end system destino, donde son re ensamblados en los datos originales.

#### ¿Qué son las RFCs?

Es importante que todo el mundo este de acuerdo en lo que cada protocolo hace para que la gente pueda crear sistemas y productos que interoperan. Aquí es donde entran los standards. Internet Standards son desarrollados por Internet Engineering Task Force (IETF). Los documentos del IETF son llamados requests for comments (RFCs). Definen protocolos tales como TCP, IP, HTTP y SMTP.

#### ¿Qué es un protocolo?

Un protocolo define el formato y el orden de los mensajes intercambiados entre dos o más entidades de comunicación y también las acciones tomadas en la transmisión y/o recepción de un mensaje o evento.

#### ¿Por qué dos máquinas con distintos sistemas operativos pueden formar parte de una misma red?

Dos máquinas con distintos SO pueden formar parte de una misma red porque la comunicación en internet no depende del SO interno, sino del uso de protocolos de red estandarizados.

#### ¿Cuáles son las 2 categorías en las que pueden clasificarse a los sistemas finales o End Systems? Dé un ejemplo del rol de cada uno en alguna aplicación distribuida que corra sobre Internet

Los hosts se dividen en dos categorías: clientes y servidores. Informalmente los clientes tienden a ser PCs de escritorio o smartphones, mientras que los servidores tienden a ser máquinas más poderosas que guardan y distribuyen páginas web, streaming, etc. La mayoría de los servidores están en grandes data centers.

#### ¿Cuál es la diferencia entre una red conmutada de paquetes de una red conmutada de circuitos?

	Un red conmutada de paquetes usa transmisión store-and-forward en los inputs de los links. Esto significa que el packet switch debe recibir el paquete entero antes de empezar a transmitir el primer bit del paquete en el outbound link.
	En una red conmutada de circuitos, los recursos necesarios en el camino para proveer comunicación entre los end systems son reservados por la duración de la sesión.
	La red de paquetes tiene el problema de que puede congestionarse y genera un delay, también al tener un buffer limitado puede ocurrir packet loss. Mientras que la red de circuitos reserva el uso del transmition link sin importar la demanda, pudiendo haber mucho tiempo en el que no se envía información.
	La tendencia indica que la red de paquetes es más eficiente y las ventajas de la red de circuitos solo se aprovechan en casos muy particulares.

