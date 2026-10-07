
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
chan atender[N]

Process Cliente[id:0..N-1]
{
	send llegaCliente(id)
	receive atender[id]()
}

Process Empleado
{
	int id
	
	while(true)
	{
		receive llegaCliente(id)
		send atender[id]())
		// atiende
	}
}
```

Está bien este nivel de abstración?

b)
```
chan llegaCliente
chan atender[N]

Process Cliente[id:0..N-1]
{
	send llegaCliente(id)
	receive atender[id]()
}

Process Empleado[0..1]
{
	int id
	
	while(true)
	{
		receive llegaCliente(id)
		send atender[id]())
		//atiende
	}
}
```

c) Como son varios empleados no se puede evitar hacer busy waiting. Se necesita un proceso coordinador

## Ejercicio 2

Se desea modelar el funcionamiento de un banco en el cual existen 5 cajas para realizar
pagos. Existen P clientes que desean hacer un pago. Para esto, cada uno selecciona la caja
donde hay menos personas esperando; una vez seleccionada, espera a ser atendido. En cada
caja, los clientes son atendidos por orden de llegada por los cajeros. Luego del pago, se les
entrega un comprobante. Nota: maximizar la concurrencia.

