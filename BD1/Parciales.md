
## 1era fecha 05/11/2025

### Normalización

Busco dependencias funcionales:

DF1) #revista -> nombre_revista, periodicidad
DF2) #revista, #publicacion -> ejemplares_impresos
DF3) #punto_venta -> nombre_punto_venta, ubicacion_punto_venta, #contacto
DF4) #contacto -> nombre_apellido, direccion, telefono

CC: {#revista, #publicacion , #punto_venta , sponsor}

REVISTAS no está en BCNF ya que existe, al menos, el determinante #contacto de DF4 que no es superclave del esquema. Por lo tanto particiono a partir de DF4 generando dos relaciones nuevas:

E1 (<u>#contacto(sub)</u>, nombre_apellido, direccion, telefono)
E2 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), #contacto(sub), sponsor(sub)</u>, ejemplares_impresos, nombre_punto_venta, ubicacion_punto_venta, nombre_revista, periodicidad)

Se perdio info? No porque E1interE2 da #revista que es clave de E1
Se perdieron dfs? DF4 vale en E1. DF1,DF2 y DF3 valen en E2.
Por validación simple puedo decir que E1 está en BCNF ya que #contacto es superclave de E1

E2 no está en BCNF ya existe, al menos, el determinante #punto_venta de DF3 que no es superclave del esquema. Particiono en base a DF3 para generar dos nuevas relaciones:

E3 (<u>#punto_venta(sub)</u>, nombre_punto_venta, ubicacion_punto_venta, #contacto )
E4 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), sponsor(sub)</u>, ejemplares_impresos,  nombre_revista, periodicidad)

se perdió info? No porque E3interE4 da #punto_venta que es clave de E3
se pierden dfs? En E3 vale DF3 y en E4 valen DF1 y DF2.
E3 está en BCNF porque #punto_venta es superclave del esquema.

E4 no está en BCNF ya que existe, al menos, el determinante #revista de DF1 que no es superclave del esquema. Particiono E4 a partir de DF1 para obtener dos relaciones nuevas:

E5 (<u>#revista(sub)</u>, nombre_revista, periodicidad)
E6 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), sponsor(sub)</u>, ejemplares_impresos,)

se perdio info? No ya que E5interE6 da #revista que es clave de E5
se pierden dfs? En E5 vale DF1 y en E6 vale DF2
E5 está en BCNF ya que #revista es superclave del esquema

E6 no está en BCNF ya que existe el determinante #revista, #publicacion que no es superclave del esquema. Particiono E6 a partir de DF2, generando dos nuevas relaciones:

E7 (<u>#revista,#publicacion(todo sub)</u>, ejemplares_impresos)
E8 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), sponsor(sub)</u>,)

se pierde info? No ya que E7interE8 da #revista,#publicacion que es clave de E7
se pierden dfs? En E7 vale DF2
E7 está en BCNF ya que #revista,#publicacion es superclave del esquema

En E8 solo existen valores de la CC, por lo que cualquier df que se encuentre va a ser trivial en E8. E8 está en BCNF.

Me queda

E1 (<u>#contacto(sub)</u>, nombre_apellido, direccion, telefono)
E3 (<u>#punto_venta(sub)</u>, nombre_punto_venta, ubicacion_punto_venta, #contacto )
E5 (<u>#revista(sub)</u>, nombre_revista, periodicidad)
E7 (<u>#revista,#publicacion(todo sub)</u>, ejemplares_impresos)
E8 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), sponsor(sub)</u>,)

CP:{#punto_venta,#revista,#publicacion,sponsor}

ahora paso a los DMs

DM1) #publicacion, #revista -->> sponsor
DM2) #revista -->> #punto_venta 

En base a las DMs encontrados puedo decir que E8 no está en 4FN ya que existen dm1 y dm2 que no son trivales en E8. Pariciono E8 a partir de DM1

E9 (<u>#publicacion(sub), #revista(sub)</u>, sponsor)
E10 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub)</u>)

E9 está en 4FN porque vale DM1 que es trivial en ella
E10 está en 4FN porque vale DM2 que es trivial en ella

resultado

