## Ejercicio 1

![[concu 3-1.png]]

a)
Funciona pero no respeta el orden de llegada. Cuando se ejecuta el signal se sigue el protocolo signal and continue, por lo que un nuevo auto puede llega, confirmar que cant no es mayor que 0 y hacer uso del puente, dejando al auto anterior esperando.

b)
```
Monitor Puente
{
	cond cola
	int esperando=0
	bool ocupado = false
	
	Procedure entrarPuente()
	{
		if (ocupado){esperando++; wait(cola)}
		else {ocupado = true}
	}
	
	Procedure salirPuente()
	{
		if(cant>0) {esperando--; signal(cola)}
		else {ocupado=false}
	}
}

Process Auto[id:1..N]
{
	Puente.entrarPuente()
	cruzando()
	Puente.salirPuente()
}
```

¿Sin monitor?
	Entiendo que no, porque un auto no podría comunicarle a otro auto que ya terminó de cruzar. Si o si se necesita un monitor para el envío de mensajes. (Con semáforos se puede pero la idea es no usarlos en esta práctica)
¿Menos Procedimientos?
	No se me ocurre, medio que necesitas los dos del monitor. Uno para que un auto avise que llegó y otro para que avise que terminó.
¿Sin variable condición?
	No porque la necesitas para mantener el orden de llegada.

c)
La primera solución no respeta el orden.
La que implemente en el punto b mantiene el orden de llegada al hacer uso de la variable condición.

## Ejercicio 2

Existen N procesos que deben leer información de una base de datos administrada por un
motor que admite un número limitado de consultas simultáneas.
a) Analice el problema y defina qué procesos, recursos y monitores/sincronizaciones
serán necesarios/convenientes para resolverlo.
b) Implemente el acceso a la base de datos por parte de los procesos, sabiendo que el
motor de la base de datos puede atender a lo sumo 5 consultas de lectura simultáneas.

```
Monitor DB
{
	int cant = 5
	cond cola

	Procedure pedirAcceso()
	{
		while(cant==0){wait(cola)}
		cant--
	}
	
	Procedure salir()
	{
		cant++
		signal(cola)
	}
}

Process Proceso[id:1..N]
{
	DB.pedirAcceso()
	hacerConsulta()
	DB.salir()
}
```

## Ejercicio 3

Existen N personas que deben fotocopiar un documento. La fotocopiadora sólo puede ser
usada por una persona a la vez. Analice el problema y defina qué procesos, recursos y
monitores serán necesarios/convenientes, además de las posibles sincronizaciones requeridas
para resolver el problema. Luego, resuelva considerando las siguientes situaciones:
a) Implemente una solución suponiendo que no importa el orden de uso. Existe una función
Fotocopiar() que simula el uso de la fotocopiadora.
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
c) Modifique la solución de (b) para el caso en que se deba dar prioridad de acuerdo con la
edad de cada persona (cuando la fotocopiadora está libre, la debe usar la persona de mayor
edad entre las que estén esperando para usarla).
d) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden
dado por el identificador del proceso (la persona X no puede usar la fotocopiadora hasta
que no haya terminado de usarla la persona X-1).
e) Modifique la solución de (b) para el caso en que además haya un Empleado que le indica
a cada persona cuándo debe usar la fotocopiadora.
f) Modificar la solución (e) para el caso en que sean 10 fotocopiadoras. El empleado le indica
a la persona qué fotocopiadora usar y cuándo hacerlo.

a)
```
Monitor Fotocopiadora
{
	Procedure usar()
	{
		// hace uso de la fotocopiadora
	}
}

Process Persona[id:1..N]
{
	Fotocopiadora.usar();
}
```

b)
```
Monitor Fotocopiadora
{
	cond cola
	int esperando=0
	bool libre = true
	
	Procedure iniciar()
	{
		if(not libre){esperando++;await(cola);}
		else {libre = false}
		
	}
	
	Process finalizar()
	{
		if(esperando>0)
		{
			signal(cola);
			esperando--
		}
		else{libre=true}
	}
}

Process Persona[id:1..N]
{
	Fotocopiadora.iniciar()
	// hace uso de la fotocopiadora
	Fotocopiadora.finalizar()
}
```

