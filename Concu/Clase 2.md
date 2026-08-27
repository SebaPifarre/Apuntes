
### Técnica busy waiting

* Sumamente ineficiente, especialmente si hay un solo core que se encarga de estar preguntando constantemente.
* Con semáforos y monitores está prohibido el uso del busy waiting, solo para está clase

Se implementan protocolos de entrada y de salida para entrar y salir de la sección crítica. 

Propiedades que debe cumplir la solución

* <u>Exclusión mutua</u>: A lo sumo un proceso está en su SC
* <u>Ausencia de Deadlock(livelock)</u>: Si 2 o más procesos tratan de entrar a sus SC, al menos uno tendrá exito
* <u>Ausencia de Demora Innecesaria</u>: Si un proceso trata de entrar a su SC y los otros están en sus SNC o terminaron, el primero no está impedido de entrar a su SC
* <u>Eventual Entrada</u>: Un proceso que intenta entrar a su SC tiene posibilidades de hacerlo (eventualmente lo hará).
	* Muchas veces eventual entrada depende de la política de scheduling.

### Implementación de sentencias await

![[ImplementacionAwait.png]]

Al entrar a la SC se pregunta si la condición no se cumple. En ese caso, para no bloquear a otros procesos de entrar a la SC, se sale de la SC (Exit) y se vuelve a ingresar (Enter)
Esto es ineficiente, ya que muchas veces el mismo proceso sale ingresa nuevamente. Se solventa un poco ingresando un delay entre la salida y la entrada.

Sección crítica -> trozo de código finito.