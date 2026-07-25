
### Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El sistema DNS es un sistema distribuido de forma jerárquica, el cual está conformado por muchos servidores a lo largo y ancho del mundo. Cada servidor tendrá la responsabilidad de mantener una parte dentro de la jerarquía de nombres.
El servicio DNS tiene la particularidad que no es utilizado directamente por los usuarios o aplicaciones, sino que funciona como un apoyo al resto de los servicios y sistemas de Internet.

### ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

Un root server provee las direcciones IP de los servidores TLD. Se encargan de delegar cada una de las zonas generadas para los TLD, tanto gTLD como ccTLD. La delegación consiste en saber las direcciones IP de los servidores que se encargan de resolver (o sub-delegar) las zonas de manera autoritativa. Un servidor autoritativo tiene toda la información para una zona, puede producir cambios sobre la misma y es el que tiene la última versión.
gLTD (generic Top Level Domain) contiene los dominios con propósitos particulares de acuerdo a diferentes actividades (.com, .net, .org).

### ¿Qué es una respuesta del tipo autoritativa?

Una respuesta autoritativa es una respuesta que viene directamente desde el servidor DNS autoritativo que alberga oficialmente el registro DNS para un hostname. En el protocolo DNS, la sección header del mensaje contiene una flag autoritativa de un bit. Este bit está seteado en 1 cuando el servidor DNS que provee la respuesta es la fuente autoritativa del nombre buscado.

### ¿Qué diferencia una consulta DNS recursiva de una iterativa?

En una consulta recursiva la consulta se va delegando desde el servidor local hasta el servidor autoritativo. En cambio, en la iterativa, el local se encarga de la consulta, realizando sub-consultas a cada uno de los servidores.

### ¿Qué es el resolver?

El resolver se lo podría considerar como un agente encargado de resolver los nombres a solicitar del cliente. Este agente, generalmente, no se implementa como un servicio activo, sino como un conjunto de rutinas encapsuladas en una biblioteca de funciones que se link-edita conjuntamente con la aplicación.
También se le llama al servidor DNS local resolver.

### Registros

* <mark style="background: #FF5582A6;">Registros A (Address)</mark>
	* Son registros que mapean de un nombre de dominio a una dirección IP. Son los más comunes. Pueden existir varios registros A con el mismo nombre, por ejemplo para realizar balance de carga de un servidor muy accedido.
	
* <mark style="background: #FFB8EBA6;">Registros MX (Mail Exchanger)</mark>
	* Son registros que indican, para un nombre de dominio, cuáles son los servidores de mail SMTP encargados de recibir los mensajes para ese dominio. De esta forma, no es necesario especificar el servidor completo de mail donde se encuentra la casilla destino, alcanza con indicar solamente el dominio. El servidor de mail SMTP que envía el mensaje deberá consultar, vía el servicio DNS, cuáles son los servidores y asignarles prioridades. Los valores más bajos son mejores prioridades. Así, al momento de hacerse el envío del mensaje, si el servidor primario no está activo, se podría recurrir a otro de menor prioridad para enviar el email.
	
* <mark style="background: #FFB86CA6;">Registros PTR (Pointer)</mark>
	* Estos son registros que mapean direcciones IP a nombres de dominio. Son el inverso de los registros A. Estos registros deben estar en un sub-árbol (dominio) separado que se llama in-addr.arpa.
	* Esto se debe a que la búsqueda se realiza usando la dirección IP y no el nombre. Aunque la información existe en los registros directos A, no se puede generar un mecanismo de búsqueda organizado, ya que dad una dirección IP no hay forma de saber en donde está asignada. Para este propósito está el sub-árbol in-addr.arpa que organiza las direcciones por octeto de las direcciones IP, generando un árbol. De esta manera se provee un mecanismo a nodo de índice de búsqueda. 

* <mark style="background: #BBFABBA6;">Registros AAAA</mark>
	* El registro AAAA es el equivalente para IPv6. Se llama cuádruple A porque una dirección IPv6 (128 bits) es cuatro veces más larga que una dirección IPv4 (32 bits). Su función es devolver la dirección IPv6 de 128 bits asociada a un nombre de dominio cuando un cliente realiza una consulta DNS.

* <mark style="background: #ABF7F7A6;">Registro SRV (Service)</mark>
	* Los registros SRV se usan para localizar servicios (host y puerto) ofrecidos por un dominio.

* <mark style="background: #D2B3FFA6;">Registro NS (Name Server)</mark>
	* Los registros NS indican los servidores de nombre autoritativos para una zona o sub-dominio. A partir de esto, se puede lograr una delegación de sub-dominios. A diferencia de los registros MX, estos no llevan asociados una prioridad. Todos los servidores tienen la misma precedencia.
	* Para lograr un mejor balance, al ir respondiendo se debería ir rotando el orden oc el que se entregan los servidores autoritativos para una zona. Al tener varios servidores para un mismo dominio no es necesario configurar a todos con los mismos datos. El software de DNS permite asignar roles de Servidor Primario y Servidor(es) Secundario(s). De esta forma solo se requiere configurar el primario y luego el secundario obtendra una copia de la base de datos del primario.

* <mark style="background: #CACFD9A6;">Registro CNAME</mark>
	* Si el registro es de tipo=CNAME, el campo Value es el nombre de host canónico para el nombre de alias indicado en el campo Name. Se utiliza para proporcionar a los hosts que realizan consultas el nombre verdadero (canónico) de un servidor que utiliza uno o varios alias mnemónicos. (foo.com, relay1.bar.foo.com, CNAME)

* <mark style="background: #FFF3A3A6;">Registro SOA</mark>
	* Component crítico en las configuraciones reales de DNS. Se utiliza para indicar que un servidor DNS es la fuente de información original (la autoridad) para una zona específica. Contiene datos como el servidor maestro, el correo del responsable y parámetros de tiempo que son vitales para la sincronización entre servidores DNS primarios y secundarios.

* <mark style="background: #BBFABBA6;">Registros TXT</mark>
	* Permiten a los administradores de un dominio insertar texto arbitrario en los registros DNS.


### En Internet, un dominio suele tener más de un servidor DNS, ¿por qué cree que esto es así?

La razón principal por la que un dominio cuenta con una estructura de servidores primario y secundarios es para garantizar la confiabilidad y seguridad de la infraestructura del sistema de nombres de dominio DNS.

### Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son secundarios (o esclavos). ¿Cuál es la razón de que sea
así?