
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
sem espera[N] = ([N] 0)
sem mutex = 1
int siguiente = 0

Process Persona[id:0..N-1]
{
	if(siguiente <> id) {P(espera[id])}
	imprimir(documento)
	siguiente++
	V(espera[siguiente])
}
```