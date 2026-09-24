
## Ejercicio 1

Existen N personas que deben ser chequeadas por un detector de metales antes de poder ingresar al avión. 
a. Analice el problema y defina qué procesos, recursos y semáforos/sincronizaciones serán necesarios/convenientes para resolverlo. 
b. Implemente una solución que modele el acceso de las personas a un detector (es decir, si el detector está libre la persona lo puede utilizar; en caso contrario, debe esperar). 
c. Modifique su solución para el caso que haya tres detectores. 
d. Modifique la solución anterior para el caso en que cada persona pueda pasar más de una vez, siendo aleatoria esa cantidad de veces.

a) - b)
```
sem detector = 1

Process Persona[id: 0..N-1]
{
	P(detector)
	//se examina
	V(detector)
}
```

c)
```
sem detector = 3

Process Persona[id: 0..N-1]
{
	P(detector)
	//Se examina
	V(detector)
}
```

d)
```
sem detector = 3

Process Persona[id: 0..N-1]
{
	for i=1 to num_random
	{
		P(detector)
		//Seexamina
		V(detector)
	}
}
```

## Ejercicio 2

Un sistema de control cuenta con 4 procesos que realizan chequeos en forma colaborativa. Para ello, reciben el historial de fallos del día anterior (por simplicidad, de tamaño N). De cada fallo, se conoce su número de identificación (ID) y su nivel de gravedad (0=bajo, 1=intermedio, 2=alto, 3=crítico). Resuelva considerando las siguientes situaciones:
a) Se debe imprimir en pantalla los ID de todos los errores críticos (no importa el orden).
b) Se debe calcular la cantidad de fallos por nivel de gravedad, debiendo quedar los resultados en un vector global. 
c) Ídem b) pero cada proceso debe ocuparse de contar los fallos de un nivel de gravedad determinado.

a)

```
datos[0..N-1]

Process Proceso[id:0..3]
{
	i = id * N/4
	fin = i + N/4
	for i to fin
	{
		 if (datos[i].gravedad == critico) write(datos[i])
	} 
}
```

No uso semáforos??

b)
```
datos[0..N-1]
conteo[0..3]=0
sem mutex[0..3]=1

Process Proceso[id:0..3]
{
	conteo_local[0..3]=0
	i = id * N/4
	fin = i + N/4
	for i to fin
	{
		 conteo_local[datos[i].gravedad]++
	} 
	for i=0 to 3
	{
	     P(mutex[i])
		 conteo[i] = conteo[i] + conteo_local[i]
		 V(mutex[i])
	}
}
```

c)
```
datos[0..N-1]
conteo[0..3]=0

Process Proceso[id:0..3]
{
	i = id * N/4
	fin = i + N/4
	for i to fin
	{
		 if (datos[i].gravedad == id) conteo[id]++
	} 
}
```

## Ejercicio 3

Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola. Además, existen P procesos que necesitan usar una instancia del recurso. Para eso, deben sacar la instancia de la cola antes de usarla. Una vez usada, la instancia debe ser encolada nuevamente para su reúso.

```
sem mutex = 5
c cola

Process Proceso[id:0..4]
{
	P(mutex)
	//desencola
	//usa recurso
	V(mutex)
}
```

## Ejercicio 4

Suponga que existe una BD que puede ser accedida por 6 usuarios como máximo al mismo tiempo. Además, los usuarios se clasifican como usuarios de prioridad alta y usuarios de prioridad baja. Por último, la BD tiene la siguiente restricción: • no puede haber más de 4 usuarios con prioridad alta al mismo tiempo usando la BD. • no puede haber más de 5 usuarios con prioridad baja al mismo tiempo usando la BD. Indique si la solución presentada es la más adecuada. Justifique la respuesta.

![[2-4.png]]

```
sem total = 6
sem alta = 4
sem baja = 5

Process Usuario-Alta[id:1..L]::
{
	P(alta)
	P(total)
	//usa la BD
	V(total)
	V(alta)
}

Process Usuario-Baja[id:1..k]::
{
	P(baja)
	P(total)
	//usa la BD
	V(total)
	V(baja)
}
```

