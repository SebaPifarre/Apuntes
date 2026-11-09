
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

```ada
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

```ada
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

```ada
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

```ada
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

```ada
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

```ada
Procedure ej5

Task Servidor is
	Entry Documento(d:IN texto; res:OUT texto; err: OUT text)
end Servidor;

Task Type Usuario;

arrUsuarios: array (1..U) of Usuario;

Task Body Usuario is
	doc,res,err:texto
	seguir:boolean:=true
Begin
	trabajar(doc)
	while(seguir) loop
		SELECT
			Servidor.Documento(doc,res,err)
			if(err=null)
				seguir:=false
			else
				modificar(doc)
		OR DELAY 120
			delay 60
		end select;
	end loop;
end Usuario;

Task Body Servidor is
	res,err,doc:texto
Begin
	loop
		accept Documento(doc:IN texto;res OUT text;err:OUT text) do
			err := verificarErrores(err);
			res:= procesar(doc);
	end loop;
end Servidor;
```

## Ejercicio 6

En una playa hay 5 equipos de 4 personas cada uno (en total son 20 personas donde cada
una conoce previamente a que equipo pertenece). Cuando las personas van llegando
esperan con los de su equipo hasta que el mismo esté completo (hayan llegado los 4
integrantes), a partir de ese momento el equipo comienza a jugar. El juego consiste en que
cada integrante del grupo junta 15 monedas de a una en una playa (las monedas pueden ser
de 1, 2 o 5 pesos) y se suman los montos de las 60 monedas conseguidas en el grupo. Al
finalizar cada persona debe conocer el grupo que más dinero junto. Nota: maximizar la
concurrencia. Suponga que para simular la búsqueda de una moneda por parte de una
persona existe una función Moneda() que retorna el valor de la moneda encontrada.

```ada
Procedure ej6

Task Type Equipo is
	Entry Llegada;
	Entry Comenzar;
	Entry Ident(id:IN int)
	Entry Ganador(g:OUT int);
end Equipo;

Task Type Persona;

Task Admin is
	Entry Submit (v:IN int; e:IN int)
	Entry Resultado(v:OUT int)
end Admin;

arrEquipo: array(1..5) of Equipo;
arrPersona: array(1..20) of Persona;

Task Body Equipo is
	i:int
	total:int:=0
	quienGano:int
	miId:int
Begin
	accept Ident(id:IN int) do
		miId:=id
	for i in 1..4 loop
		accept llegada
	end loop;
	for i in 1..4 loop
		accept comenzar
	end loop;
	
	for i in 1..4 loop
		accept depositar(val:IN int)
			total:=total+val
	end loop;
	
	Admin.Submit(total, miId)
	accept Resultad(e:OUT int) do
		quienGano := e
	for i in 1..4 loop
		accept ganador(g:OUT int) do
			g:=quienGano;
	end loop;
end Equipo;

Task Body Persona is
	monto:int:=0
	miEquipo,v,i,g:int
Begin
	miEquipo:=obtener(miEquipo)
	arrEquipo(miEquipo).Llegada;
	arrEquipo(miEquipo).Comenzar
	
	for i in 1..15 loop
		Moneda(v)
		monto:=monto+v
		
	arrEquipo(miEquipo).depositar(monto);
	arrEquipo(miEquipo).Ganador(g);
end Persona;
Task Body Admin is
	arrRes: array(1..5) of int;
Begin
	for i in 1..5 loop
		accept Submit(num:IN int; e:IN int) do
			arrRes(e):=num;
	g:=obtenerGanador(arrRes)
	for i in 1..5 loop
		accept Resultado(gandor: OUT int) do
			ganador:=g
end Admin;
begin
	for i in 1..5 loop
		Equipo.Ident(i)
	end loop;
end ej6;
```

## Ejercicio 7

Se debe calcular el valor promedio de un vector de 1 millón de números enteros que se
encuentra distribuido entre 10 procesos Worker (es decir, cada Worker tiene un vector de
100 mil números). Para ello, existe un Coordinador que determina el momento en que se
debe realizar el cálculo de este promedio y que, además, se queda con el resultado. Nota:
maximizar la concurrencia; este cálculo se hace una sola vez.

```ada
Procedure ej7

Task Coordinador is
	Entry Submit(val:IN real)
end Coordinador;

Task Type Worker is
	Entry Comienzo;
End Worker;

arrWorker : array (1..10) of Worker;

Task Body Worker is
	miArr: array(1..1M) of int;
	total:real:=0.0;
Begin
	accept Comienzo do
	
	for i in 1..1M loop
		total:=total+miArr(i);
	end loop;
	total:=total/1M
	Coordinador.Submit(total);
End Worker;

Task Body Coordinador is
	total:real:=0.0;
	p,res:real;
Begin
	for i in 1..10 loop
		arrWorker(i).Comienzo;
	end loop;
	for i in 1..10 loop
		accept Submit(parcial:IN real) do
			p:=parcial;
		total:=total+p
	end loop;
	res := total/10;
End Coordinador;
Begin
	null
End ej7;
```

```ada
Procedure ej7

Task Coordinador is
	Entry Comienzo;
	Entry Submit(val:IN real)
end Coordinador;

Task Type Worker;

arrWorker : array (1..10) of Worker;

Task Body Worker is
	miArr: array(1..100_000) of int;
	total:real:=0.0;
