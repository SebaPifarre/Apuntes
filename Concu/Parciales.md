
![[parcial_concu_2026_05_06.jpeg]]

```
int cant_ubicados = 0
sem llegada = 1, largada = 0, aviso_comisario = 0, finalizo = 0
sem esperando_resultado = ([22], 0)

Process Auto[id:1..22]
{
	P(llegada)
	Ubicarse()
	cant_ubicados++
	if(cant_ubicados<22){P(largada)}
	else{V(aviso_comisario)}
	for i=1 to 50
		Dar_vuelta()
	P(finalizo)
	push(C, id)
	V(aviso_comisario)
	V(finalizo)
	P(esperando_resultado[id])
}

Process Comisario
{
	int id, puesto, i

	P(aviso_comisario)
	for i=1 to 21 
		V(largada)
	for i=1 to 22
		P(aviso_comisario)
		P(finalizo)
		pop(C, id)
		V(finalizo)
		Revisar(id) //esto devuelve o el puesto o -1
		puestos[id]=puesto
		V(esperando_resultado[id])
		
}
```

```
Process Auto[id:1..50]
{
	int idC
	int categoria
	
	Cirtuito[idC].llegada(categoria, id)
	Usar()
	Circuito[idC].salida()
}

Monitor Circuito[1..3]
{
	cond F1, F2
	bool libre=true
	int esperando1, esperando2 = 0

	Procedure llegada(categoria: in int)
	{
		if(libre){libre=false}
		else
		{
			if(categoria==1)
			{
				esperando1++
				wait(F1)
			}
			else
			{
				esperando2++
				wait(F2)
			}
		}
	}
	
	Procedure salida(categoria: in int)
	{
		if(esperando1>0){signal(F1)}
		else if(esperando2>0){signal(F2)}
		else{libre=true}
	}
}
```

```

```