E1 (<u>#contacto(sub)</u>, nombre_apellido, direccion, telefono)
E3 (<u>#punto_venta(sub)</u>, nombre_punto_venta, ubicacion_punto_venta, #contacto )
E5 (<u>#revista(sub)</u>, nombre_revista, periodicidad)
E7 (<u>#revista,#publicacion(todo sub)</u>, ejemplares_impresos)
E9 (<u>#publicacion(sub), #revista(sub)</u>, sponsor)
E10 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub)</u>)

CP:{#punto_venta,#revista,#publicacion,sponsor}


```mysql
DECLARE TRIGGER actualizar_stock
AFTER INSERT
ON DEVOLUCION
BEGIN
	DECLARE cant INT;
	SET cant = new.cantidad_cajas * (select unidades por caja from PRODUCTO p
									 where p.id_producto = new.id_producto)
	
	UPDATE INVENTARIO SET (stock_unidades=stock_unidades + cant)
	WHERE id_bodega = new.id_bodega && id_producto = new.id_producto
END
```


A <- (lista #usuario, #permiso)(PERMISO_REQUERIDO |X| USUARIO_UTILIZA_SISTEMA)
B <- A - PERMISO_OBTENIDO
(lista #usuario) B - USUARIO_UILIZA_SISTEMA


## 2da fecha 27-11-2024

PARQUEDIVERSIONES (id_atractivo, nombre_atractivo, descripcion_atractivo, id_categoria_atractivo,
                   nombre_categoria, cuil_empleado, nombre_apellido_empleado, nro_legajo_empleado,
                   dia_semana, id_agente_mantenimiento)

Donde:

- De cada atractivo (identificado por id_atractivo) se conoce su nombre (nombre_atractivo), una descripción (descripción_atractivo) y las categorías a la que pertenece. Uno o más atractivos pueden tener el mismo nombre y/o descripción. Puede haber diferentes atractivos por categoría.
- El id de la categoría es un valor único (id_categoria_atractivo). De cada categoría se conoce su nombre. Un nombre de categoría puede repetirse para diferentes id de categoría.
- De cada empleado que trabaja en el parque de diversiones, se conocen además de sus datos personales (cuil_empleado, nombre_apellido_empleado) un número de legajo (nro_legajo_empleado) el cual, al igual que el cuil, es único en el sistema.
- En cada atractivo hay asignados diversos empleados, pero dado un atractivo y un empleado asignado se conoce el día de la semana que le corresponde.
- Un atractivo puede ser mantenido por distintos empleados en un día determinado.
- Un empleado puede tener asignado más de un atractivo por día.
- El parque de diversiones también dispone de personal de mantenimiento, que están disponibles para atender a los diversos atractivos del parque.


Busco dependencias funcionales y claves candidatas:

df1) id_atractivo -> nombre_atractivo, descripcion_atractivo
df2) id_categoria_atractivo -> nombre_categoria
df3) cuil_empleado -> nombre_apellido_empleado, nro_legajo_empleado
df4) nro_legajo_empleado -> nombre_apellido_empleado, cuil_empleado
df5) cuil_empleado, id_atractivo -> dia_semana
df6) nro_legajo_empleado, id_atractivo -> dia_semana

CC1 {id_atractivo, id_categoria_atractivo, cuil_empleado, id_agente_mantenimiento}
CC2 {id_atractivo, id_categoria_atractivo, nro_legajo_empleado, id_agente_mantenimiento}

PARQUEDIVERSIONES no está en BCNF ya que existe, al menos, el determinante {id_atractivo} que no es superclave del esquema. Por lo tanto particiono PARQUEDIVERSIONES a partir de df1, generando dos nuevas particiones:

P1 (<u>id_atractivo(sub)</u>, nombre_atractivo, descripcion_atractivo)
P2 (<u>id_atractivo(sub)</u>, <u>id_categoria_atractivo(sub)</u>, nombre_categoria, <u>cuil_empleado(sub)</u>, nombre_apellido_empleado, nro_legajo_empleado(otro sub), dia_semana, <u>id_agente_mantenimiento</u>)

no se pierde información ya que P1interP2 da id_atractivo que es clave de P1
df1 vale en P1. df2,df3,df4,df5y df6 valen en P2. No se pierden dfs.

