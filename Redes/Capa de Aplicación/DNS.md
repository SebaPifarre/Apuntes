
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

* Registros A (Adress)