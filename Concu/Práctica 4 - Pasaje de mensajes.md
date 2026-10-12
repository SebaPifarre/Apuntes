
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

```
chan pedirCabina (int)
chan asignar[N] (int)
chan pagar(int, int, int)
chan enviarTicket[N](text)
chan aviso(int)

Process Empleado
{
	int ok,idCabina,aux
	cola cabinas = (10)
	cola C
	text ticket

	while(true)
	{
		receive aviso(ok)
		
		if(not empty(pedirCabina)) -> {
			recieve pedirCabina(idC)
			if(empty(cabinas))
			{
				push(C, idC)
			}
			else{
				pop(Cabinas, idCabina)
				send asignar[idC](idCabina)
			}
		}
		 (not empty(pagar)) -> {
			recieve pagar(monto, idC, idCabina)
			ticket = Cobrar(monto)
			send enviarTicket[idC](ticket)
			if(not empty(C))
			{
				pop(C,aux)
				send asignar[aux](idCabina)
			}
			else
			{
				push(cabinas, idCabina)
			}
		}
	}
}

Process Cliente[id:0..N-1]
{
	int idCabina, monto
	text ticket

	send pedirCabina(id)
	send aviso(1)
	recieve asignar[id] (idCabina)
	
	monto=usarCabina()
	
	send pagar(monto, id, idCabina)
	send aviso(1)
	recieve enviarTicket(ticket)
}
```

b)
```
chan pedirCabina (int)
chan asignar[N] (int)
chan pagar(int, int, int)
chan enviarTicket[N](text)
chan aviso(int)

Process Empleado
{
	int ok,idCabina,aux
	cola cabinas = (10)
	cola C
	text ticket

	while(true)
	{
		receive aviso(ok)
		
		if (not empty(pagar)) -> {
			recieve pagar(monto, idC, idCabina)
			ticket = Cobrar(monto)
			send enviarTicket[idC](ticket)
			if(not empty(C))
			{
				pop(C,aux)
				send asignar[aux](idCabina)
			}
			else
			{
				push(cabinas, idCabina)
			}
		}
		
		 (not empty(pedirCabina) && empty(pagar)) -> {
			recieve pedirCabina(idC)
			if(empty(cabinas))
			{
				push(C, idC)
			}
			else{
				pop(Cabinas, idCabina)
				send asignar[idC](idCabina)
			}
		}
	}
}

Process Cliente[id:0..N-1]
{
	int idCabina, monto
	text ticket

	send pedirCabina(id)
	send aviso(1)
	recieve asignar[id] (idCabina)
	
	monto=usarCabina()
	
	send pagar(monto, id, idCabina)
	send aviso(1)
	recieve enviarTicket(ticket)
}
```

## Ejercicio 5

Resolver la administración de 3 impresoras de una oficina. Las impresoras son usadas por N
administrativos, los cuales están continuamente trabajando y cada tanto envían documentos
a imprimir. Cada impresora, cuando está libre, toma un documento y lo imprime, de
acuerdo con el orden de llegada.
a) Implemente una solución para el problema descrito.
b) Modifique la solución implementada para que considere la presencia de un director de
oficina que también usa las impresoras, el cual tiene prioridad sobre los
administrativos.
c) Modifique la solución (a) considerando que cada administrativo imprime 10 trabajos y
que todos los procesos deben terminar su ejecución.
d) Modifique la solución (b) considerando que tanto el director como cada administrativo
imprimen 10 trabajos y que todos los procesos deben terminar su ejecución.
e) Si la solución al ítem d) implica realizar Busy Waiting, modifíquela para evitarlo.

Nota: ni los administrativos ni el director deben esperar a que se imprima el documento.

a)
```
chan imprimir(text)

Process Administrativo[id:0..N-1]
{
	text documentos
	
	while(true)
	{
		//trabaja
		send imprimir(documentos)
	}
}

Process Fotocopiadora[id:0..2]
{
	text documentos

	while(true)
	{
		receive imprimir (documentos)
		// imprime
	}
}

```

b)
```
chan imprimir(text, boolean)
chan imprimirPrioridad(text)
chan aviso(int)
chan disponible(text)

Process Administrativo[id:0..N-1]
{
	text documentos
	
	while(true)
	{
		//trabaja
		send imprimir(documentos, false)
		send aviso(1)
	}
}

Process Director
{
	text documentos
	
	while(true)
	{
		//trabaja
		send imprimir(documentos, true)
		send aviso(1)
	}
}

Process Coordinador
{
	text documentos
	int ok

	while(true)
	{
		receive aviso(ok)
		
		if(not empty(imprimirPrioridad))
			receive imprimirPrioridad(documentos)
			send disponible (documentos)
		
		 (not empty(imprimir) && empty(imprimirPrioridad))
			receive imprimirPrioridad(documentos)
			send disponible (documentos)
	}
}

Process Impresora[id:0..2]
{
	text docs
	
	while(true)
	{
		receive disponible(docs)
		// imprime
	}
}

```

c)
```
chan solicitar(int)
chan asignar[N](int)
chan imprimir[3](text)
chan respuesta[N](text)
chan liberar(int)

Process Administrativo[id:0..N-1]
{
	text documentos, copias
	int idF
	
	while(true)
	{
		//trabaja
		send solicitar(id)
		send(aviso)
		receive asignar[id](idF)
		send imprimir[idF](documentos, id)
		receive respuesta[id](copias)
	}
}

Process Fotocopiadora[id:0..2]
{
	text documentos, copias
	int idA

	while(true)
	{
		receive imprimir[id] (documentos)
		copias = copiar(documentos, idA)
		send respuestas[idA] (copias)
		send liberada(id)
		send aviso(1)
	}
}

Process Coordinador
{
	cola C, copiadoras=(0,1,2)
	int ok, idF, idA

	while(true)
	{
		receive aviso(ok)
		
		if (not empty(liberada))
		{
			receive liberada(idF)
			if (empty(C))
				push(copiadoras, idF)
			else
				pop(C, idA)
				send asignar[idA] (idF)
		}
		 (not empty(solicitar) && empty(liberada))
		{
			receive solicitar(idA)
			if (empty(copiadoras))
				push(C, idA)
			else
				pop(copiadoras, idF)
				send asignar[idA] (idF)
		}
	}
}
```