En P1 se cumple que id_atractivo es superclave, entonces P1 está en bcnf.

P2 no está en bcnf ya que existe, al menos, el determinante de df2 {id_categoria_atractivo} que no es superclave del esquema. Por lo tanto, particiono P2 a partir de df2:

P3 (<u>id_categoria_atractivo(sub)</u>, nombre_categoria)
P4 (<u>id_atractivo(sub)</u>, <u>id_categoria_atractivo(sub)</u>, <u>cuil_empleado(sub)</u>, nombre_apellido_empleado, nro_legajo_empleado(otro sub), dia_semana, <u>id_agente_mantenimiento(sub)</u>)

No se pierde información ya que P3interP4 da id_categoria_atracion que es clave de P3
df3,df4,df5 y df6 valen en P4. df2 vale en P3 entonces no se pierden dfs. Además como id_categoria_atraccion es superclave del esquema, P3 está en bcnf.

P4 no está en bcnf ya que existe, al menos, el determinante {cuil_empleado} de la df 3 que no es superclave del esquema. Por lo tanto, particiono P4 a partir de df3:

P5 (<u>cuil_empleado(sub)</u>, nombre_apellido_empleado, nro_legajo_empleado(otro sub))
P6 (<u>id_categoria(sub</u>), <u>id_categoria_atractivo(sub)</u>, <u>cuil_empleado(sub)</u>, dia_semana, <u>id_agente_mantenimiento(sub)</u>)

No se pierde información ya que P5interP6 da cuil_empleado que es clave de P5

df3 y df4 son equivalentes ya que producen el mismo conjunto de atributos, entonces se da que ambas valen en P5.
Que pasa con df6? No se pierde ya que se puede obtener nro_legajo_empleado por transitividad a través de cuil_empleado -> nombre_apellido_emplenro_legajo_empleado

df5 vale en P6. No se pierden dfs

P5 está en bcnf ya que cuil_empleado es superclave del esquema

P6 no está en bcnf ya que existe, al menos, el determinante {cuil_empleado, id_atractivo} de la df5 que no es clave del esquema. Por lo tanto, particiono P6 a partir de df5 obteniendo dos relaciones nuevas:

P7 (<u>cuil_empleado(sub)</u>, <u>id_atractivo(sub)</u>, dia_semana)
P8 (<u>id_categoria(sub)</u>, <u>id_categoria_atractivo(sub)</u>, <u>cuil_empleado(sub)</u>, <u>id_agente_mantenimiento(sub)</u>)

No se pierde información ya que p7interp8 da {cuil_empleado, id_atractivo} que es clave de P7.
En p7 vale df5 y como cuil_empleado, id_atractivo es superclave del esquema, p7 está en bcnf.

Todos los atributos de P8 son parte de la CC1, entonces cualquier df que se encuentre será trivial en ella. P8 está en bcnf.


P1 (<u>id_atractivo(sub)</u>, nombre_atractivo, descripcion_atractivo)
P3 (<u>id_categoria_atractivo(sub)</u>, nombre_categoria)
P5 (<u>cuil_empleado(sub)</u>, nombre_apellido_empleado, nro_legajo_empleado(otro sub))
P7 (<u>cuil_empleado(sub)</u>, <u>id_atractivo(sub)</u>, dia_semana)
P8 (<u>id_categoria(sub)</u>, <u>id_categoria_atractivo(sub)</u>, <u>cuil_empleado(sub)</u>, <u>id_agente_mantenimiento(sub)</u>)

Clave: {id_atractivo, id_categoria_atractivo, cuil_empleado, id_agente_mantenimiento}

Busco dependencias multivaluadas

DM1) id_atractivo ->> id_categoria_atractivo
DM2) id_atractivo ->> cuil_empleado
DM3) vacio ->> id_agente_mantenimiento

P8 no está en 4FN ya que existe la DM3 que no es trivial en ella. Particiono a partir de dm3, obteniendo:

P9 (<u>id_agente_mantenimiento</u>)
P10 (<u>id_atractivo(sub)</u>, <u>id_categoria_atractivo(sub)</u>, <u>cuil_empleado(sub)</u>)

P9 está en 4FN ya que solo vale dm3 que es trivial en ella.

