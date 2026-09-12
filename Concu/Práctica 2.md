
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
sem mutex = 1
datos[0..N-1]

Process Proceso[id:0..3]
{
	P(mutex)
	if(index<N)
	{
		aux
	}
}
```