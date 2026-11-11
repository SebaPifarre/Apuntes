
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
		Admin
		atender(trat)
		Paciente[id]!tratamiento(trat)
}

Process Admin
{
	C:cola
	id:int
	
	do Paciente[*]?llegada(id) -> push(C,id)
	[] not empty(C); Medico?Siguiente -> pop(C,id); Medico!Siguiente(id)
	
}
```