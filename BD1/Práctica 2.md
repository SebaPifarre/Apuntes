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
S1 está en BCNF ya que email es superclave del esquema

#### ¿Se perdió info?

No ya que S1 intersección S2 da {email} que es clave del esquema

S2 no se encuentra en BCNF ya que existe al menos el determinante # plan del DF3 que no es superclave del esquema. Por lo tanto particionamos por la DF3, creando dos nuevas relaciones:

S3) (<u>#plan(sub)</u>, nombre_plan, texto_condiciones, precio)
S4) (<u>#subscripcion(sub)</u>, email,#plan,<u>email_adicional(sub)</u>,nombre_adicional,fecha_adicional,<u>#contenido(sub)</u>,titulo,sinopsis,duracion)

#### ¿Se perdieron DFs?

No ya que en S3 vale DF3. En S4 valen DF1,DF4,DF5,DF6
S3 está en BCNF ya que {#plan} es superclave del esquema

#### ¿Se perdió info?

no ya que S3interseccionS4 da {#plan} que es clave del esquema

S4 no está en BCNF ya que existe al menos un determinante {email_adicional} de DF4 que no es superclave del esquema. Por lo tanto particionamos por la DF4, creando dos nuevas realciones:


S5 (<u>email_adicional(sub)</u>, nombre_adicional)
S6 (<u>#subscripcion(sub)</u>, email,#plan,<u>email_adicional(sub)</u>,fecha_adicional,<u>#contenido(sub)</u>,titulo,sinopsis,duracion)

como S5IntersecciónS6 da {email_adicional} que es clave del esquema S5 no se pierde información

DF4 vale en S5. DF1,DF5 y DF6 valen en S6
S5 está en BCNF ya que {email_adicional} es superclave del esquema S5

S6 no está en BCNF ya que existe al menos un determinante {#contenido} de DF6 que no es superclave del esquema S6. Por lo tanto particionamos por la DF6, creando dos nuevas relaciones:

S7 (<u>#contenido(sub)</u>,titulo,sinopsis,duracion)
S8 (<u>#suscripcion(sub)</u>,email,#plan,<u>email_adicional(sub)</u>,fecha_adicional,<u>#contennido(sub)</u>)

como S7interseccionS8 da {#contenido} que es clave del esquema S7 entonces no se pierde información

DF6 vale para S7. DF1, DF5 valen para S8 entonces no se pierden DFs
S7 está en BCNF ya que {#contendio} es clave del esquema S7

S8 no está en BCNF ya que existe al menos la DF1, cuyo determinante {#suscripcion} NO es superclave del esquema. Por lo tanto particionamos por la DF1, creando dos nuevas relaciones:

S9 (<u>#suscripcion(sub)</u>, email,#plan)
S10 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>,fecha_adicional,<u>#contennido(sub)</u>)

No se pierde información ya que S9intersecciónS10 da {#suscripcion} que es clave del esquema S9.

No se pierden Dfs ya que DF1 vale para S9 y DF5 vale para S10
S9 está en BCNF ya que {#suscripcion} es clave del esquema S9

S10 no está en BCNF ya que existe DF5, cuyo determinante {#suscripcion, email_adicional} NO es superclave del esquema. Entonces particionamos por la DF5, creando dos nuevas relaciones:

S11 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>, fecha_adicional)
S12 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>,<u>#contennido(sub)</u>)

No se pierde información ya que S11IntersecciónS12 da {#suscripcion,email_adicional} que es clave del esquema S11

No se pierden dfs ya que DF5 vale para S11
S11 está en BCNF ya que {#suscripcion,email_adicional} es clave del esquema s11

En S11 todos los atributos forman parte de la clave, cualquier dependencia funcional que detectemos va a ser trivial. S11 cumple con BCNF

S1 (<u>email</u>, nombre_usuario)
S3 (<u>#plan(sub)</u>, nombre_plan, texto_condiciones, precio)
S5 (<u>email_adicional(sub)</u>, nombre_adicional)
S7 (<u>#contenido(sub)</u>,titulo,sinopsis,duracion)
S9 (<u>#suscripcion(sub)</u>, email,#plan)
S11 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>, fecha_adicional)
S12 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>,<u>#contennido(sub)</u>)


Para S12 valen

DM1) #suscripcion -->>  #contenido 
DM2) #suscripcion -->> email_adicional

El esquema S12 no está en 4FN porque existen dependencis multivaluadas 1-2 que no son triviales en S12
Entonces se particiona S12 utilizando una de las dependencias multivaluadas, DM1

S13 (<u>#suscripcion(sub)</u>,<u>#contennido(sub)</u>)
S14 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>) 

La partición S13 está en 4FN ya que sólo vale la dependencia multivaluada 1 que es trivial en ella.
La partición S14 está en 4FN ya que sólo vale la dependencia multivaluada 2 que es trivial en ella.

S1 (<u>email</u>, nombre_usuario)
S3 (<u>#plan(sub)</u>, nombre_plan, texto_condiciones, precio)
S5 (<u>email_adicional(sub)</u>, nombre_adicional)
S7 (<u>#contenido(sub)</u>,titulo,sinopsis,duracion)
S9 (<u>#suscripcion(sub)</u>, email,#plan)
S11 (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>, fecha_adicional)
S13 (<u>#suscripcion(sub)</u>,<u>#contennido(sub)</u>)

S14 se elimina del esquema por ser proyección de S11

-CP (<u>#suscripcion(sub)</u>, <u>email_adicional(sub)</u>,<u>#contennido(sub)</u>)

S1,S3,S5,S7,S9 y S11 están en 4FN porque no tienen dependencias multivaluadas.


## Ejercicio 7

MEDICION_AMBIENTAL(#medicion, #pozo, valor_medicion, #parametro, fecha_medicion,
cuil_operario, #instrumento, nombre_parametro, valor_ref, descripcion_pozo,
fecha_perforacion, apellido_operario, nombre_operario, fecha_nacimiento, marca_instrumento, modelo_instrumento, dominio_vehiculo, fecha_adquisicion)
Donde:

●​ Cada medición es realizada por un operario en un pozo, en una fecha determinada. En
ella se miden varios parámetros, y para cada uno se obtiene un valor. Notar que un
mismo parámetro (#parametro) puede ser medido en diferentes mediciones.
Independientemente de las mediciones, todo parámetro tiene un nombre y valor de
referencia, y el #parametro es único en el sistema.
●​ En cada medición se utilizan varios instrumentos, independientemente de los
parámetros medidos. De cada instrumento se conoce la marca y modelo.
●​ De cada operario se conoce su cuit, nombre, apellido y fecha de nacimiento.
●​ La empresa cuenta con vehículos, y de cada uno se conoce la fecha en la que fue
adquirido. El dominio (patente) de cada vehículo es único en el sistema.
●​ Un pozo tiene una descripción y una fecha de perforación. El identificador #pozo es
único en el sistema.


DF1) #paramentro -> nombre_pametro, valor_ref
DF2) #instrumento -> marca_instrumento, modelo_instumento
DF3) dominio_vehiculo -> fecha_adquisicion
DF4) #pozo -> descripción_pozo, fecha_perforación
DF5) #medicion -> cuil_operario, fecha_medicion, #pozo
DF6) cuil_operario -> nombre_operario, apellido_operario, fecha_nacimiento
DF7) #medicion, #parametro  -> valor_medicion

CC {#parametro, #instrumento, dominio_vehiculo, #medicion}

Medicion_ambiental no está en BCNF ya que existe por lo menos DF6 donde el determinante {cuil_operario} no es superclave del esquema Medicion_ambiental. Por lo tanto debemos particionar usando DF6, generando dos nuevas relaciones:

M1) (<u>cuil_operario(sub)</u>, nombre_operario, apellido_operario, fecha_nacimiento)
M2) (<u>#parametro(sub)</u>, nombre_parameto, valor_ref, <u>#instrumento(sub)</u>, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo(sub)</u>, fecha_adquisicion, #pozo, descripcion_pozo, fecha_perforacion, <u>#medicion(sub)</u>, cuil_operario, fecha_medicion valor_medicion)

No se pierde información porque M1interM2 da {cuil_operario} que es clave de M1

No se pierden DFs. DF6 vale en M1.
DF1,DF2,DF3,DF4, DF5 y DF7 valen en M2
M1 está en BCNF ya que DF6 vale en el esquema y {cuil_operario} es superclave del esquema.

M2 no está en BCNF ya que existe, al menos, {dominio_vehiculo} de la DF3 que no es superclave del esquema. Por lo tanto, perticionamos M2 por la DF3

M3 (<u>dominio_vehiculo(sub)</u>, fecha_adquisicion)
M4 (<u>#parametro(sub)</u>, nombre_parameto, valor_ref, <u>#instrumento(sub)</u>, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo(sub)</u>, #pozo, descripcion_pozo, fecha_perforacion, <u>#medicion(sub)</u>, cuil_operario, fecha_medicion valor_medicion)

No se pierde información ya que M3interM4 da {dominio_vehiculo} que es clave de M3

No se pierden DFs. DF3 vale en M3. DF1, DF2, DF4, DF5 y DF7 valen en M4.
M3 está en BCNF ya que {dominio_vehiculo} es superclave del esquema y DF3 vale en el esquema.

M4 no está en BCNF ya que existe, por lo menos, {#pozo} de DF4 que no es superclave del esquema. Por lo tanto, particionemos M4 con DF4

M5 (<u>#pozo(sub)</u>, descripcion_pozo, fecha_perforacion)
M6 (<u>#parametro(sub)</u>, nombre_parameto, valor_ref, <u>#instrumento(sub)</u>, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo(sub)</u>, #pozo,  <u>#medicion(sub)</u>, cuil_operario, fecha_medicion valor_medicion)

No se pierde información ya que M5interM6 da {#pozo} que es clave de M5
No se pierden dfs. DF4 vale en M5. DF1,DF2,DF5 y DF7 valen en M6
M5 está en BCNF ya que {#pozo} es superclave del esquema y vale DF4

M6 no está en BCNF porque existe, al menos, {#parametro} de DF1 que no es superclave del esquema. Por lo tanto particionamos M6, generando dos nuevas relaciones:

M7 (<u>#parametro(sub)</u>, nombre_parametro, valor_ref)
M8 (<u>#parametro(sub)</u>, <u>#instrumento(sub)</u>, marca_instrumento, modelo_instrumento, <u>dominio_vehiculo(sub)</u>, #pozo,  <u>#medicion(sub)</u>, cuil_operario, fecha_medicion valor_medicion)

No se pierde información porque M7interM8 da {#parametro} que es clave de M7
No se pierden dfs. DF1 vale en M7. DF2,DF5,DF7 valen en M8
M7 está en BCNF ya que {#parametro} es superclave del esquema y vale DF1

M8 no está en BCNF ya que existe, al menos, {#instrumento} de DF2 que no es superclave del esquema. Por lo tanto, particionamos M8 en DF2 y obtenemos dos nuevas relaciones:

M9 (<u>#instrumento(sub)</u>, marca_intrumento, modelo_instrumento)
M10 (<u>#parametro(sub)</u>, <u>#instrumento(sub)</u>, <u>dominio_vehiculo(sub)</u>, #pozo,  <u>#medicion(sub)</u>, cuil_operario, fecha_medicion valor_medicion)

No se pierde info porque M9interM10 da {#instrumento} que es clave de M9
No se pierden dfs ya DF2 vale en M9 y DF5 y DF7 valen en M10
M9 está en BCNF ya que {#instrumento} es superclave del esquema y vale DF2

M10 no está en BCNF ya que existe, al menos, {#medicion} de DF5 que No es superclave del esquema. Por lo tanto, particionamos M10 con DF5 obteniendo dos nuevas relaciones:

M11 (<u>#medicion(sub)</u>, cuil_operario, fecha_medicion, #pozo)
M12 (<u>#parametro(sub)</u>, <u>#instrumento(sub)</u>, <u>dominio_vehiculo(sub)</u>, <u>#medicion(sub)</u>, valor_medicion)

No se pierde información ya que M11interM12 da {#medicion} que es clave de M11
No se pierden dfs porque DF5 vale en M11 y DF7 vale en M12
M11 está en BCNF ya que {#medicion} es superclave del esquema y vale DF5

M12 no está en BCNF ya que existe, al menos, {#medicion,#parametro} de DF7 que no es superclave del esquema. Entonces particionamos M12 con DF7 generando dos nuevas relaciones:

M13 (<u>#medicion, #parametro(sub)</u>, valor_medicion)
M14 (<u>#parametro(sub)</u>, <u>#instrumento(sub)</u>, <u>dominio_vehiculo(sub)</u>, <u>#medicion(sub)</u>)

No se pierde información ya que M13interM14 da {#medicion,#parametro} que es clave de M13
No se pierden dfs porque DF7 vale en M13
M13 está en BCNF porque {#medicion,#parametro} es superclave del esquema y vale DF7.

En M14 todos los atributos son parte de la CC. Cualquier df que encontremos va a ser trivial. M14 está en BCNF.

M1) (<u>cuil_operario(sub)</u>, nombre_operario, apellido_operario, fecha_nacimiento)
M3 (<u>dominio_vehiculo(sub)</u>, fecha_adquisicion)
M5 (<u>#pozo(sub)</u>, descripcion_pozo, fecha_perforacion)
M7 (<u>#parametro(sub)</u>, nombre_parametro, valor_ref)
M9 (<u>#instrumento(sub)</u>, marca_intrumento, modelo_instrumento)
M11 (<u>#medicion(sub)</u>, cuil_operario, fecha_medicion, #pozo)
M13 (<u>#medicion, #parametro(sub)</u>, valor_medicion)
M14 (<u>#parametro(sub)</u>, <u>#instrumento(sub)</u>, <u>dominio_vehiculo(sub)</u>, <u>#medicion(sub)</u>)

-CP {<u>#parametro,#instrumento,dominio_vehiculo,#medicion</u>}

DM1) #parametro , #medicion -> #instrumento 
