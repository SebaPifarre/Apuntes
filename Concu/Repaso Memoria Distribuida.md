
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
	Terminal!termine()
}

Process Terminal
{
	id:int
	C:cola
	libre:bool
	
	libre:=true
	do Persona[*]?llegue(id) -> if(libre) libre:=false; Persona[id]!turno();
								else	   push(C,id)
	
	[] Persona[*]?termine() -> if(empty(C)) libre:=true
								else pop(C,id); Persona[id]!turno();
}
```

## Ejercicio 3

Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que
deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son
atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los recibos de pago.

```
chan altaPrioridad(int,text,real),mediaPrioridad(int,text,real),bajaPrioridad(int,text,real)
chan resultado[P](text,real)
chan aviso

Process Persona[id:1..P]
{
	res:texto
	boletas:texto
	plata,vuelto:real
	
	send aviso()

	if("embrazada")
	{
		send altaPrioridad(id,boletas,plata)
	}
	else if("menos de 5")
	{
		send mediaPrioridad(id)
	}
	else
	{
		send bajaPrioridad(id)
	}
	
	receive resultado[id](res,vuelto)
}

Process Caja
{
	res,boletas:text
	id:int
	plata,vuelto:real

	while(true)
	{
	
	receive aviso()
	
	if (empty(altaPrioridad) && empty(mediaPrioridad)); receive bajaPrioridad(id,boletas,plata)
		=> registraPago(res); send resultado[id](res);
		
	[] empty(altaPrioridad); receive mediaPrioridad(id,boletas,plata) => registrarPago(res); send resultado[id](res);
	
	[] receive altaPrioridad(id,boletas,plata) => null;
	
	
	registrarPago(boletas,plata,res,vuelto); send resultado[id](res,vuelto);
	
	
	
	}
}
```

# ADA

## Ejercicio 1

Resolver el siguiente problema. La página web del Banco Central exhibe las diferentes cotizaciones
del dólar oficial de 20 bancos del país, tanto para la compra como para la venta. Existe una tarea
programada que se ocupa de actualizar la página en forma periódica y para ello consulta la cotización de cada uno de los 20 bancos. Cada banco dispone de una API, cuya única función es procesar las solicitudes de aplicaciones externas. La tarea programada consulta de a una API por vez, esperando a lo sumo 5 segundos por su respuesta. Si pasado ese tiempo no respondió, entonces se mostrará vacía la información de ese banco.

```ada
Procedure ej1 is

Task BancoCentral is
end BancoCentral;

Task Type Banco is
	Entry Consultar(c:OUT real);
end Banco;

arrBanco : array(1..20) of Banco;

Task Body Banco is
	cotiHoy:real
Begin
	cotiHoy:=obtenerCoti()
	accept Consultar(c:OUT real)
		c:=cotiHoy;
	end Consultar;
End Banco;

Task Body BancoCentral is
	cotis : array(1..20) of real;
Begin
	for i in 1..20 loop
		SELECT
			arrBanco(i).Consultar(c);
			cotis(i):=c;
		OR DELAY 5
			cotis(i):=null;
		end select;
	end loop;
End BancoCentral;

```

## Ejercicio 2

Resolver el siguiente problema. En un negocio de cobros digitales hay P personas que deben pasar
por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que pagan más. Adicionalmente, las personas ancianas tienen prioridad sobre los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los recibos de pago.

```ada
Procedure ej2Ada is

Task Cajero is
	Entry altaPrioridad(id:IN int; b:IN texto; d:IN real)
	Entry mediaPrioridad(id:IN int; b:IN texto; d:IN real)
	Entry bajaPrioridad(id:IN int; b:IN texto; d:IN real)
End Cajero;

Task Type Persona;

arrPersona:array (1..P) of Persona;

Task Body Persona is
	boletas:texto;
	dinero:real;
	recibos:texto;
	vuelto:real;
Begin
	if(alta) then
		Cajero.altaPrioridad(boletas,dinero,recibos,vuelto)
	else if(media)
		Cajero.mediaPrioridad(boletas,dinero,recibos,vuelto)
	else
		Cajero.bajaPrioridad(boletas,dinero,recibos,vuelto)
	
	
End Persona;
	
Task Body Cajero is


```