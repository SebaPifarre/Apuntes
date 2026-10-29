
## Consideraciones

![[Consideracions-ada-1.png]]

![[consideracions-ada-2.png]]

## Ejercicio 1

Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso.
El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2
unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de
vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas,
recursos y sincronizaciones serán necesarios/convenientes para resolverlo.
a. Realice la solución suponiendo que todos los vehículos tienen la misma prioridad.
b. Modifique la solución para que tengan mayor prioridad los camiones que el resto de los
vehículos.

```ada
Task Puente is
	entry EntrarAuto
	entry EntrarCamioneta
	entry EntrarCamion
	entry Salir(P: IN integer)

Task Type Auto
Task Type Camioneta
Task Type Camion

Task body Puente is
	peso:integer;
Begin
	peso:=5
	loop 
		SELECT
			when(peso>=1 and EntrarCamion'count = 0)  => ACCEPT EntrarAuto do
				peso := peso-1
		OR
			when(peso>=2 and EntrarCamion'count = 0) => ACCEPT EntrarCamioneta do
				peso := peso-2
		OR
			when(peso>=3) => ACCEPT EntrarCamion do
				peso := peso-3
		OR
			ACCEPT Salir(P: IN integer) do
				peso := peso + P
		END SELECT
	end loop
end Puente

Task body Auto is
	Puente.EntrarAuto
	//cruza
	Puente.salir(1)

Task body Camioneta is
	Puente.EntrarCamioneta
	//cruza
	Puente.Salir(2)
	
Task body Camion is
	Puente.EntrarCamion
	//cruza
	Puente.Salir(3)
```

## Ejercicio 2

Se quiere modelar el funcionamiento de un banco, al cual llegan clientes que deben realizar
un pago y retirar un comprobante. Existe un único empleado en el banco, el cual atiende de
acuerdo con el orden de llegada.
a) Implemente una solución donde los clientes llegan y se retiran sólo después de haber sido
atendidos.
b) Implemente una solución donde los clientes se retiran si esperan más de 10 minutos para
realizar el pago.
c) Implemente una solución donde los clientes se retiran si no son atendidos
inmediatamente.
d) Implemente una solución donde los clientes esperan a lo sumo 10 minutos para ser
atendidos. Si pasado ese lapso no fueron atendidos, entonces solicitan atención una vez más
y se retiran si no son atendidos inmediatamente.

a)
```ada
Procedure Banco is

Task Empleado is
	entry Solicitar(R: OUT text);
End Banco

Task Type Cliente;

Task body Empleado is
Begin
	loop
		ACCEPT Solicitar(R: OUT text) do
			R:=atender
	end loop
End Empleado

Task body Cliente is
	R:text
Begin
	Empleado.Solicitar(R)
End Cliente

End Banco
```

b)

```
Procedure Banco is

Task Empleado is
	entry Solicitar(datos: IN text; R: OUT text);
End Banco

Task Type Cliente;

Task body Empleado is
Begin
	loop
		ACCEPT Solicitar(datos: IN text; R: OUT text) do
			R:=atender(datos)
	end loop
End Empleado

Task body Cliente is
	R:text
Begin
	SELECT
		Empleado.Solicitar(R)
	OR DELAY 600
		NULL
		END SELECT
End Cliente

Begin
	null
End Banco
```

c)

```
Procedure Banco is

Task Empleado is
	entry Solicitar(datos: IN text; R: OUT text);
End Banco

Task Type Cliente;

Task body Empleado is
Begin
	loop
		ACCEPT Solicitar(datos: IN text; R: OUT text) do
			R:=atender(datos)
	end loop
End Empleado

Task body Cliente is
	R:text
Begin
	SELECT
		Empleado.Solicitar(R)
	OR DELAY 0
		NULL
		END SELECT
End Cliente

Begin
	null
End Banco
```

d)

```
Procedure Banco is

Task Empleado is
	entry Solicitar(datos: IN text; R: OUT text);
End Banco

Task Type Cliente;

Task body Empleado is
Begin
	loop
		ACCEPT Solicitar(datos: IN text; R: OUT text) do
			R:=atender(datos)
	end loop
End Empleado

Task body Cliente is
	R:text
Begin
	SELECT
		Empleado.Solicitar(R)
	OR
		SELECT
			Empleado.Solicitar
		ELSE
			null
		END SELECT
	END SELECT
End Cliente

Begin
	null
End Banco
```

## Ejercicio 3

Se dispone de un sistema compuesto por 1 central y 2 procesos periféricos, que se
comunican continuamente. Se requiere modelar su funcionamiento considerando las
siguientes condiciones:
-
La central siempre comienza su ejecución tomando una señal del proceso 1; luego
toma aleatoriamente señales de cualquiera de los dos indefinidamente. Al recibir una
señal de proceso 2, recibe señales del mismo proceso durante 3 minutos.
-
Los procesos periféricos envían señales continuamente a la central. La señal del
proceso 1 será considerada vieja (se deshecha) si en 2 minutos no fue recibida. Si la
señal del proceso 2 no puede ser recibida inmediatamente, entonces espera 1 minuto y
vuelve a mandarla (no se deshecha).

```
Procedure Central

Task Central is
	entry Pedido1(D: IN text)
	entry Pedido2(D: IN text)
	entry Tiempo
End Central

Task Timer is
	entry Iniciar
End Timer

Task type Perif

p1: Perif
p2: Perif

Task body Central is
	datos:text
Begin
	ACCEPT Pedido1(D: IN text)
	loop
		SELECT
			ACCEPT Pedido1(D: IN text) do
				//resolver señal
			ACCEPT Pedido2(D: IN text) do
				//resolver señal
				loop
					SELECT 
						ACCEPT Pedido2(D: IN text) do
							//resolver señal
						ACCEPT Tiempo
							break
					END SELECT
				end loop
		END SELECT
	end loop
End Central

Task body Timer is
Begin
	ACCEP
End Timer
```