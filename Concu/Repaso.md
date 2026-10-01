
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

```