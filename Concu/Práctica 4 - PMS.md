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
	while(true)
	{
		// prepara muestra
		Admin!muestreo(muestra)
	}
}

Process Empleado2
{
	text muestra, analisis, res
	while(true)
	{
		Admin!listo()
		Admin?muestreo(muestra)
		analisis = crearAnalisis(muestra)
		Empleado3!analisis(analisis)
		Empleado3?resultado(res)
	}
}

Process Empleado3
{
	text muestra, res

	while(true)
	{
		Empleado2?analisis(muestra)
		// realiza analisis
		Empleado2.resultado(res)
	}
	
}

Process Admin
{
	cola C(text)
	text muestra, res

	do Empleado1?muestreo(muestra) -> push(C, muestra)
	  not empty(C); Empleado2?listo() -> pop(C, muestra)
										  Empleado2!muestreo(muestra)
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
terminar su ejecución.

a)

```
Process Alumno[id:0..N-1]
{
	text examn, res

	//resuelve examn
	Admin!entrega(examn, id)
	Profesor?resultado(res)
}

Process Profesor
{
	text examn, correccion
	int id

	for i=1 to N
	{
		Admin!aviso()
		Admin?proximo(examn, id)
		//corrige
		Alumno[id]!resultado(correccion)
	}
}

Process Admin
{
	text examn
	int id, cant=0
	cola C


		do cant<N; Alumno[*]?entrega(examn, id) -> push(C, examn, id)
													cant++
		  not empty(C); Profesor?aviso() -> pop(C, examn, id)
											 Profesor!proximo(examn, id)
}
```

b)

```
Process Alumno[id:0..N-1]
{
	text examn, res

	//resuelve examn
	Admin!entrega(examn, id)
	Profesor[*]?resultado(res)
}

Process Profesor[id:0..P-1]
{
	text examn, correccion
	int idN

	while(true)
	{
		Admin!aviso(id)
		Admin?proximo(examn, idN)
		if(examn=='fin') break
		//corrige
		Alumno[idN]!resultado(correccion)
	}
}

Process Admin
{
	text examn
	int idN, idP, cant=0, fin=0
	cola C


		do cant<N; Alumno[*]?entrega(examn, id) -> push(C, examn, id)
													cant++
		  not empty(C); Profesor[*]?aviso(idP) -> pop(C, examn, id)
											 Profesor[idP]!proximo(examn, id)
		[] ( cant == N and empty(C) and fin<P) ; Profesor[*]?aviso(idP) ->
												Profesor[idP]!proximo('fin', id)
												fin++
}
```

c)
```
Process Aula
{
	for i=0 to N-1
	{
		Alumno[*]?llegue()
	}
	for i=0 to N-1
	{
		Alumno[i]!empezar()
	}
}

Process Alumno[id:0..N-1]
{
	text examn, res
	
	Aula!llegue()
	Aula[id]?empezar()
	//resuelve examn
	Admin!entrega(examn, id)
	Profesor[*]?resultado(res)
}

// el resto igual que el anterior

```

## Ejercicio 4

En una exposición aeronáutica hay un simulador de vuelo (que debe ser usado con
exclusión mutua) y un empleado encargado de administrar su uso. Hay P personas que
esperan a que el empleado las deje acceder al simulador de a una por vez, la usan por un
rato y luego se retiran.
a) Implemente una solución donde el empleado sólo se ocupa de garantizar la exclusión
mutua (sin importar el orden).
b) Modifique la solución anterior para que el empleado los deje acceder según el orden de
su identificador (hasta que la persona i no lo haya usado, la persona i+1 debe esperar).
c) Modifique la solución a) para que el empleado considere el orden de llegada para dar
acceso al simulador.
Nota: cada persona usa sólo una vez el simulador

a)

```
Process Persona[id:0..P-1]
{
	Empleado!llegue()
	//usa el simulador
	Empleado!termine()
}

Process Empleado
{
	int cant = 0
	bool ocupado = false
	
	do (cant<P and ocupado==false); Persona[*]?llegue() -> ocupado = true
														   cant++
	[] ocupado==true; Persona[*]?termine() -> ocupado = false
}
```

b)

```
Process Persona[id:0..P-1]
{
	Empleado!llegue()
	//usa el simulador
	Empleado!termine()
}

Process Empleado
{
	int i
	
	for i=0 to P-1
	{
		Persona[i]?llegue()
		Persona[i]?termine()
	}
}
```

c)

```
Process Persona[id:0..P-1]
{
	Admin!llegue(id)
	Empleado?comenzar()
	//usa el simulador
	Empleado!termine()
}

Process Admin
{
	cola C(int)
	int id, cant=0
	
	do cant<P; Persona[*]?llegue(id) -> push(C, id)
										cant++
	[] not empty(C); Empleado?aviso() -> pop(C,id)
										 Empleado!siguiente(id)
}

Process Empleado
{
	int i,id
	
	for i=0 to P-1
	{
		Admin!aviso()
		Admin?siguiente(id)
		Persona[id]!comenzar()
		Persona[id]?termine()
	}
}
```

## Ejercicio 5

En un estadio de fútbol hay una máquina expendedora de gaseosas que debe ser usada por
E Espectadores de acuerdo con el orden de llegada. Cuando el espectador accede a la máquina en su turno, usa la máquina y luego se retira para dejar al siguiente. Nota: cada espectador usa la máquina sólo una vez

```
Process Espectador[id:0..E-1]
{
	Admin!llegue(id)
	Maquina?comenzar()
	//uso maquina
	Maquina!finalizar()
}
Process Maquina
{
	int i,id
	
	for i=0 to E-1
	{
		Admin!aviso()
		Admin?proximo(id)
		Espectador[id]!comenzar()
		Espectador[id]?finalizar()
	}
}

Process Admin
{
	int id, cant = 0
	
	do (cant<E); Espectador[*]?llegue() -> push(C, id)
										   cant++
	[] not empty(C); Maquina?aviso() -> pop(C, id)
										Maquina!proximo(id)
}
```