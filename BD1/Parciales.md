
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

E6 no está en BCNF ya que existe 