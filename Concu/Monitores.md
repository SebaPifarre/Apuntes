## Diseño de un reloj lógico

Primera solución
* Tiene el problema que despierta a todos cada vez que pasa una instancia de tiempo
* Despertar a todos para que checkeen y se vuelvan a dormir no es una buena solución
* Pude darse que se despierten muchos timers a la vez pero al que le corresponde cortar quede atascado esperando que si libere la condición para checkear

Tercera solución
* Implementa lo que propone la segunda solución pero usando herramientas que se pueden usar en la práctica.
* Esta manera de implementar la solución me sirve cuando quiero mantener un orden que no es el de llegada.

## Rendezvous

* Notar que hay muchos puntos de sincronización en el mismo problema.