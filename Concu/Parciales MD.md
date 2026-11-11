
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


```