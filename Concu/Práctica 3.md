
![[concu 3-1.png]]

a)
Solo funciona si no hay nunca ningún auto esperando
Si hay gente en la cola, como cada vez que se despierta a un auto se vuelve a checkear la cantidad, como esa cantidad siempre va a ser mayor que 0 se vuelve a dormir y se traba el programa.

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
	Entiendo que no, porque un auto no podría comunicarle a otro auto que ya terminó de cruzar. Si o si se necesita un monitor para el envío de mensajes.
¿Menos Procedimientos?
	No se me ocurre, medio que necesitas los dos del monitor. Uno para que un auto avise que llegó y otro para que avise que terminó.
¿Sin variable condición?
	No porque la necesitas para mantener el orden de llegada.

c)
La primera solución directamente no funciona.
La que implemente en el punto b mantiene el orden de llegada al hacer uso de la variable condición.