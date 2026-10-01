
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

```
