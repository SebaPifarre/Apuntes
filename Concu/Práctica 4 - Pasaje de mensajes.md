
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

```

chan llegada(int)
chan otorgar[P](int)
chan esperar[5](int)
chan entrega[P](text)

Process Cliente[id:0..P-1]
{
	int nroCola
	text pago, comprobante

	send llegada(id)
	recieve otorgar[id](nroCola)
	send esperar[nroCola](id)
	receive entrega[id](comprobante)
}

Process Coordinador
{
	int aux
	
	while(true)
	{
		recieve llegada(aux)
		cola = obtenerMenosEsperando()
		send otorgar[aux](cola)
	}
}

Process Caja[id:1..5]
{
	int aux
	text comprobante

	while(true)
	{
		recieve esperar[id](aux)
		//realiza el pago
		send entrega[aux](comprobante)
	}
}
```

## Ejercicio 3

Se debe modelar el funcionamiento de una casa de comida rápida, en la cual trabajan 2
cocineros y 3 vendedores, y que debe atender a C clientes. El modelado debe considerar
que:
-
Cada cliente realiza un pedido y luego espera a que se lo entreguen.
-
Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se
lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender,
los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre
1 y 3 minutos para hacer esto).
-
Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo
cocina y se lo entrega directamente al cliente correspondiente.
Nota: maximizar la concurrencia.

```
chan realizarPedido(text, int)
chan entregar[C] (text)
chan vendedorDisponible(int)
chan asignarPedido[3](text, int)

Process Cliente[id:0..C]
{
	text  comida
	send realizarPedido(id)
	recieve entregar[id] (comida)
}

Process Vendedor[id:0..2]
{
	text p

	while(true)
	{
		send vendedorDisponible(id)
		recieve asignarPedido(p, idC)
		if(p<>'vacio'){
			//tomar pedido
			send pedidosTomados(p, id)
		}
		else
		{
			delay(1-3 min)
		}
	}
}

Process Coordinador
{
	int idV,idC
	text res
	
	while(true)
	{
		recieve vendedorDisponible(idV)
		if(empty(realizarPedido))
		{
			res = 'vacio'
		}
		else
		{
			recieve realizarPedido(res,idC)
		}
		send asignarPedido[idV](res,idC)
		
	}
}

Process Cocinero[id:1..3]
{
	int idC
	text p
	
	while(true)
	{
		recieve pedidosTomados(p, idC)
		//cocinar pedido(p)
		send entregar[idC] (p)
	}
}
```

## Ejercicio 4

Simular la atención en un locutorio con 10 cabinas telefónicas, el cual tiene un empleado
que se encarga de atender a N clientes. Al llegar, cada cliente espera hasta que el empleado
le indique a qué cabina ir, la usa y luego se dirige al empleado para pagarle. El empleado
atiende a los clientes en el orden en que hacen los pedidos. A cada cliente se le entrega un
ticket factura por la operación.
a) Implemente una solución para el problema descrito.
b) Modifique la solución implementada para que el empleado dé prioridad a los que
terminaron de usar la cabina sobre los que están esperando para usarla.
Nota: maximizar la concurrencia; suponga que hay una función Cobrar() llamada por el
empleado que simula que el empleado le cobra al cliente.

