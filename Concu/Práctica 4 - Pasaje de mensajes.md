
![[pma consideraciones.png]]

## Ejercicio 1

Suponga que N clientes llegan a la cola de un banco y que serán atendidos por sus
empleados. Analice el problema y defina qué procesos, recursos y canales/comunicaciones
serán necesarios/convenientes para resolverlo. Luego, resuelva considerando las siguientes
situaciones:
a. Existe un único empleado, el cual atiende por orden de llegada.
b. Ídem a) pero considerando que hay 2 empleados para atender, ¿qué debe
modificarse en la solución anterior?
c. Ídem b) pero considerando que, si no hay clientes para atender, los empleados
realizan tareas administrativas durante 15 minutos. ¿Se puede resolver sin usar
procesos adicionales? ¿Qué consecuencias implicaría?

a)
```
chan llegaCliente

Process Cliente[id:0..N-1]
{
	send llegaCliente()
	receive 
}

Process Empleado
{
	while(true)
	{
		recieve(llegaCliente)
	}
}
```

Está bien este nivel de abstración?

b)
```
chan llegaCliente

Process Cliente[id:0..N-1]
{
	send(llegaCliente)
}

Process Empleado[id:0..1]
{
	while(true)
	{
		recieve(llegaCliente)
	}
}
```