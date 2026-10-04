
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
	V(e)
	V(hayComprador)
	P(espera[id])
	if(seGenero[id]){comprobante=comprobantes[id]}
	else{"No se pudo"}
}

Process Cajero[id:1..C]
{
	text comprobante
	int aux
	bool ok;
	bool seGenero[N]=([N], true)

	P(horario)
	while(true)
	{
		P(hayComprador)
		P(e)
		pop(C, aux, solicitud)
		V(e)
		P(f)
		if(entradas>0){ok=true}
		else{ok=false}
		V(f)
		if(ok){comprobantes[aux]=generar_comprobante()}
		else{seGenero[aux]=false}
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
Process Persona[id:1..N]
{
	int idP
	
	Puesto[idP].llegada(id)
	General.retirar(id)
}

Process Empleado[id:1..4]
{
	int idP
	while(seguir)
	{
		Puesto[idP].sig(seguir)
		if(not seguir){break}
		//procesar
		General.entregar()
	}
}

Monitor Puesto[id:1..4]
{
	cond hayPersona,espera
	cola C
	bool libre
	bool cortar=false
	
	Procedure llegada(id:in int)
	{
		libre=false
		push(C,id, tramite)
		signal(hayPersona)
		wait(espera)
	}
	
	Procedure sig(seguir:out bool;tramite:out text)
	{
		if(libre){wait(hayPersona)}
		if(cortar){seguir=false}
		else
		{
			pop(C,aux,tramite)
			libre=true
		}
	}
	Procedure cerrar()
	{
		cortar=true
		singal(hayPerona)
	}
	
}

Monitor General
{
	int total=0
	bool entregado[N]=([N], false)
	text resultados[N]
	cond espera[N]
	
	Procedure entregar(R:in text; id:in int)
	{
		cant++
		resultados[id]=R
		signal(espera[id])
		entregado[id]=true
		if(cant==N)
		{
			for i=1 to 4
				Puesto[i].cerrar()
		}
	}
	
	Procedure retirar(R:out text)
	{
		if(entregado[id]==false){wait(espera[id])}
		R=resultados[id]
	}
}
```

![[concu-par-7-10-24.png]]

Monitores

```
Process Persona[id:1..N]
{
	Lago.llegada()
	//cruzar
	Lago.salida()
}

Monitor Lago
{
	int esperando = 0
	cond espera
	bool libre=true
	
	Procedure llegada()
	{
		if(libre){libre=false}
		else{esperando++; wait(espera)}
	}
	
	Procedure salida()
	{
		if(esperando>0){esperando--;signal(espera)}
		else{libre=true}
	}
}
```

```
Process Persona[id:1..N]
{
	Lago.llegada(id)
	//cruzar
	Lago.salida()
}

Monitor Lago
{
	int esperando = 0
	cond espera[N]=([N],0)
	bool libre=true
	cola C
	
	Procedure llegada(id:in int)
	{
		if(libre){libre=false}
		else{esperando++; insertar(C,id); wait(espera[id])}
	}
	
	Procedure salida()
	{
		if(esperando>0){esperando--;signal(espera[id])}
		else{libre=true}
	}
}
```



![[concu-par-14-5-25.png]]

Semáforos 1

```
char caracteres[1000000]
int f=0,c=0,terminados=0

Process Worker[id:0..3]
{
	for i=id to 
}
```