## Ejercicio 5

En una empresa de logística de paquetes existe una sala de contenedores donde se preparan las entregas. Cada contenedor puede almacenar un paquete y la sala cuenta con capacidad para N contenedores. Resuelva considerando las siguientes situaciones: 
a) La empresa cuenta con 2 empleados: un empleado Preparador que se ocupa de preparar los paquetes y dejarlos en los contenedores; un empleado Entregador que se ocupa de tomar los paquetes de los contenedores y realizar las entregas. Tanto el Preparador como el Entregador trabajan de a un paquete por vez. 
b) Modifique la solución a) para el caso en que haya P empleados Preparadores.
c) Modifique la solución a) para el caso en que haya E empleados Entregadores. 
d) Modifique la solución a) para el caso en que haya P empleados Preparadores y E empleados Entregadores.

a)
```
sem full = 0
sem empty = N
typeT buf[N]
int rear, front = 0

Process Productor
{
	while(true)
		//Genera paquete
		P(empty)
		buf[rear] = paquete
		rear = (rear + 1) mod N
		V(full)
	
}

Process Consumidor
typeT paquete
Process Consumidor
{
	while(true)
		P(full)
		paquete = buf[front]
		front = (front + 1) mod N
		V(empty)
		//Consume paquete
}
```

b)
```
sem full = 0
sem empty = N
sem mutexD = 1
typeT buf[N]
int rear, front = 0

Process Productor
{
	while(true)
		//Genera paquete
		P(empty)
		P(mutexD)
		buf[rear] = paquete
		rear = (rear + 1) mod N
		V(mutexD)
		V(full)
	
}

Process Consumidor
typeT paquete
Process Consumidor
{
	while(true)
		P(full)
		paquete = buf[front]
		front = (front + 1) mod N
		V(empty)
		//Consume paquete
}
```

```
sem full = 0
sem empty = N
sem mutexF = 1
typeT buf[N]
int rear, front = 0

Process Productor
{
	while(true)
		//Genera paquete
		P(empty)
		buf[rear] = paquete
		rear = (rear + 1) mod N
		V(full)
	
}

Process Consumidor
typeT paquete
Process Consumidor
{
	while(true)
		P(full)
		P(mutexF)
		paquete = buf[front]
		front = (front + 1) mod N
		V(mutexF)
		V(empty)
		//Consume paquete
}
```

d)

```
sem full = 0
sem empty = N
sem mutexD,mutexF = 1
typeT buf[N]
int rear, front = 0

Process Productor
{
	while(true)
		//Genera paquete
		P(empty)
		P(mutexD)
		buf[rear] = paquete
		rear = (rear + 1) mod N
		V(mutexD)
		V(full)
	
}

Process Consumidor
typeT paquete
Process Consumidor
{
	while(true)
		P(full)
		P(mutexF)
		paquete = buf[front]
		front = (front + 1) mod N
		V(mutexF)
		V(empty)
		//Consume paquete
}
```

## Ejercicio 6

Existen N personas que deben imprimir un trabajo cada una. Resolver cada ítem usando semáforos:
a) Implemente una solución suponiendo que existe una única impresora compartida por todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar el orden. Existe una función Imprimir(documento) llamada por la persona que simula el uso de la impresora. Sólo se deben usar los procesos que representan a las Personas. 
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
c) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden dado por el identificador del proceso (la persona X no puede usar la impresora hasta que no haya terminado de usarla la persona X-1).
d) Modifique la solución de (b) para el caso en que además hay un proceso Coordinador que le indica a cada persona que es su turno de usar la impresora.
e) Modificar la solución (d) para el caso en que sean 5 impresoras. El coordinador le indica a la persona cuándo puede usar una impresora, y cual debe usar.

a)
```
sem libre = 1
Process Persona[id:0..N-1]
{
	P(libre)
	imprimir(documento)
	V(libre)
}
```

