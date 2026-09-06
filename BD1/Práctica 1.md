
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


## Ejercicio 3

![[prac1-ej3.png]]

El estado nacional implementó distintos subsidios destinados a sectores productivos. Cada
subsidio tiene un nombre y un monto asignado.
Para cada subsidio se realiza una liquidación mensual, de la cual se registra a qué mes y año
corresponde, el total gastado y la fecha de realización. En esta liquidación, a cada beneficiario
del subsidio se le liquida un monto, el cual dependerá de la situación del beneficiario. Un
beneficiario puede ser una persona Jurídica o Física, y en el caso de la persona física, debe
estar inscripta en el monotributo. De cada beneficiario se conoce la actividad económica en la
cual se encuentra inscripto y su cuil o cuit que lo identifica. De las personas jurídicas se conoce
la razón social, provincia, departamento, localidad y cantidad de empleados. De las personas
físicas se conoce nombre y apellido, provincia, departamento, localidad y categoría del
monotributo.

Para el diagrama de Entidades y Relaciones propuesto responda si las siguientes afirmaciones
son verdaderas (V) o falsas (F). Justificar:
A.​ La relación tiene está mal definida, ya que debería ser entre persona y
categoría_monotributo.
B.​ La relación realiza está bien definida, ya que todas las personas realizan actividades.
C.​ La jerarquía de Persona representa correctamente la problemática.
D.​ La relación pertenece está mal definida, ya que no puede haber atributos en las
relaciones.
E.​ La agregación de la relación posee está correctamente definida ya que con una relación
uno a muchos se puede agregar.
F.​ Con este diseño es posible conocer el saldo disponible del subsidio para futuras
liquidaciones.
G.​ El modelo no tiene redundancia de datos.

A) Falso. Solo las personas físicas están asociadas con el monotributo. Si se hiciera la relación con persona se estaría diciendo que las personas jurídicas tienen monotributo.
B) Verdadero. 
C) Falso. Al ser una especialización se describe que puede haber otro tipo de persona cuando solamente pueden existir los dos tipos descritos.
D) Falso. Las relaciones pueden tener atributos.
E) Falso. No se puede hacer una agregación porque las cardinalidades superiores de la relación deben ser mayor a 1. <mark style="background: #FF5582A6;">Entonces que pasa? Desaparece la agregación?</mark> 
F) Verdadero. Con el monto de la entidad Subsidio menos el monto o el total gastado se puede calcular.
G) Falso. Hay redundancia de datos entre los dos tipos de personas.


## Ejercicio 4

Dados los siguientes modelos E/R sobre vendedores que trabajan en locales, responda:
A.​ En qué casos modelaría un atributo fecha_de_ingreso en la relación se_emplea –entre
Vendedor y Local - como se muestra en la variante “A”?

![[1-4-a.png]]

B.​ ¿En qué casos haría falta modelar una entidad Fecha de Ingreso relacionada con la
agregación Vendedor Local como se muestra en la parte llamada B en el modelo?

![[1-4-b.png]]

C.​ ¿Qué se está modelando con Horario cuando está la agregación? Indíquelo agregando
la cardinalidad correspondiente

A) En la variante A, al guardar la fecha de ingreso en la relación *se emplea* solo se podrá guardar una fecha de ingreso para un vendedor en cierto local. No permite guardar un historial de los ingresos de un vendedor en un mismo local.

B) Al modelar la fecha de ingreso en una entidad aparte como muestra la variante B, se puede mantener un historial de los ingresos de un vendedor en un local.

C) Dependiendo como se agrega la cardinalidad a la entidad horario se modelaran distintos casos.
* Si ambas cardinalidades son (1,n) se está indicando que un vendedor puede trabajar en un local en distintos horarios y que en un horario pueden haber varios vendedores en un local.
* Si existe una cardinalidad (1,1) del lado de la agregación, estamos indicando que para un horario hay asignado un único vendedor en el local
* Si la cardinalidad (1,1) se encuentra del lado del horario se indica que un vendedor puede trabajar en un único horario para un local.

## Ejercicio 5

Dada la transformación 1 a 1 el modelo de entidades y relaciones al modelo relacional,
responda si las siguientes afirmaciones son V o F:
A.​ La relación brinda tiene los atributos correspondientes y su clave está bien definida
B.​ La relación tiene tiene los atributos correspondientes y su clave está bien definida
C.​ La relación dicta tiene los atributos correspondientes y su clave está bien definida
D.​ La relación tiene no debería existir y los identificadores de la agregación deberían estar
en InfoCurso.
E.​ La relación dicta no debería existir y los atributos de Profesor deberían estar en
InfoCurso.

![[1-5.png]]

A) Verdadero. La transformación 1 a 1 de una relación (1,n) - (1,n) usa ambos identificadores en la relación.
B) False. La transformación de la relación (1,1) - (1,1) usa como clave alguna de las claves primarias de las entidades, no ambas.
C) Falso. La transformación de la relación (1,n) - (1,1) usa como clave primaria la clave de la entidad del lado (1,n)
D) Falso.
E) Falso. Si se agregan los atributos del profesor a InfoCurso, dado que un profesor dicta uno o varios cursos, vamos a tener información repetida.

<mark style="background: #FF5582A6;">Consulta</mark>

![[consulta.png]]

Me hace ruido que para dos grupos de cardinalidades distintos lleven a la misma conversión 1 a 1
Mirando solo el modelo físico no puedo interpretar cual de lo