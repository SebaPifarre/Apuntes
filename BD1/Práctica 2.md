INSTALACIONES (idCuidador, nyAp, cuilCuidador, idVivero, nombreVivero, mtrCuadradosVivero,
tempPromedioVivero, idPlanta, nombrePlanta, idEspecie, nombreEspecie, quimicoPlanta, consultorVivero,
patente_vehiculo)

· El idVivero es un identificador único que no se repite para diferentes viveros. Del vivero se conoce su nombre (diferentes viveros pueden tener el mismo nombre), los metros cuadrados que ocupa, la temperatura promedio que debe mantener y el cuidador responsable del mismo.
· Un mismo cuidador (idCuidador) puede cuidar diversos viveros. Tener en cuenta que un vivero tiene solamente un cuidador responsable asignado.
· Del cuidador se conoce su nombre y apellido y el cuil. El idCuidador no se repite para diferentes cuidadores.
· El idPlanta es único. Por ejemplo, una planta es el helecho. De cada planta se conoce el nombre de la planta y la especie a la que
pertenece.
· El idEspecie es único. Un ejemplo de especie es el árbol. Cada planta pertenece a una única especie y a una especie pertenecen
diversas plantas.
· A cada planta en un vivero (por ejemplo: helecho en el vivero 1) se le aplica un conjunto de químicos. Los mismos químicos se pueden aplicar a plantas de diferentes viveros y a diferentes plantas en el mismo vivero.
· Se cuenta con el registro de los vehículos de los viveros que se utilizan para traslados y repartos.
· Cada vivero tiene diversos consultores de viveros (consultorVivero), que son quienes asesoran ante dudas eventuales. El mismo consultor puede asesorar en diversos viveros.

#### Primero las dependencias funcionales

DF1) idCuidador -> nyAp, cuilCuidador
DF2) cuilCuidador -> nyAp, idCuidador
DF3) idVivero -> nombreVivero, mtrCuadradosVivero, tempPromedioVivero, idCuidador
DF4) idVivero -> nombreVivero, mtrCuadradosVivero, tempPromedioVivero, cuilCuidador
DF5) idEspecia -> nombreEspecie
DF6) idPlanta -> nombrePlanta, idEspecie

 CC: {idVivero, idPlanta, quimicoPlanta, consultorVivero, patente_vehiculo}

#### ¿Instalciones, cumple BCNF?

Instalaciones no se encuentra en BCNF ya que existe al menos el determinante de la DF5 
{idEspecie} que NO es superclave del esquema Instalaciones. Por lo tanto, particionamos por la DF6, creando dos nuevas relaciones:

I1(<u>idEspecie</u>, nombreEspecie)

I2(idCuidador, nyAp, cuilCuidador, <u>idVivero</u>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero, <u>idPlanta</u>, nombrePlanta, <u>quimicoPlanta</u>, <u>consultorVivero</u>, <u>patente_vehiculo</u>)

#### ¿Se pierden DFs?

No. En I2 valen DF1, DF2, DF3, DF4, DF6. I1 está en BCNF ya que DF5 vale en el esquema e {idEspecie} es superclave del esquema.

#### ¿Se pierde información?

No, ya que I1 interseccion I2 es {idEspecie}, clave de I1

I2 no está en BCNF ya que existe, al menos, {idCuidador} de la DF1 que NO es superclave del esquema. Por lo tanto, particionamos I2 por la DF1.

I3 (<u>idCuidador</u>, nyAp, cuilCuidador)

I4 (idCuidador, <u>idVivero</u>, nombreVivero, mtrCuadradosVivero, tempPromedioVivero, <u>idPlanta</u>, nombrePlanta, <u>quimicoPlanta</u>, <u>consultorVivero</u>, <u>patente_vehiculo</u>)

#### ¿Se pierden DFs?

No. En I4 valen DF3, DF4 y DF6. I3 está en BCNF ya que valen tanto DF1 como DF2 y tanto idCuidador como cuilCuidador son superclaves del esquema.

