
## 1era fecha 05/11/2025

### Normalización

Busco dependencias funcionales:

DF1) #revista -> nombre_revista, periodicidad
DF2) #revista, #publicacion -> ejemplares_impresos
DF3) #punto_venta -> nombre_punto_venta, ubicacion_punto_venta, #contacto
DF4) #contacto -> nombre_apellido, direccion, telefono

CC: {#revista, #publicacion , #punto_venta , sponsor}

REVISTAS no está en BCNF ya que existe, al menos, #contacto de DF4 que no es superclave del esquema. Por lo tanto particiono a partir de DF4 generando dos relaciones nuevas:

E1 (<u>#revista(sub)</u>, nombre_revista, periodicidad)
E2 (<u>#revista(sub), #publicacion(sub), #punto_venta(sub), #contacto(sub)</u>, ejemplares_impresos, nombre_punto_venta, ubicacion_punto_venta, nombre_apellido, direccion, 