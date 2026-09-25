
![[concu 3-1.png]]

a)
Solo funciona si no hay nunca ningún auto esperando
Si hay gente en la cola, como cada vez que se despierta a un auto se vuelve a checkear la cantidad, como esa cantidad siempre va a ser mayor que 0 se vuelve a dormir y se traba el programa.

b)
```
Monitor Puente
{
	cond cola
	int cant=0
	bool ocupado = false
	
	Procedure entrarPuente()
	{
		if (not ocupado){}
	}
}
```