DF4 no se pierde, ya que es posible acceder con su determinante idViviero a todos sus determinados, ya se directa o indirectamente.

* nombreVivero, mtrCuadradosVivero y tempPromedioVivero se acceden directamente desde I4
* cuilCuidador no está en I4 pero no se pierde la df porque ya existe una relación indirecta a través de la dependencia funcional idVivero -> idCuidador y la dependencia idCuidador -> cuilCuidador. Por transitividad, se recupera.

#### ¿Se pierde info?

No, ya que I3 intersección I4 es idCuidador que es clave en I3

I4 no está en BCNF ya que existe idPlanta de la DF6 que no es superclave del esquema. Particionamos I4 por la DF6

I5 (<u>idPlanta</u>, nombrePlanta, idEspecie)
I6 (idCuidador, <u>idVivero</u>, nombreVivero, mtrCuadradoVivero, tempPromedioVivero, <u>idPlanta</u>, <u>quimicoPlanta</u>, <u>consultorVivero</u>, <u>patente_vehiculo</u>)

#### ¿Se pierden DFs?

No, en I5 vale DF6. I5 está en BCNF ya que {idPlanta} es superclave del esquema. 
No se pierde info ya que I5 interseccion I6 es {idPlanta}, superclave del esquema.

En I6 vale la DF3. I6 no está en BCNF ya que eiste {idVivero} de la DF3 que No es superclave del esquema. Por lo tanto, particionamos I 6 por la DF3

I7 (<u>idVivero</u>, nombreVivero, mtrCuadradoVivero, tempVivero, idCuidador)
I8 (<u>idVivero</u>, <u>idPlanta</u>, <u>quimicoPlanta</u>, <u>consultorVivero</u>, <u>patente_vehiculo</u>)

No se pierde info ya que I7 intersección I8 es {idVivero} clave de I7. En I7 vale DF3. I7 está en BCNF ya que {idVivero} es supercalve del esquema.

Notar que en I8 quedaron los atributos que conforman la clave. Con lo cual, cualquier DF que exista será trivial. Por eso puedo afirmar que I8 está en BCNF.


## Ejercicio 8

SUSCRIPCION (#suscripcion, email, nombre_usuario, # plan, nombre_plan,
texto_condiciones, precio, email_adicional, nombre_adicional, # contenido, titulo, sinopsis,
duracion, fecha_adicional)
Donde:
●​ Cada suscripción es realizada por un único usuario (identificado por el email) y un plan,
pero además hay usuarios adicionales que la utilizan (email_adicional). De cada usuario
adicional que se suma a la suscripción, se guarda la fecha.
●​ Un plan de suscripción tiene un nombre (que no puede garantizarse que sea único en el
sistema), condiciones, y un precio mensual.
●​ Cada contenido tiene un título, sinopsis y duración. El # contenido es único en el
sistema, pero del título no puede garantizarse que lo sea.
●​ De cada suscripción se sabe qué contenidos fueron reproducidos, sin distinción sobre
qué usuario (titular o adicionales) reprodujo cada uno.


#### Dependencias funcionales

DF1) # suscripcion -> email, # plan
DF2) email -> nombre_usuario
DF3) # plan -> nombre_plan, texto_condiciones, precio
DF4) email_adicional -> nombre_adicional
DF5) email_adicional, # suscripcion -> fecha_adicional
DF6) # contenido -> titulo, sinopsis, duracion

CC = {#suscripcion,email_adicional,#contenido}

Suscripcion no se encuentra en BCNF ya que existe al menos el determinante {email} de la DF2 que NO es superclave del esquema. Por lo tanto particionamos por la DF2, creando dos nuevas relaciones:

S1) (<u>email</u>, nombre_usuario)
S2) (<u>#suscripcion</u>,email,#plan,nombre_plan,texto_condiciones,precio,<u>email_adicional</u>,nombre_adicional,fecha_adicional,<u>#contenido</u>,titulo,sinopsis,duracion)

#### ¿Se perdio DFs?

No, ya que en S1 vale DF2. En S2 valen DF1, DF3, DF4, DF5 y DF6