Begin
	Coordinador.Comienzo;
	
	for i in 1..100_000 loop
		total:=total+miArr(i);
	end loop;
	Coordinador.Submit(total);
End Worker;

Task Body Coordinador is
	total:real:=0.0;
	p,res:real;
Begin
	for i in 1..20 loop
		SELECT
			accept Comienzo;
			end Comienzo;
		OR
			accept Submit(parcial:IN real) do
				p:=parcial;
			end Submit;
			total:=total+p
		end select;
	end loop;
	res := total/1M;
End Coordinador;
Begin
	null
End ej7;
```

## Ejercicio 8

Hay un sistema de reconocimiento de huellas dactilares de la policía que tiene 8 Servidores
para realizar el reconocimiento, cada uno de ellos trabajando con una Base de Datos propia;
a su vez hay un Especialista que utiliza indefinidamente. El sistema funciona de la siguiente
manera: el Especialista toma una imagen de una huella (TEST) y se la envía a los servidores
para que cada uno de ellos le devuelva el código y el valor de similitud de la huella que más
se asemeja a TEST en su BD; al final del procesamiento, el especialista debe conocer el
código de la huella con mayor valor de similitud entre las devueltas por los 8 servidores.
Cuando ha terminado de procesar una huella comienza nuevamente todo el ciclo. Nota:
suponga que existe una función Buscar(test, código, valor) que utiliza cada Servidor donde
recibe como parámetro de entrada la huella test, y devuelve como parámetros de salida el
código y el valor de similitud de la huella más parecida a test en la BD correspondiente.
Maximizar la concurrencia y no generar demora innecesaria.

```ada
Procedure ej8

Task Especialista is
	Entry Siguiente(test:OUT texto);
	Entry Resultado(cod:IN text; valor:IN int);
End Especialista;

Task Type Servidor;

arrServidor: array (1..8) of Servidor;

Task Body Servidor is
	DB:database;
	huella,codigo:texto;
	valor:int;
Begin
	loop
		Especialista.Siguiente(huella);
		Buscar(huella,codigo,valor,DB);
		Especialista.Resultado(codigo,valor);
	end loop;
End Servidor;

Task Body Especialista is
	huella:texto;
	C:cola;
Begin
	loop
		Obtener(huella)
		reset(C)
		for i in 1..8 loop
			accept Siguiente(test:OUT texto) do
				test:=huel;a;
			end Siguiente;
		end loop;
		for i in 1..8 loop
			accept Resultado(cod:IN text; valor:IN int) do
				push(C,valor,cod)
			end Resultado;
		end loop;
		ObtenerMayor(C) -- podria ser un maximo y ya
	end loop;
End Especialista;

Begin
	null;
End ej8;

```

## Ejercicio 9

Una empresa de limpieza se encarga de recolectar residuos en una ciudad por medio de 3
camiones. Hay P personas que hacen reclamos continuamente hasta que uno de los camiones pase por su casa. Cada persona hace un reclamo y espera a lo sumo 15 minutos a que llegue un camión; si no pasa, vuelve a hacer el reclamo y a esperar a lo sumo 15 minutos a que llegue un camión; y así sucesivamente hasta que el camión llegue y recolecte los residuos. Sólo cuando un camión llega, es cuando deja de hacer reclamos y se retira. Cuando un camión está libre la empresa lo envía a la casa de la persona que más reclamos ha hecho sin ser atendido. Nota: maximizar la concurrencia.

```ada
Procedure ej9

Task Admin is
	Entry Reclamo(id:IN int);
	Entry Siguiente(id:OUT int);
	Entry YaPaso(id:IN int);
End Admin;

Task Type Persona is
	Entry Ident(id:IN int);
	Entry PasoCamion;
End Persona;

Task Type Camion;

arrPersona: array (1..P) of Persona;
arrCamion: array (1..3) of Camion;

Task Body Persona is
	miID:int;
	sucio:boolean:=true;
Begin
	accept Ident(id: IN int) do
		miID=id;
	end Ident;
	while(sucio) loop
		Admin.Reclamo(id);
		SELECT
			accept PasoCamion do
				sucio:=false;
			end PasoCamion;
		OR DELAY 900
			null
		end select;
	end loop;
	Admin.YaPaso(id);
End Persona;

Task Body Camion is
	prox:int
Begin
	loop
		Admin.Siguiente(prox);
		arrPersona(prox).PasoCamion;
		// recoleta la basura
	end loop;
End Camion;

Task Body Admin is
	reclamos: array (1..P) of int:=(P,0);
	actual:int;
	cant:int:=0;
Begin
	loop
		SELECT 
			accept Reclamo(id:IN int) do
				actual:=id;
			end Reclamo;
			if(reclamos(actual)>-1) then
				if(reclamos(actual)=0) then
					cant:=cant+1;
				end if;
				reclamos(actual)++;
				cant++;
			end if;
		OR
			when(cant>0) => accept Siguiente(id:OUT int) do
				actual:= posMax(reclamos)
				id:=actual
			end Siguiente;	
			cant:=cant-1;
			reclamos(actual):=-1;
		OR
			accept YaPaso(id:IN int) do
				reclamos(id):=0
			end YaPaso;
		end select;
	end loop;
End Admin;

Begin
	for i in 1..P loop
		arrPersona(i).Ident(i);
	end loop
End ej9;

```