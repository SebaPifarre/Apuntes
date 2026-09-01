
## Ejercicio 1

![[bd-prac1-ej1.png]]

a) Cambiaría la cardinalidad a por -> (1,1)

b) Cambiaría la cardinalidad b por -> (1,1)

c) Ajuste el modelo para representar museos de unicamente dos tipos: de arte contemporáneo, con fecha de inauguración, país, director, curador a cargo y movimiento artístico; y de arte en general, del cual se conoce una fecha estimada de inauguración, país, director, restaurador principal y datos históricos. De los datos históricos se registra un año y una descripción histórica, por ejemplo que una pintura famosa se exhibió por primera vez allí en un año determinado.

![[res.png]]

## Ejercicio 2

A. En una especialización, la entidad padre no representa datos que realmente existan, sino que sirve para representar los aspectos comunes de las entidades hijas.
B. En una agregación, la cardinalidad mínima debe ser mayor a 0 
C. Una entidad puede no tener un atributo identificador en el modelo ER 
D. No es correcto modelar atributos en las relaciones en un modelo ER

A) Falsa. Una especialización es el resultado de tomar u subconjunto de entidades de un nivel para formar un conjunto de entidades de nivel más bajo.
Una generalización es el resultado de tomar uno o más conjuntos de entidades (de nivel más bajo) y producir un conjunto de entidades de un nivel más alto.

B) Falso. La cardinalidad máxima para cada entidad de la relación siempre es mayor a 1.

C) Falso. Toda entidad posee al menos una posible clave o identificador. Sirven para identificar de manera única a una entidad.

D) Falsa. Las relaciones pueden tener atributos. Creo que solamente si tiene cardinalidad muchos a muchos o opcional a muchos (cuando la relación se vuelve tabla al pasar al modelo física).



