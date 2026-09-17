## Diseño de un reloj lógico

Primera solución
* Tiene el problema que despierta a todos cada vez que pasa una instancia de tiempo
* Despertar a todos para que checkeen y se vuelvan a dormir no es una buena solución
* Pude darse que se despierten muchos timers a la vez pero al que le corresponde cortar quede atascado esperando que si libere la condición para checkear

Tercera solución
* Implementa lo que propone la segunda solución pero usando herramientas que se pueden usar en la práctica.
* Esta manera de implementar la solución me sirve cuando quiero mantener un orden que no es el de llegada.

## Rendezvous

* Notar que hay muchas etapas de sincronización en el mismo problema.
* Dentro de corte de pelo, la idea del wait despues del signal es porque si no libero el monitor, el peluquero no va a poder hacer uso del monitor. Por eso no debo preguntar si tengo que hacer el wait porque se que el peluquero lo va a necesitar.


# Explicación Práctica

* En monitores, si lo trato como objetos (getter, setter), estoy rompiendo con la exclusión mutua porque estría modificando valores de las variables permanentes fuera del monitor.
* Hay que tener cuidado de no meter toda la información dentro de un solo monitor. Solo tener en cada monitor la información que necesite para hacer el mayor uso de la concurrencia.

* Ejercicio 6 -> Cliente - Servidor con orden de llegada

* 