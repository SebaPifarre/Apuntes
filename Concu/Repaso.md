
# Semáforos

## Ejercicio 1

Resolver los problemas siguientes:
a) En una estación de trenes, asisten P personas que deben realizar una carga de su tarjeta SUBE
en la terminal disponible. La terminal es utilizada en forma exclusiva por cada persona de acuerdo
con el orden de llegada. Implemente una solución utilizando únicamente procesos Persona. Nota:
la función UsarTerminal() le permite cargar la SUBE en la terminal disponible.
b) Resuelva el mismo problema anterior pero ahora considerando que hay T terminales disponibles.
Las personas realizan una única fila y la carga la realizan en la primera terminal que se libera.
Recuerde que sólo debe emplear procesos Persona. Nota: la función UsarTerminal(t) le permite
cargar la SUBE en la terminal t.

a)

```
sem mutex = 1
bool libre = true
cola C
sem espera[P] = ([P], 0)

Process Persona[id:1..P]
{
	int aux

	P(mutex)
	if(libre){libre=false; V(mutex)}
	else
	{
		push(C,id)
		V(mutex)
		P(espera[id])
	}
	UsarTerminal()
	P(mutex)
	if(empty(C)){libre=true; V(mutex)}
	else {pop(C, aux); V(mutex); V(espera[aux])}
}
```

b)

```
sem mutex = 1
cola T = {1..T}
cola C
sem espera[P] = ([P], 0)
int asignada[P]

Process Persona[id:1..P]
{
	int aux
	int terminal

	P(mutex)
	if(not empty(T)){pop(T, terminal) V(mutex)}
	else
	{
		push(C,id)
		V(mutex)
		P(espera[id])
		terminal = asignada[id]
	}
	UsarTerminal(terminal)
	P(mutex)
	if(empty(C)){push(T, terminal); V(mutex)}
	else {pop(C, aux); asignada[aux]=terminal; V(mutex); V(espera[aux])}
}
```

## Ejercicio 2

Implemente una solución para el siguiente problema. Un sistema debe validar un conjunto de 10000(T) transacciones que se encuentran disponibles en una estructura de datos. Para ello, el sistema dispone de 7 workers, los cuales trabajan colaborativamente validando de a 1 transacción por vez cada uno.
Cada validación puede tomar un tiempo diferente y para realizarla los workers disponen de la
función Validar(t), la cual retorna como resultado un número entero entre 0 al 9. Al finalizar el
procesamiento, el último worker en terminar debe informar la cantidad de transacciones por cada
resultado de la función de validación. Nota: maximizar la concurrencia.


```
int prox = 0
transaccion[T] transacciones
int[10] totales = ([10], 0)
sem mutexD, mutexR = 1
int terminados = 0

Process Worker[id:1..7]
{
	transaccion t
	int puntaje,i
	int[10] parciales = ([10], 0)

	while(true)
	{
		P(mutexD)
		if(prox < T)
		{
			t=transaciones[prox]
			prox++; 
			V(mutexD)
			puntaje = Validar(t)
			parciales[puntaje]++
		}
		else
		{
			V(mutexD)
			break
		}
	}
	P(mutexR)
	for i = 1 to 10
		totales[i]+=parciales[i]
	terminados++
	if(terminados == 7){se informa}
	V(mutexR)
	
	
}
```

¿Puedo usar el mismo mutexR para actualizar los valores y para la variable terminados?

## Ejercicio 3

Implemente una solución para el siguiente problema. Se debe simular el uso de una máquina
expendedora de gaseosas con capacidad para 100 latas por parte de U usuarios. Además, existe un
repositor encargado de reponer las latas de la máquina. Los usuarios usan la máquina según el orden de llegada. Cuando les toca usarla, sacan una lata y luego se retiran. En el caso de que la máquina se quede sin latas, entonces le debe avisar al repositor para que cargue nuevamente la máquina en forma completa. Luego de la recarga, saca una botella y se retira. Nota: maximizar la concurrencia; mientras se reponen las latas se debe permitir que otros usuarios puedan agregarse a la fila.

```
sem mutexD = 1
sem mutexR = 0
sem notificar = 0
cola C
sem espera[U]=([U],0)
bool libre = true
int cantidad = 100

Process Usuario[id:1..U]
{
	int aux

	P(mutexD)
	if(not libre){push(C,id); V(mutexD); P(espera[id])}
	else
	{
		libre=false
		V(mutexD)
	}
	
	if(cant == 0){V(notificar); P(mutexR)}
	cantidad--
	
	P(mutexD)
	if(empty(C)){libre=true}
	else{pop(C, aux); V(espera[aux])}
	V(mutexD)
}

Process Repositor
{
	while(true)
	{
		P(notificar)
		// reposita la maquina
		cantidad = 100
		V(mutexR)
	}
}
```

Preguntar lo de el último V(mutexD) comparado con el 1 a
Está bien que cantidad no esté dentro de un P-V?