c)
```
Monitor Fotocopiadora
{
	colaCondicional cola;
	int cant_esperando = 0;
	cond[N] esperando;
	bool libre = true;
	
	Procedure iniciar(in id, edad)
	{
		if(not libre){push(cola, id, edad), cant_esperando++; wait(cond[id])}
		else {libre = false}
	}
	
	Procedure finalizar()
	{
		int id;
		if(esperando>0){cant_esperando--; pop(cola, id); signal(esperando[id])}
		else{libre=true}
	}
}

Process Persona[id:1..N]
{
	int edad
	Fotocopiadora.iniciar(id, edad)
	Fotocopiar()
	Fotocopiadora.finalizar()
}
```

d)
```
Monitor Fotocopiadora
{
	int idActual = 1
	cond esperando[N]
	
	Procedure iniciar(in id)
	{
		if(id <> idActual){wait(esperando[id])}
	}
	
	Procedure finalizar()
	{
		idActual++
		signal(esperando[idActual])
	}
}

Process Persona[id:1..N]
{
	Fotocopiadora.iniciar(id)
	Fotocopiar()
	Fotocopiadora.finalizar()
}
```

e)

```
Process Persona
{
	int id
	Fotocopiadora.iniciar(id)
	Fotocopiar()
	Fotocopiadora.finalizar(id)
}

Process Empleado
{
	int id
	for int i=1 to N do
		Fotocopiadora.siguiente(id)
}

Monitor Fotocopiadora
{
	int esperando = 0
	int flibres = 10
	int asignacion[N]
	cola fotocopiadora
	cond cola, hayPersona, termino

	Procedure iniciar(int out:id)
	{
		esperando++
		signal(hayPersona)
		wait(cola)
	}
	
	Procedure siguiente()
	{
		if(libres == 0){wait(disponible)}
		if(esperando == 0){wait(hayPersona)}
		esperando--
		libres--
		pop(C,id)
		asignacion[]
		signal(cola)
		wait(termino)
	}
	
	Procedure finalizar()
	{
		signal(termino)
	}
}
```

f)

```
Process Persona[id:1..N]
{
	int idF
	Fotocopiadora.iniciar(id, idF)
	Fotocopiar(idF)
	Fotocopiadora.finalizar(idF)
}

Process Empleado
{
	for int i=1 to N do
		Fotocopiadora.siguiente()
}

Monitor Fotocopiadora
{
	cola C, F
	cond esperaC, hayPersona, termino, liberada
	int[N] asignacion

	Procedure iniciar(int in:id, int out:idF)
	{
		push(C, id)
		signal(hayPersona)
		wait(esperaC)
		idF = asignacion[id]
	}
	
	Procedure siguiente()
	{
		int id,idF
		
		if(empty(F)){wait(liberada)}
		if(empty(C)){wait(hayPersona)}
		pop(C,id)
		pop(F, idF)
		asignacion[id]=idF
		signal(esperaC)
	}
	
	Procedure finalizar(int in:idF)
	{
		push(F,idF)
		signal(liberada)
	}
}
```

## Ejercicio 4

Existen N vehículos que deben pasar por un puente de acuerdo con el orden de llegada.
Considere que el puente no soporta más de 50000 kg y que cada vehículo cuenta con su
propio peso (ningún vehículo supera el peso soportado por el puente).

```
Monitor Puente
{
	cond espera
	int pesoActual=50000

	Procedure llegada(int in:peso)
	{
		if((pesoActual - peso) < 0){wait(espera)}
		pesoActual = pesoActual - peso
	}
	
	Procedure salida(int in:peso)
	{
		pesoActual=pesoActual+peso
		signal(espera)
	}
}

Process Auto[id:1..N]
{
	int peso

	Puente.llegada(peso)
	//cruzando
	Puente.salida(peso)
}

```

