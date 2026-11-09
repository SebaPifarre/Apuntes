
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
				Timer.Iniciar
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
	ACCEPT Iniciar do
		delay 120
		Central.Tiempo
End Timer
```

## Ejercicio 4

En una clínica existe un médico de guardia que recibe continuamente peticiones de
atención de las E enfermeras que trabajan en su piso y de las P personas que llegan a la
clínica ser atendidos.
Cuando una persona necesita que la atiendan espera a lo sumo 5 minutos a que el médico lo
haga, si pasado ese tiempo no lo hace, espera 10 minutos y vuelve a requerir la atención del
médico. Si no es atendida tres veces, se enoja y se retira de la clínica.
Cuando una enfermera requiere la atención del médico, si este no lo atiende inmediatamente
le hace una nota y se la deja en el consultorio para que esta resuelva su pedido en el
momento que pueda (el pedido puede ser que el médico le firme algún papel). Cuando la
petición ha sido recibida por el médico o la nota ha sido dejada en el escritorio, continúa
trabajando y haciendo más peticiones.
El médico atiende los pedidos dándole prioridad a los enfermos que llegan para ser atendidos.
Cuando atiende un pedido, recibe la solicitud y la procesa durante un cierto tiempo. Cuando
está libre aprovecha a procesar las notas dejadas por las enfermeras.

```
Procedure ej4

Task Medico is
	Entry PeticionE(p:IN TEXT)
	Entry PeticionP(p:IN TEXT)
END Medico;

Task Escritorio is
	Entry DejarNota(nota:IN TEXT)
	Entry TomarNota(nota:OUT TEXT)

Task Type Enfermera;
Task Type Paciente;

arrEnfermeras: array (1..E) of Enfermera;
arrPacientes: array(1..P) of Paciente;

Task Body Escritorio is
	C: cola;
Begin
	loop
		SELECT
			accept DejarNota(n: IN TEXT) do
				push(C,n)
			end DejarNota;
		OR
			when(not empty(C)) =>
				accept TomarNota(n: OUT TEXT) do
					pop(C,n)
			end TomarNota;
		END SELECT;
	end loop;
end escritorio;

Task Body Enfermera is
	n:text
Begin
	loop
		Select
			Medico.PeticionE
		Else
			n:=generarNota(n)
			Escritorio.DejarNota(n)
		end select;
	end loop;
end Enfermera;

Task Body Paciente is
	intentos:int
	atendido:boolean
Begin
	atendido:=false
	intentos:=0
	while(not atendido && intendos<3)
		Select 
			Medico.PeticionP()
			atendido=true
		OR delay 300
			intentos++
			delay 600
		end select;
	end while
		
end Paciente;

Task Body medico
	p:text
begin
	loop
		Select
			accept PeticionE(pedido:IN TEXT)
				p:=pedido
			end PeticionE
			resolver(p)
		OR
			when (PeticionP`count=0) =>
				accept PeticionP
					resolver(p)
			end PeticionP;
		else
			select
				Escritorio.TomarNota(p)
				procesarNota(p)
			else
				null
end medico;
begin
null
end ej4;
```

## Ejercicio 5

En un sistema para acreditar carreras universitarias, hay UN Servidor que atiende pedidos
de U Usuarios de a uno a la vez y de acuerdo con el orden en que se hacen los pedidos.
Cada usuario trabaja en el documento a presentar, y luego lo envía al servidor; espera la
respuesta de este que le indica si está todo bien o hay algún error. Mientras haya algún error,
vuelve a trabajar con el documento y a enviarlo al servidor. Cuando el servidor le responde
que está todo bien, el usuario se retira. Cuando un usuario envía un pedido espera a lo sumo
2 minutos a que sea recibido por el servidor, pasado ese tiempo espera un minuto y vuelve a
intentarlo (usando el mismo documento).

```
Procedure ej5

Task Servidor is
	Entry Documento(d:IN texto; res:OUT texto; err: OUT text)
end Servidor;

Task Type Usuario;

arrUsuarios: array (1..U) of Usuario;

Task Body Usuario is
	doc:texto
	seguir:boolean:=true
Begin
	while(seguir) loop
		modificar(doc)
		SELECT
			Servidor.Documento(doc)
			seguir:=false
		OR DELAY 120
			delay 60
		end select;
	end loop;
end Usuario;

Task Body Servidor is
	doc:texto
Begin
	
end Servidor;
```