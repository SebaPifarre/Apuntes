
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