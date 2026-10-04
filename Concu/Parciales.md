
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

![[parcial_semaforos.png]]

```
sem hayComprador = 0
sem e = 1, f = 1
cola c(int,text)
sem espera[N] = ([N], 0)
text comprobantes[N]
sem horario = 0

Process timer
{
	delay()
	for 1 to C
		V(horario)
}

Process Comprador[id:1..N]
{
	text solicitud, comprobante

	P(e)
	push(C, id, solicitud)
	V(hayComprador)
	V(e)
	P(espera[id])
}

Process Cajero[id:1..C]
{
	text comprobante
	int aux

	P(horario)
	while(true)
	{
		P(hayComprador)
		P(e)
		pop(C, aux, solicitud)
		V(e)
		P(f)
		if(entradas>0){entradas--; comprobante=realizarCompra()}
		else{comprobante = "no hay entradas"}
		V(f)
		comprobantes[aux]=comprobante
		V(espera[aux])
	}
}
```

![[concu 9-12-34.png]]

Semáforos

```


Process Asistente[id:1..A]
{
	P(mutex)
	if(not libre){push(C, id); P(espera[id])}
	else{libre = false; V(mutex)}
	if(lentes==0){V(necesitaRecarga); P(finalizoRecarga)}
	lentes--
	P(mutex)
	if(not empty(C)){pop(C,id); V(espera[id])}
	else{libre=true}
	V(mutex)
}


Process Repositor
{
	while(true)
		P(necesitaRecarga)
		for 1 to num
			lentes++
		V(finalizoRecarga)
}
```

Monitores

```
Process Persona[1..N]
{
	
}
```