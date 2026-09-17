## Diseño de un reloj lógico

Primera solución)
* Tiene el problema que despierta a todos cada vez que pasa una instancia de tiempo
* Despertar a todos para que checkeen y se vuelvan a dormir no es una buena solución
* Pude darse que se despierten muchos timers a la vez pero al que le corresponde cortar quede atascado esperando que si libere la condición para checkear

Segunda solución)
* 