P10 no está en 4fn ya que existe DM2 que no es trivial en ella. Particino

P11 (<u>id_atractivo</u>, <u>cuil_empleado</u>)
P12 (<u>id_atractivo</u>, <u>id_categoria_atractivo</u>)

P11 está en 4fn ya que solo vale dm2 que es trivial en ella
P12 está en 4fb ya que solo vale dm1 que es trivial en ella

P1 (<u>id_atractivo(sub)</u>, nombre_atractivo, descripcion_atractivo)
P3 (<u>id_categoria_atractivo(sub)</u>, nombre_categoria)
P5 (<u>cuil_empleado(sub)</u>, nombre_apellido_empleado, nro_legajo_empleado(otro sub))
P7 (<u>cuil_empleado(sub)</u>, <u>id_atractivo(sub)</u>, dia_semana)
P9 (<u>id_agente_mantenimiento</u>)
<s>P11 (<u>id_atractivo</u>, <u>cuil_empleado</u>)</s>
P12 (<u>id_atractivo</u>, <u>id_categoria_atractivo</u>)

Se eliminó P11 por ser un proyección de P7

P1, P3, P5 y P7 están en 4fn porque no tienen dms.

## Ejercicio 11

DISPOSITIVOS (marca_id, descripMarca, modelo_id, descripModelo, equipo_tipo_id,
descripEquipoTipo, nombreEmpresa, cuit, direcciónEmpresa, usuario_id, apyn,
direcciónUsuario, cuil, plan_id, descripPlan, importe, equipo_id, imei, fec_alta, fec_baja,
observaciones, línea_id, fec_alta_linea, fec_baja_linea)

Donde:
●​ Para cada equipo interesa conocer su tipo, modelo, imei, fecha en que se dio de alta,
fecha en que se da de baja y las observaciones que sean necesarias.
●​ De cada marca se conoce su descripción
●​ De cada modelo se conoce su descripción y a qué marca pertenece.
●​ Para cada plan, se registra qué empresa lo brinda, descripción e importe del mismo.
●​ Para cada tipo de equipo se conoce la descripción
●​ Para cada empresa se registra el nombre, cuit y dirección
●​ De cada usuario se registra su nombre y apellido, número de documento, dirección y
CUIL
●​ Para cada línea se necesita registrar qué plan posee, la fecha de alta de la línea, la
fecha de baja, el equipo que la posee y el usuario de la misma.

Busco dependencias funcionales

df1) marca_id -> descripMarca
df2) modelo_id -> descripModelo, marca_id
df3) equipo_tipo_id -> descripEquipoTipo
df4) plan_id -> descripPlan, importe, cuit
df5) cuit -> nombreEmpresa, direccionEmpresa
df6) usuario_id -> apyn, direccionUsuario, cuil
df7) cuil -> apyn, direccionUsuario, usuario_id
df8) linea_id -> plan_id, fec_alta_linea, fec_baja_linea, equipo_id, usuario_id
df9) linea_id -> plan_id, fec_alta_linea, fec_baja_linea, equipo_id, cuil
df10) equipo_id -> equipo_tipo_id, modelo_id, imei, fec_alta_linea, fec_baja_linea, observaciones

## 2da Fecha 2023

TORNEOS (#torneo, nombre_torneo, año, #equipo, nombre_equipo, estadio_equipo, puesto,
         #reglamentacion, descripcion, #auspiciante)

- De cada torneo, se conoce su identificador (#torneo, único en el sistema) y un nombre. Un mismo torneo tiene diferentes ediciones, cada edición se realiza en un año determinado y el mismo torneo no puede repetirse el mismo año. En un año pueden realizarse varios torneos.
- Cada edición de un torneo tiene diferentes auspiciantes, identificados por #auspiciante (único en el sistema).
- En cada edición de un torneo participan varios equipos. De cada equipo se conoce su nombre, su estadio y su #equipo, que no se repite para diferentes equipos.
- Cada equipo finaliza una edición de un torneo en un puesto. Dos o más equipos no pueden finalizar en un mismo puesto.
- Además, se conoce un conjunto de reglamentaciones, identificadas por #reglamentación, aplicables a estos torneos.


Busco las dependencias funcionales:

