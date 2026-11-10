
## Ejercicio 1

En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera impresora que se encuentre libre:
a) Implemente un programa que permita resolver el problema anterior usando PMA.
b) Resuelva el mismo problema anterior pero ahora usando PMS.

a) PMA

```
chan Reporte(texto)
chan Disponible(int)
chan Impresora[5](texto)

Process Empleado[id:1..100]
{
	texto:R,res
	while(true)
	{
		send Reporte(R)
	}
}

Process Coordinador
{
	idI:int
	R:texto

	while(true)
	{
		receive Disponible(idI)
		receive Reporte(R)
		send Impresora[idI](R)
	}
}

Process Impresora[1..5]
{
	R:texto
	idE:int
	
	while(true)
	{
		send Disponible(id)
		receive Impresora[id](idE,R)
		imprime(R)
	}
}
```

b) PMS

```
Process Admin
{
	C:cola(texto)
	R:texto
	id:int
	
	do Empleado[*]?reporte(R) -> push(C,R)
	[] (not empty(C)); Impresora[*]?disponible(id) ->
							pop(C,R)
							Impresora[id]!copiar(R)
	od
		
}

Process Empleado[id:1..100]
{
	R:texto
	while(true)
	{
		generar(R)
		Admin!reporte(R)
	}	
}

Process Impresora[id:1..5]
{
	R:texto
	while(true)
	{
		Admin!disponible(id)
		Admin?copiar(R)
		copiar(R)
	}
}
```

## Ejercicio 2

Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que
debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la
terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la
terminal.

```
Process Persona[id:1..P]
{
	Terminal!llegue(id)
	Terminal?turno()
	//uso terminal
	Terminal!termine
}

Process Terminal
{
	id:int
	
	do Persona[*]?llegue(id) -> push(C,id)
}
```