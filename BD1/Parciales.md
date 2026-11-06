
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