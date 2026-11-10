
## Ejercicio 1

En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera impresora que se encuentre libre:
a) Implemente un programa que permita resolver el problema anterior usando PMA.
b) Resuelva el mismo problema anterior pero ahora usando PMS.

a) PMA

```
chan Reporte(texto)
chan Empleado[100](texto)
chan Disponible(int)

Process Empleado[id:1..100]
{
	texto:R,res
	{
		send Reporte(R)
		receive Empleado[id] (res)
	}
}

Process Coordinador
{
	id:int

	while(true)
	{
		receive Disponible(id)
		
	}
}
```