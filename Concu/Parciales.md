
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