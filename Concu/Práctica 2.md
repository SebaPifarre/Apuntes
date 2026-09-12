
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

```