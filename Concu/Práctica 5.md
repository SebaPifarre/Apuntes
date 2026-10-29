
## Consideraciones

![[Consideracions-ada-1.png]]

![[consideracions-ada-2.png]]

## Ejercicio 1

Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso.
El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2
unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de
vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas,
recursos y sincronizaciones serán necesarios/convenientes para resolverlo.
a. Realice la solución suponiendo que todos los vehículos tienen la misma prioridad.
b. Modifique la solución para que tengan mayor prioridad los camiones que el resto de los
vehículos.

```ada
Task Puente is
	entry EntrarAuto
	entry EntrarCamioneta
	entry EntrarCamion
	entry Salir(P: IN integer)

Task Type Auto
Task Type Camioneta
Task Type Camion

Task body Puente is
	peso:integer;
Begin
	peso=5
	loop 
		SELECT
			when(peso>=1 and EntrarCamion'count = 0)  => ACCEPT EntrarAuto do
				peso := peso-1
		OR
			when(peso>=2 and EntrarCamion'count = 0) => ACCEPT EntrarCamioneta do
				peso := peso-2
		OR
			when(peso>=3) => ACCEPT EntrarCamion do
				peso := peso-3
		OR
			ACCEPT Salir(P: IN integer) do
				peso := peso + P
		END SELECT
	end loop
end Puente

Task body Auto is
	Puente.EntrarAuto
	//cruza
	Puente.salir(1)

Task body Camioneta is
	Puente.EntrarCamioneta
	//cruza
	Puente.Salir(2)
	
Task body Camion is
	Puente.EntrarCamion
	//cruza
	Puente.Salir(3)
```

## Ejercicio 2

