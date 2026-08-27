
#### Propiedades de seguridad y vida

* Seguridad
	* Partial correctness -> Asegura que si finaliza el proceso el resultado es correcto pero no asegura que el proceso finalice
	* Una falla de seguridad indica que algo anda mal
* Vida
	* Una falla de vida indica que las cosas dejan de ejecutar
	* Tiene que ver con los conceptos de deadlock y inanición

Total correctness es a lo que se aspira, que el programa no tenga errores y al finalizar garantiza un resultado correcto.

Fairness:
	trata de garantizar que todos los procesos tengan chances de avanzar sin importar que hacen el resto de los procesos

elegible
	Una acción atómica en un proceso es elegible si es la próxima acción atómica en el proceso que será ejecutada. varios procesos -> varias acciones atómicas elegibles

La política de sheduling determina cuál será la próxima en ejecutarse

* Fairness Incondicional
	* Asegura que toda acción atómica incondicional que es elegible eventualmente es ejecutada