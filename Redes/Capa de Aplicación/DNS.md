
### Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El sistema DNS es un sistema distribuido de forma jerárquica, el cual está conformado por muchos servidores a lo largo y ancho del mundo. Cada servidor tendrá la responsabilidad de mantener una parte dentro de la jerarquía de nombres.
El servicio DNS tiene la particularidad que no es utilizado directamente por los usuarios o aplicaciones, sino que funciona como un apoyo al resto de los servicios y sistemas de Internet.

### ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

Un root server provee las direcciones IP de los servidores TLD. Se encargan de delegar cada una de las zonas generadas para los TLD, tanto gTLD como ccTLD. La delegación consiste en saber las direcciones IP de los servidores que se encargan de resolver (o sub-delegar) las zonas de manera autoritativa. Un servidor autoritativo tiene toda la información para una zona, puede producir cambios sobre 