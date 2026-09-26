## Ejercicio 1

![[concu 3-1.png]]

a)
Funciona pero no respeta el orden de llegada. Cuando se ejecuta el signal se sigue el protocolo signal and continue, por lo que un nuevo auto puede llega, confirmar que cant no es mayor que 0 y hacer uso del puente, dejando al auto anterior esperando.

b)
```
Monitor Puente
{
	cond cola
	int esperando=0
	bool ocupado = false
	
	Procedure entrarPuente()
	{
		if (ocupado){esperando++; wait(cola)}
		else {ocupado = true}
	}
	
	Procedure salirPuente()
	{
		if(cant>0) {esperando--; signal(cola)}
		else {ocupado=false}
	}
}

Process Auto[id:1..N]
{
	Puente.entrarPuente()
	cruzando()
	Puente.salirPuente()
}
```

¿Sin monitor?
	Entiendo que no, porque un auto no podría comunicarle a otro auto que ya terminó de cruzar. Si o si se necesita un monitor para el envío de mensajes. (Con semáforos se puede pero la idea es no usarlos en esta práctica)
¿Menos Procedimientos?
	No se me ocurre, medio que necesitas los dos del monitor. Uno para que un auto avise que llegó y otro para que avise que terminó.
¿Sin variable condición?
	No porque la necesitas para mantener el orden de llegada.

c)
La primera solución no respeta el orden.
La que implemente en el punto b mantiene el orden de llegada al hacer uso de la variable condición.

## Ejercicio 2

Existen N procesos que deben leer información de una base de datos administrada por un
motor que admite un número limitado de consultas simultáneas.
a) Analice el problema y defina qué procesos, recursos y monitores/sincronizaciones
serán necesarios/convenientes para resolverlo.
b) Implemente el acceso a la base de datos por parte de los procesos, sabiendo que el
motor de la base de datos puede atender a lo sumo 5 consultas de lectura simultáneas.

```
Monitor DB
{

}

Process Proceso[id:1..5]
{
	
}
```