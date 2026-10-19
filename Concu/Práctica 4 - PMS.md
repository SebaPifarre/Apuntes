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
		Admin!reporte(sitio, id)
		Analizador?respuesta(res)
	}
}

Process Analizador
{
	text pedido, res
	int id

	while(true)
	{
		Admin!listo()
		Admin?envio(pedido, id)
		// resuleve la consulta
		Examinador[id]!respuesta(res)
	}
}

Process Admin
{
	cola C
	text pedido
	int id

	do Examinador[*]?reporte(pedido, id) -> push(C, pedido, id)
	  not empty(C); Analizador?listo() -> pop(C, pedido, id)
											Analizador!envio(pedido, id)
}
```

d)
```
Process Examinador[id:0..R-1]
{
	text sitio, res

	while(true)
	{
		// Buscar sitio infectado
		Admin!reporte(sitio)
	}
}

Process Analizador
{
	text pedido, res
	int id

	while(true)
	{
		Admin!aviso()
		Admin?consulta(pedido)
		// resuleve la consulta
	}
}

Process Admin
{
	text sitio, peticion
	cola C(text)

	do Examinador[*]?reporte(sitio) -> push(C, sitio)
	  not empty(C); Analizador?aviso -> pop(C, peticion)
											Analizdor!consulta(peticion)
	
}
```

## Ejercicio 2

En un laboratorio de genética veterinaria hay 3 empleados. El primero de ellos
continuamente prepara las muestras de ADN; cada vez que termina, se la envía al segundo
empleado y vuelve a su trabajo. El segundo empleado toma cada muestra de ADN
preparada, arma el set de análisis que se deben realizar con ella y espera el resultado para
archivarlo. Por último, el tercer empleado se encarga de realizar el análisis y devolverle el
resultado al segundo empleado.

```
Process Empleado1
{
	// prepara muestra
	Empleado2!muestreo(muestra)
}

Process Empleado3
{
	text muestra, res

	Empleado2!listo()
	Empleado2?analisis(muestra)
	// realiza analisis
	Empleado2.resultado(res)
}

Process Empleado2
{
	cola C(text)
	text muestra, res

	do Empleado1?muestreo(muestra) -> push(C, muestra)
	  not empty(C); Empleado3?listo() -> pop(C, muestra)
										  Empleado3!analisis(muestra)
	  Empleado3?resultado(res) // guarda res
}
```

## Ejercicio 3

En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo
entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los
profesores corrigen los exámenes respetando el orden en que los alumnos van entregando.
a) Considerando que P=1.
b) Considerando que P>1.
c) Ídem b) pero considerando que los alumnos no comienzan a realizar su examen hasta
que todos hayan llegado al aula.
Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben
terminar su ejecución