b)
```
sem espera[N] = ([N] 0)
sem mutex = 1
cola c
boolean libre = true

Process Persona[id:0..N-1]
{
	P(mutex)
	if(libre) {libre = false; V(mutex)}
	else
	{
		push(C,id)
		V(mutex)
		P(espera[id])
	}
	imprimir(documento)
	P(mutex)
	if(empty(C)) libre=true
	else {pop(C,aux); V(espera[id])}
	V(mutex)
}
```

c)
```
sem espera[N] = ([N] 1,0...)
sem mutex = 1
int siguiente = 0

Process Persona[id:0..N-1]
{
	P(espera[id])
	imprimir(documento)
	V(espera[id+1])
}
```

d)
```
sem espera[N] = ([N] 0)
sem mutex = 1
sem termino = 0
cola C
int id;

Process Coordinador
{
	while(true)
	{
		if(C not empty)
		{
			P(mutex)
			pop(C, id)
			V(mutex)
			V(espera[id])
			P(termino)
		}
	}
}

Process Persona[id:0..N-1]
{
	P(mutex)
	push(C,id);
	V(Mutex); 
	P(espera[id]);
	imprimir(documento)
	V(termino)
}
```

e) TERMINAR

```
sem espera[N] = ([N] 0)
sem mutex = 1
sem impresoras = 5
cola C
int IDimpre
cola I = (1,2,3,4,5)

Process Coordinador
{
	while(true)
	{
		if(C not empty)
		{
			P(mutex)
			pop(C, id)
			V(mutex)
			P(impresoras)
			pop(I, IDimpre)
			V(espera[id])
		}
	}
}

Process Persona[id:0..N-1]
{
	P(mutex)
	push(C, id)
	V(mutex)
	P(espera[id])
	imprimir(documento)
	V(impresoras)
}

```

## Ejercicio 7

Suponga que se tiene un curso con 50 alumnos. Cada alumno debe realizar una tarea y existen 10 enunciados posibles. Una vez que todos los alumnos eligieron su tarea, comienzan a realizarla. Cada vez que un alumno termina su tarea, le avisa al profesor y se queda esperando el puntaje del grupo (depende de todos aquellos que comparten el mismo enunciado). Cuando un grupo termina, el profesor les otorga un puntaje que representa el orden en que se terminó esa tarea de las 10 posibles.
Nota: Para elegir la tarea, suponga que existe una función elegir que le asigna una tarea a un alumno (esta función asignará 10 tareas diferentes entre 50 alumnos, es decir, que 5 alumnos tendrán la tarea 1, otros 5 la tarea 2 y así sucesivamente para las 10 tareas).

```
int presentes = 0, enunciado_actual
sem mutex = 1, barrera = 0, espera_profesor = 0, entrega = 0, espera_resultado = 0

int puntajes[5]
int terminados[5]
cola C

Process Alumno[id: 1..N]
{
	int i, nro_enunciado, nota
	
	nro_enunciado = elegir()
	P(mutex) 
	presentes = presentes + 1
	if (presentes == N) 
		{
		for i = 1..N -> { V(Barrera); }
		}
	V(mutex)
	P(Barrera)
	// empieza examen
	Pmutexentregas
	C(push, nro_enunciado)
	Vmutexentregas
	V(avisa_profesor)
	P(terminados[nro_enunciado])
	nota=puntajes_finales[nro_enunciado]
}

Process Profesor
{
	int i
	int entregas[5]
	int id_grupo

	for i = 1..N
	{
		P(avisa_profesor)
		P(mutexentregas)
		id_grupo = pop(C)
		V(mutexentregas)
		puntajes[id_grupo]+=calcularPuntaje()
		entregas[id_grupo]++
		if(entregas[id_grupo]==5)
		{		
			puntajes_finales[id_grupo]=puntajes[id_grupo]
			for i=1 to 5
				V(terminados[enunciado_actual])
		}  
		
	}
}
```

