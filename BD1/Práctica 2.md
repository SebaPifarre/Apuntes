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
DF2) cuilCuidador 
DF2) idVivero -> nombreVivero, mtrCuadradosVivero, tempPromedioVivero, idCuidador
DF3) idPlanta -> nombrePlanta, idEspecie
DF4) idEspecia -> nombreEspecie
DF5) 


