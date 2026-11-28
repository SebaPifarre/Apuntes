
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
Que pasa con df6? No se pierde ya que se puede obtener nro_legajo_empleado por transitividad a través de la DF5  cuil_empleado -> nombre_apellido_emplenro_legajo_empleado

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

df1) #torneo -> nombre_torneo
df2) #equipo -> nombre_equipo, estadio_equipo
df3) #equipo, #torneo , año -> puesto
df4)  #torneo, puesto, año -> #equipo
df5) #reglamentacion -> descripcion

CC1 {#torneo, #equipo, año, #reglamentacion , #auspiciante }
CC2 {#torneo, año, puesto, #reglamentacion , #auspiciante }

TORNEOS no está en bcnf ya que existe, al menos, el determinante de la df1 #torneo que no es superclave del esquema. Por lo tanto particiono TORNEOS a partir de df1, obteniendo dos relaciones:

T1 (<u>#torneo(sub)</u>, nombre_torneo)
T2 (<u>#torneo(sub)(otro sub)</u>, <u>#equipo(sub)</u>, nombre_equipo, estadio_equipo, <u>año(sub)</u>, puesto(otro sub), <u>#reglamentacion(sub)(otro sub)</u>, <u>#auspiciante(sub)(otro sub)</u>, descripcion )

No se pierde información ya que T1interT2 da #torneo que es clave del esquema.
No se pierden dfs. df2, df3, df4 y df5 valen en T2. df1 vale en T1 y como #torno es superclave del esquema puedo afirmar que T1 está en bcnf.

T2 no está en bcnf ya que existe, al menos, el determinante #reglamentacion de df5 que no es superclave del esquema. Por lo tanto, particiono T2 a partir de df5 generando dos nuevas relaciones:

T3 (<u>#reglamento(sub)</u>, descripcion)
T4 (<u>#torneo(sub)(otro sub)</u>, <u>#equipo(sub)</u>, nombre_equipo, estadio_equipo, <u>año(sub)</u>, puesto(otro sub), <u>#reglamentacion(sub)(otro sub)</u>, <u>#auspiciante(sub)(otro sub)</u>)

No se pierde información ya que T3interT4 da #reglamento que es clave del esquema.
No se pierden dfs. En T4 valen df2,df3 y df4. En T3 vale df5, y como #reglamento es superclave del esquema puedo decir que T3 está en bcnf.

T4 no está en bcnf ya que existe, al menos, el determinante {#equipo, #torneo, año} de la df3 que no es superclave del esquema. Por lo tanto, particiono T4 a partir de df3 generando dos nuevas relaciones:

T5 (<u>#equipo, #torneo, año</u>, puesto)
T6 (<u>#torneo(sub)(otro sub)</u>, <u>#equipo(sub)</u>, nombre_equipo, estadio_equipo, <u>año(sub)</u>, <u>#reglamentacion(sub)(otro sub)</u>, <u>#auspiciante(sub)(otro sub)</u>)

No se pierde información ya que T5interT6 da {#equipo,#torneo,año} que es suplerclave del esquema.
No se pierden dfs. Como df3 y df4 son equivalentes(ambos determinantes determinan el mismo conjunto de atributos) ambas dfs valen en T5. df2 vale en T6.
Como se eligió particionar sobre df3, se define en este punto la CC1 como clave del esquema.
Como {#equipo,#torneo,año} puedo decir que T5 está en bcnf

T6 no está en bcnf ya que existe, al menos, el determinante #equipo de la df2 que no es superclave del esquema. Por lo tanto particiono a partir de df2 obteniendo dos relaciones nuevas:

T7 (<u>#equipo(sub)</u>, nombre_equipo, estadio_equipo)
T8 (<u>#torneo(sub)(otro sub)</u>, <u>#equipo(sub)</u>, <u>año(sub)</u>, <u>#reglamentacion(sub)(otro sub)</u>, <u>#auspiciante(sub)(otro sub)</u>)

No se pierde información ya que T7interT8 da #equipo que es superclave del esquema.
No se pierden dfs ya que df2 vale en T7 y como #equipo es clave de T7 puedo decir que T7 está en bcnf.

Todos los atributos de T8 forman parte de la CC1 entonces, cualquier df que se encuentre será trivial en T8 -> T8 está en bcnf.


## 2da fecha 29-11-2025


VIAJES (#viaje, origen, destino, #conductor, #patente, marca, modelo, fecha_alta, monto, nombre_cond,
        apellido_cond, telefono_cond, telefono_sucursal, ubicación_sucursal, #sucursal, #seguro_medico)

Donde:

- De cada vehículo que la empresa registra se conoce su patente, marca, modelo y la fecha de alta del vehículo en la compañía.
- La empresa contrata a diversos conductores para que hagan uso de estos vehículos. De estos se conoce un #conductor (que no se repite para diferentes conductores), su nombre, apellido y un teléfono de contacto (podría repetirse para diferentes conductores).
- Cada conductor realiza una serie de viajes identificados con #viaje, el cual es número secuencial para cada conductor. Es decir, cada conductor comienza por el #viaje igual a 1. De cada viaje se conoce el origen y el destino del mismo, el monto (valor del viaje) y el vehículo de la empresa con el que se realizó el mismo.
- La empresa cuenta con varias sucursales físicas. De ellas se conoce el #sucursal (no se repite para diferentes sucursales), la ubicación (no se repite; se sabe que dos sucursales no pueden estar en la misma ubicación) y un número de teléfono (podría repetirse para diferentes sucursales).
- Cada conductor está ligado a distintas sucursales y cada sucursal posee un conjunto de conductores para sus viajes. Es necesario registrar qué conductores tiene cada sucursal y para qué sucursales trabaja un conductor.
- Además, la empresa cuenta con diferentes seguros médicos aplicables a los conductores, cada uno identificado con #seguro_medico.

Busco las dependencias funcionales:

df1) #patente -> marca, modelo, fecha_alta
df2) #conductor -> nombre_cond, apellido_cond, telefono_cond
df3) #viaje, #conductor -> origen, destino, monto #patente 
df4) #sucursal -> ubicacion_sucursal, telefono_sucursal
df5) ubicacion_sucursal -> #sucursal, telefono_sucursal

CC1 {#conductor, #viaje, #sucursal, #seguro_medico}
CC2 {#conductor, #viaje, ubicacion_sucursal, #seguro_medico }

VIAJES no está en BCNF ya que existe, al menos, el determinante #patente de la df1 que no es superclave del esquema. Por lo tanto, particiono VIAJES a partir de df1 generando dos nuevas relaciones:

V1 (<u>#patente(sub)</u>, marca, modelo, fecha_alta)
V2 (<u>#patente(sub)</u>, <u>#conductor(sub)</u>, nombre_cond, apellido_cond, telefono_cond, <u>#viaje(sub)</u>, origen, destino, monto, <u>#sucursal(sub)</u>, ubicacion_sucursal, telefono_sucursal, #seguro_medico )

No se pierde info ya que V1interV2 da #patente que es clave de V1
En V1 vale la df1. Df2, df3, df4 y df5 valen en V2.
Como #patente es superclave del esquema, V1 está en BCNF

V2 no está en BCNF ya que existe, al menos, el determinante {#sucursal} de la df 4 que no es superclave del esquema. Por lo tanto, particiono V2 a partir de df4 genrando dos relaciones nuevas:

V3 (<u>#sucursal(sub)</u>, ubicacion_sucursal(otro sub), telefono_sucursal)
V4 (<u>#patente(sub)</u>, <u>#conductor(sub)</u>, nombre_cond, apellido_cond, telefono_cond, <u>#viaje(sub)</u>, origen, destino, monto, <u>#sucursal(sub)</u>, #seguro_medico )

No se pierde info ya que V3interV4 da #sucursal que es clave de V3.
Se da que como df4 y df5 son equivalentes, ambas valen en V3. df2 y df3 valen en V4.
Como #sucursal es superclave del esquema, V3 está en BCNF

V4 no está en BCNF ya que existe, al menos, el determinante {#conductor} de la df2 que no es superclave del esquema. Por lo tanto, particiono V4 a partir de la df2 obteniendo dos nuevas relaciones:

V5 (<u>#conductor(sub)</u>, nombre_cond, apellido_cond, telefono_cond)
V6 (<u>#patente(sub)</u>, <u>#conductor(sub)</u>, <u>#viaje(sub)</u>, origen, destino, monto, <u>#sucursal(sub)</u>, #seguro_medico )

No se pierde info ya que V5interV6 da #conductor que es clave de V5.
df2 vale en V5. df3 vale en v6. no se pierden dfs
Como #conductor es superclave del esquema, V5 está en BCNF

V6 no está en bcnf ya que existe el determinante {#viaje, #conductor} que no es superclave del esquema. Por lo tanto, particiono V6 a partir de df3 generando dos nuevas relaciones:

V7 (<u>#viaje(sub)</u>, <u>#conductor(sub)</u>, origen, destino monto, #patente )
V8 (<u>#conductor(sub)</u>, <u>#viaje(sub)</u>, <u>#sucursal(sub)</u>, #seguro_medico )

No se pierde info. A pesar de que #patente ya no aparece como determinante. Se pueden obtener todos los determinados por #patente con transitividad a través del determinante {#viaje, #conductor}.
No se pierden dfs, df3 vale en V7.
Como {#viaje, #conductor} es superclave del esquema, entonces V7 está en BCNF.

Todos los atributos de V8 forman parte de la CC1, por lo tanto cualquier df que se encuentre será trivial en V8. V8 está en BCNF. CC1 queda como clave del esquema sobre CC2.

## Primera fecha 2024

Busco las dfs

df1) #poliza -> tipo_seguro, fecha_inicio, fecha_fin, #cliente
df2) #poliza -> tipo_seguro, fecha_inicio, fecha_fin, cuil
df3) #cliente -> cuil, nombre, direccion
df4) cuil -> #cliente, nombre, direccion
df5) #siniestro -> fecha_denuncia, estado_siniestro

CC = {#poliza, #siniestro, #cuota, detalle_reparacion, #perito}

POLIZAS no está en BCNF ya que existe, al menos, el determinante #cliente de la df3 que no es superclave del esquema. Por lo tanto, particiono POLIZAS a partir de df3:

P1 (<u>#cliente</u>, cuil, nombre, direccion)
P2 (<u>#poliza</u>, tipo_seguro, fecha_inicio, fecha_fin, #cliente, <u>#siniestro</u>, fecha_denuncia, estado_siniestro, <u>#cuota</u>, <u>detalle_reparacion</u>, <u>#perito</u>)

No se pierde info ya que P1 inter P2 da #cliente que es superclave de P1.
Como se da que df3 y df4 son equivalentes(determinan el mismo conjunto de atributos, independientemente de cual es el determinante), ambas valen en P1 siendo #cliente y cuil ambas claves del esquema. 
df5 vale en P2.
Que pasa con df2?
Como cuil ya no aparece en P2 podría decise que df2 no vale en P2 pero se puede usar transitividad a partir de df1. Como #poliza -> #cliente -> cuil. Por lo tanto no se pierde df2 y df1 vale en P2. No se pierden dfs.
Como df4 y df3 valen en P1, no son triviales y #cliente es superclave del esquema. Entonces P1 está en BCNF.

P2 no está en BCNF ya que existe, al menos, el determinante {#siniestro} de la df5 que no es superclave del esquema. Por lo tanto, particiono P2 a partir de df5:

P3 (<u>#siniestro</u>, fecha_denuncia, estado_siniestro)
P4 (<u>#poliza</u>, tipo_seguro, fecha_inicio, fecha_fin, #cliente, <u>#siniestro</u>, <u>#cuota</u>, <u>detalle_reparacion</u>, <u>#perito</u>)

No se pierde info ya que P3interP4 da #siniestro que es superclave de P3
No se pierden dfs. df5 vale en P3 y df2 vale en P4.
Como #sinietro es clave del esquema P3 está en BCNF.

P4 no está en BCNF ya que existe el determinante #poliza de la df2 que no es superclave del esquema. Entonces particiono a partir de df2:

P5 (<u>#poliza</u>, tipo_seguro, fecha_inicio, fecha_fin, #cliente)
P6 (<u>#poliza</u>, <u>#siniestro</u>, <u>#cuota</u>, <u>detalle_reparacion</u>, <u>#perito</u>)

No se pierde info
No se pierden dfs
P5 está en bcnf.

Como todos los atributos de P6 forman parte de la clave y cualquier df que se encuentre será trivial en P6 entonces P6 está en BCNF.

Busco dms:

dm1) #poliza ->> #cuota 
dm2) #sinietro  ->> #poliza 
dm3) #sinietro ->> #perito
dm4) #siniestro, #poliza ->> detalle_reparacion

## Primera fecha 2025

REVISTAS (#revista, #publicación, #punto_venta, #contacto, sponsor, nombre_revista, periodicidad,
          nombre_punto_venta, ubicación_punto_venta, nombre_apellido, dirección, telefono,
          ejemplares_impresos)

- De cada revista que la empresa reparte se conoce un #revista (único en el sistema), el nombre de la revista y la periodicidad de publicación (por ejemplo, semanal, quincenal o mensual).
- Cada revista publica diferentes publicaciones numeradas secuencialmente dentro de cada revista, es decir, cada revista comienza por el #publicación igual a 1. De cada publicación de una revista se conoce la cantidad de ejemplares impresos (ejemplares_impresos).
- Una publicación de una revista puede tener varios sponsors. Un sponsor es una empresa que paga publicidad en publicaciones de revistas. Notar que un sponsor puede publicitar en varias publicaciones de diversas revistas.
- Cada revista se ofrece en diversos puntos de venta (#punto_venta) y en un punto de venta pueden venderse diversas revistas.
- De cada punto de venta, identificado por #punto_venta, se sabe su nombre, ubicación y se conoce un contacto. En una misma ubicación puede haber más de un punto de venta.
- De cada contacto de un punto de venta se registra un #contacto (único en el sistema), el nombre, dirección y teléfono. Notar que el contacto puede repetirse en diferentes puntos de ventas.

Busco dfs:

df1) #revista -> nombre_revista, periodicidad
df2) #revista, #publicacion -> ejemplares_impresos
df3) #punto_venta -> nombre_punto_venta, ubicacion_punto_venta, #contacto 
df4) #contacto -> nombre_apellido, direccion, telefono

CC = {#revista,#publicacion, #punto_venta , sponsor}

REVISTAS no está en BCNF ya que existe, al menos, el determinante #revista de la df1 que no es superclave del esquema. Por lo tanto, particiono REVISTAS a partir de la df1:

R1 (<u>#revista</u>, nombre_revista, periodicidad)
R2 (<u>#revista</u>, <u>#publicacion</u>, <u>#punto_venta</u>, <u>sponsor</u>, #contacto , nombre_punto_venta, ubicacion_punto_venta, nombre_apellido, direccion, telefono )

No se pierde info ya que R1interR2 da #revista que es clave de R1.
No se pierden dfs. df1 vale en R1. df2,df3 y df4 valen en R2.
R1 está en bcnf ya que df1 vale en el esquema y #revista es superclave del esquema.

R2 no está en bcnf ya que existe, al menos, el determinante {#contacto} de la df4 que no es superclave del esquema. Por lo tanto, particiono a partir de la df4:

R3 (<u>#contacto</u>, nombre_apellido, direccion, telefono)
R4 (<u>#revista</u>, <u>#publicacion</u>, <u>#punto_venta</u>, <u>sponsor</u>, #contacto , nombre_punto_venta, ubicacion_punto_venta )

No se pierde info ya que R3interR4 da {#contacto} que es superclave del esquema.
No se pierden dfs. df4 vale en R3 y df2 con df3 valen en R4.
R3 está en bcnf ya que solo vale la df no trivial df4 y #contacto es superclave de dicho esquema. 

R4 no está en bcnf ya que existe, al menos, el determinante #punto_