## Ejercicio 8

Una fábrica de piezas metálicas debe producir T piezas por día. Para eso, cuenta con E empleados que se ocupan de producir las piezas de a una por vez. La fábrica empieza a producir una vez que todos los empleados llegan. Mientras haya piezas por fabricar, los empleados tomarán una y la realizarán. Cada empleado puede tardar distinto tiempo en fabricar una pieza. Al finalizar el día, se debe conocer cuál es el empleado que más piezas fabricó.
a) Implemente una solución asumiendo que T > E.
b) Implemente una solución que contemple cualquier valor de T y E.

```
int empleados_presentes = 0, total = 0, 
sem mutex = 1, barrera = 0, 
int cantidades[N]
Process Empleado[id:1..E]
{
	bool seguir = true
	P(mutex)
	empleados_presentes+=1
	if (empleados_presentes == N) 
		{
		for i = 1..N ->  V(Barrera) 
		}
	V(mutex)
	P(Barrera)
	P(mutex)
	while (total<T)
	{
		total++
		V(mutex)
		//produce pieza
		P(mutex)		
	}
	V(mutex)
}
```

## Ejercicio 9

Resolver el funcionamiento en una fábrica de ventanas con 7 empleados (4 carpinteros, 1 vidriero y 2 armadores) que trabajan de la siguiente manera: 
• Los carpinteros continuamente hacen marcos (cada marco es armado por un único carpintero) y los dejan en un depósito con capacidad de almacenar 30 marcos (N). 
• El vidriero continuamente hace vidrios y los deja en otro depósito con capacidad para 50 (M) vidrios. 
• Los armadores continuamente toman un marco y un vidrio (en ese orden) de los depósitos correspondientes y arman la ventana (cada ventana es armada por un único armador).

```
Marco buf[N]; int ocupadoM = 0, libreM = 0, libreV = 0, ocupadoV = 0;
Vidrio bufV[M];
sem vacioM = N, llenoM = 0, mutexD = 1, mutexR = 1, vacioV = M, llenoV = 0, mutexRV = 1;
Process Carpintero[id:1..4]
{
	while(true)
	{
		// producir marco
		P(vacioM)
		P(mutexD)
			buf[libreM]=marco
			libre=(libreM+1) mod N
		V(mutexD)
		V(llenoM)
	}
}

Process Vidriero
{
	while(true)
	{
		//producir vidrio
		P(vacioV)
		bufV[libreV]=vidrio
		libreV = (libreV + 1) mod M
		V(llenoV)
	}
}

Process Armador[id: 1..2]
{
	Marco unMarco;
	Vidrio unVidrio;
	
	while(true)
	{
		P(llenoM)
		P(mutexR)
		unMarco = buf[ocupadoM]
		ocupadoM = (ocupadoM + 1) mod N
		V(mutexR)
		V(vacioM)
		P(llenoV)
		P(mutexRV)
		unVidrio = bufV[ocupadoV]
		ocupadoV = (ocupadoV + 1) mod M
		V(mutexRV)
		V(vacioV)
		// producir cuadro
	}
}
```

Preguntar si está bien solicitar/liberar el semáforo mutexR <mark style="background: #FFB86CA6;">dos veces</mark>.

## Ejercicio 10

A una cerealera van T camiones a descargarse trigo y M camiones a descargar maíz. Sólo hay lugar para que 7 camiones a la vez descarguen, pero no pueden ser más de 5 del mismo tipo de cereal. 

a) Implemente una solución que use un proceso extra que actúe como coordinador entre los camiones. El coordinador debe atender a los camiones según el orden de llegada. Además, debe retirarse cuando todos los camiones han descargado. 
b) Implemente una solución que no use procesos adicionales (sólo camiones). No importa el orden de llegada para descargar. Nota: maximice la concurrencia.

inciso a
<mark style="background: #FFB86CA6;">Preguntar</mark> por enunciado, si tengo que atender a un trigo pero hay 5 trigos ocupados y llega un maiz. No deberia dejarlo pasar?
También preguntar si está mal encolar cuando hay espacio disponible

```
int totalT = T, totalM = M
sem esperaT[T]([T], 0)
sem esperaM[M]([M], 0)
sem mutexT = 0, mutexM = 0,

Process Coordinador
{
	while(totalT > 0 && totalM > 0)
	{
		P()
	}
}

Process Trigo[id:1..T]
{
	P(mutexT)
	if(nt = 5 || nt+nm = 7)
	{
		push(colaT,id)
		V(mutexT)
		P(esperaT[id])
	}
	else
	{
		nt++
		V(mutexT)
	}
	nt++ // si lo hago así, acá se podrían colar
	// realiza descarga
	P(mutexTotal)
	totalT++
	V(mutexTotal)
	V(evento)
}

Process Coordinador
{
	while (totalT > 0 && totalM > 0)
	{
		P(evento)
		P(mutexT)
		P(mutexM)
		if (nt<5){pop(colaT, id); V(esperaT[id])}
		else if(nm<5) {pop(colaM, id); V(esperaM[id])}
		V(mutexM)
		V(mutexT)
		P(mutexTotal)
	}
}
```


inciso b
```
sem total = 7, trigo = 5, maiz=5

Process Trigo[id:1..T]
{
	P(trigo)
	P(total)
	// realiza descarga
	V(total)
	V(trigo)
	
}

Process Maiz[id:1..M]
{
	P(maiz)
	P(total)
	// realiza descarga
	V(total)
	V(maiz)
}


```

## Ejercicio 11

En un vacunatorio hay un empleado de salud para vacunar a 50 personas. El empleado de salud atiende a las personas de acuerdo con el orden de llegada y de a 5 personas a la vez. Es decir, que cuando está libre debe esperar a que haya al menos 5 personas esperando, luego vacuna a las 5 primeras personas, y al terminar las deja ir para esperar por otras 5. Cuando ha atendido a las 50 personas el empleado de salud se retira. 
Nota: todos los procesos deben terminar su ejecución; suponga que el empleado tiene una función VacunarPersona() que simula que el empleado está vacunando a UNA persona.

```
int P=50
sem espera=([P],0)
sem liberado = ([P],0)
cola C=(int)
int esperando = 0

Process Persona[id:1..P]
{
	P(mutex)
	push(C,id)
	esperando++
	if(esperando=5){esperando=0; V(despertar_empleado)}
	V(mutex)
	P(espera[id])
	P(liberado[id])
}
Process Empleado
{
	int id, i, j
	for i=1 to 10
	int actuales[5]
	{
		P(despertar_empleado)
		for j=1 to 5
		{
			P(mutex)
			pop(C,id)
			V(mutex)
			V(espera[id])
			actuales[j]=id
		}
		
		for j=1 to 5
		{
			// aplica vacuna
			V(liberado[actuales[j]])
		}
		
	}
}
```

<mark style="background: #FFB86CA6;">Consulta</mark>

## Ejercicio 12

Simular la atención en una Terminal de Micros que posee 3 puestos para hisopar a 150 pasajeros. En cada puesto hay una Enfermera que atiende a los pasajeros de acuerdo con el orden de llegada al mismo. Cuando llega un pasajero, se dirige al Recepcionista, quien le indica qué puesto es el que tiene menos gente esperando. Luego se dirige al puesto y espera a que la enfermera correspondiente lo llame para hisoparlo. Finalmente, se retira. 
a) Implemente una solución considerando los procesos Pasajeros, Enfermera y Recepcionista.
b) Modifique la solución anterior para que sólo haya procesos Pasajeros y Enfermera, siendo los pasajeros quienes determinan por su cuenta qué puesto tiene menos personas esperando. Nota: suponga que existe una función Hisopar() que simula la atención del pasajero por parte de la enfermera correspondiente.

inciso a

```
cola c1,c2,c3
int cant1,cant2,cant3 = 0

Process Recepcionista
{
	
}
```