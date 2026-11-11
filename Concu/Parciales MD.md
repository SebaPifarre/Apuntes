
# Parcial 2025 – 1ra Fecha (ATIC)

## Ejercicio 1

Resolver con **PASAJE DE MENSAJES ASINCRÓNICOS (PMA)** el siguiente problema. Hay _**E estudiantes**_ que rinden un examen final y _**un profesor**_ que cuando todos los estudiantes han llegado les entrega el enunciado. Luego el profesor va corrigiendo los exámenes y enviando la nota de acuerdo con orden en que se van entregando. Cada alumno realiza el examen, lo entrega y espera a que el profesor le indique la nota. _**Nota:**_ maximizar la concurrencia; no realizar _busy waiting_; todos los procesos deben terminar.

```
chan llegaEstudiante,entregaEnunciado
chan entregaParcial(int,texto)
chan nota[E](int)

Process Estudiante[id:1..E]
{
	res:texto;
	nota:int;

	send llegaEstudiante
	receive entregaEnunciado
	res:=resolver
	send entregaParcial(id,res)
	receive nota[id](nota)
}

Process Profesor
{
	res:texto;
	id,nota:int;
	
	for i=1 to E do
		receive llegaEstudiante
	for i=1 to E do
		send entregaEnunciado
	for i=1 to E do
		receive entregaParcial(id,res)
		nota:=corrige(res)
		send nota[id](nota)
}
```

## Ejercicio 2

Resolver con **PASAJE DE MENSAJES SINCRÓNICOS (PMS)** el siguiente problema. En un consultorio hay _**un médico**_ para atender a _**15 pacientes**_ de acuerdo con el orden de llegada. Cada paciente al llegar espera a que el médico lo atiende y le indique su tratamiento. _**Nota:**_ existe la función _atender()_ que simula que el médico está atendiendo a un paciente; todos los procesos deben terminar.

```
Process Paciente[id:1..15]
{
	trat:texto

	Admin!llegada(id)
	Medico[id]?tratamiento(trat)
}

Process Medico
{
	id:int
	trat:texto

	for i:=1 to 15 do
		Admin!Siguiente
		Admin?Proximo(id)
		atender(trat)
		Paciente[id]!tratamiento(trat)
}

Process Admin
{
	C:cola
	id:int
	cant:int:=0
	
	do cant<15; Paciente[*]?llegada(id) -> push(C,id); cant++
	[] not empty(C); Medico?Siguiente -> pop(C,id); Medico!Proximo(id)
	
}
```

## Ejercicio 3

Resolver con **ADA** el siguiente problema. En una competencia de programadores hay _**P participantes**_ y _**UN coordinador**_. El coordinador entrega el problema a resolver a los participantes y recibe las resoluciones para corregir. Cada participante debe conocer el resultado de su trabajo y el orden en que entregó su resolución. _**Nota:**_ maximizar la concurrencia.

```ada
Procedure ej3 is

Task Coordinador is
	ENTRY Llegada
	ENTRY Inicio(e:OUT texto)
	ENTRY entrega(resolucion:IN texto; resultado:OUT texto; pos:OUT int)
End Coordinador;

Task Type Participante;

arrParti:array(1..P) of Participante;

Task Body Coordinador is
	enunciado,resultado:texto;
	i:int;
Begin
	enunciado:=generarEnunciado();
	for i in 1..P loop
		accept Llegada
		end Llegada;
	end loop;
	for i in 1..P loop
		accept Inicio(e:OUT texto) do
			e:=enunciado;
		end Inicio;
	end loop;
	for i in 1..P loop
		accept entregar(resolucion:IN texto; resultado:OUT texto; pos:OUT int) do
			resultado:=corregir(resolucion);
			pos:=i;
		End entregar;
			
	end loop;
End Coordinador;

Task Body Participante is
	en,resolucion,result:texto;
	pos:int
Begin
	Coordinador.Llegada;
	Coordinador.Inicio(en);
	res:=resolver(en);
	Coordinador.entregar(res,result,pos);
End Participante;
Begin
	null
End ej3;

```

# Programación Concurrente ATIC/Redictado Programación Concurrente – Primera fecha de MD – 10/06/2026 – Tema 2

## Ejercicio 1

