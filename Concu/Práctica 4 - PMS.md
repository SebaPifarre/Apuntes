![[Consideraciones PMS.png]]

## Ejercicio 1

Suponga que existe un antivirus distribuido que se compone de R procesos robots
Examinadores y 1 proceso Analizador. Los procesos Examinadores están buscando
continuamente posibles sitios web infectados; cada vez que encuentran uno avisan la
dirección, esperan la respuesta del Analizador, y luego continúan buscando. El proceso
Analizador se encarga de hacer todas las pruebas necesarias con cada uno de los sitios encontrados por los robots para determinar si están o no infectados, para luego responderle al Examinador.
a) Analice el problema y defina qué procesos, recursos y comunicaciones serán
necesarios/convenientes para resolverlo.
b) Implemente una solución con PMS donde el Analizador no considere el orden
de los pedidos de los Examinadores.
c) Implemente una solución con PMS donde el Analizador sí considere el orden de
los pedidos de los Examinadores.
d) Si los Examinadores no deben esperar la respuesta del Analizador, ¿cambiaría
algo de la solución al inciso b)? ¿por qué?

a)
Voy a tener los R procesos Examinadores y el proceso Analizador.
Los examinadores van a usar su canal reporte para enviar el sitio web y su id, luego van a esperar en sus canales respuesta.
Mientras el procesos administrador va a recibir por los canales reporte, tomando cualquiera que llegue, van a procesar la consulta y luego responder al id que les llego por el canal.

b)
```
Process Examinador[id:0..R-1]
{
	text sitio, res

	while(true)
	{
		// Buscar sitio infectado
		Analizador!reporte(sitio, id)
		Analizador?respuesta(res)
	}
}

Process Analizador
{
	text pedido, res
	int id

	while(true)
	{
		Examinador[*]?reporte(pedido,id)
		// resuleve la consulta
		Examinador[id]!respuesta(res)
	}
}
```

c)
```
Process Examinador[id:0..R-1]
{
	text sitio, res

	while(true)
	{
		// Buscar sitio infectado
		Analizador!reporte(sitio, id)
		Analizador?respuesta(res)
	}
}

Process Analizador
{
	text pedido, res
	int id

	while(true)
	{
		Examinador[*]?reporte(pedido,id)
		// resuleve la consulta
		Examinador[id]!respuesta(res)
	}
}

Process Admin
{
	cola C
	
	while(true)
	{
		do Examinador[*]?reporte(pedido, id) -> push(C, pedido, id)
		  not empty(C); Analuzador?listo() -> pop(C, )
	}
}
```