Resolver con **PASAJE DE MENSAJES ASINCRÓNICOS (PMA)** el siguiente problema. Existe _**un servidor**_ que resuelve pedidos de análisis de similitud de secuencias de ADN de _**C clientes**_ diferentes. Cada cliente realiza sucesivos pedidos (enviando 2 strings con las secuencias a analizar) y esperando el resultado, los pedidos pueden ser de diferentes prioridades (ALTA, MEDIA o BAJA). El servidor atiende los pedidos de acuerdo con la prioridad (y entre pedidos de una misma prioridad sigue el orden de llegada), devolviendo el resultado al cliente correspondiente (un número que indica el grado de similitud de las 2 secuencias); cada vez que no hay pedidos para resolver el servidor se duerme por 10 minutos. _**Nota:**_ suponga que existe una función _Analizar(sec1, sec2)_ que retorna un entero con el resultado del análisis; el cliente determina la prioridad de cada pedido.

```
chan altaPrioridad(int,text,text),
mediaPrioridad(int,text,text),
bajaPrioridad(int,text,text);

chan Respuesta[C]

Process Cliente[id:1..C]
{
	sec1,sec2,prio:texto;
	grado:int;

	while(true)
	{
		generarSecuencias(sec1,sec2,prio)
		if(prio="ALTA")
			send altaPrioridad(id,sec1,sec2)
		else if(prio="MEDIA")
			send mediaPrioridad(id,sec1,sec2)
		else
			send bajaPrioridad(id,sec1,sec2)
			
		receive Respuesta[id](grado)
	}
}

Process Servidor
{
	grado:int
	
	do  if(not empty(altaPrioridad)) -> {
		receive altaPrioridad(id,sec1,sec2)
		grado:=Analizar(sec1,sec2);
		send Respuesta[id](grado);
	}
	[] if(empty(altaPrioridad) and not empty(mediaPrioridad)) -> {
		receive mediaPrioridad(id,sec1,sec2);
		grado:=Analizar(sec1,sec2);
		send Respuesta[id](grado);
	}
	[] if(empty(altaPrioridad) and empty(mediaPrioridad) and not empty(bajaPrioridad)) -> {
		receive bajaPrioridad(id,sec1,sec2);
		grado:=Analizar(sec1,sec2);
		send Respuesta[id](grado);
	}
	[] if(empty(altaPrioridad) and empty(mediaPrioridad) and empty(bajaPrioridad)) -> {
		delay 600
	}
}

```

## Ejercicio 2

Resolver con **ADA** el siguiente problema. Existen _**3 docentes**_ que atienden consultas online de _**E estudiantes**_ diferentes. Cada estudiante realiza sucesivos pedidos para ser atendido por algún docente, enviando un texto con la consulta y esperando la respuesta. Los docentes atienden los pedidos de acuerdo con el orden en que se realizaron las consultas, devolviendo la respuesta al estudiante correspondiente. _**Nota:**_ suponga que existe una función _AtenderConsulta(cons)_ que retorna un texto con la respuesta a la consulta; cada consulta cuando le llegue su turno debe ser atendida por el primer docente que esté disponible.

```ada
Procedure ej2 is

Task Admin is
	ENTRY Siguiente(id:OUT int; c:OUT texto);
	ENTRY Consulta(id:IN int; c:IN texto);
End Admin;

Task Type Docente;
Task Type Estudiante is
	ENTRY Respuesta(res:IN texto);
	ENTRY Ident(id:IN int);
End Estudiante;

arrDocente:array(1..3) of Docente;
arrEstudiante:array(1..E) of Estudiante;

Task Body Estudiante is
	c,resultado:texto;
	miid:int;
Begin
	accept Ident(id:IN int) do
		miid:=id;
	end Ident;
	Admin.Consulta(miid, c);
	accept Respuesta(res:IN texto) do
		resultado:=res;
	end Respuesta;
End Estudiante;

Task Body Docente is
	id:int;
	consulta,res:texto;
Begin
	loop
		Admin.Siguiente(id, consulta);
		res:=AtenderConsulta(consulta);
		Estudiante[id].Respuesta(res);
	end loop;
End Docente;

Task Body Admin is
	C:cola(int,texto);
	consulta:texto;
	prox:int;
Begin
	loop
		SELECT
			accept Consulta(id:IN int; c:IN texto) do
				push(C,id,c);
			end Consulta;
		OR
			when(not empty(C)) => accept Siguiente(id:OUT int; c:OUT texto) do
				pop(C,prox,c);
				id:=prox;
				c:=consulta;
			end Siguiente;
	end loop;
End Admin;
Begin
	for i in 1..E loop
		arrEstudiante(i).Ident(i);
	end loop;
End ej2;
```