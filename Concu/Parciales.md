
![[parcial_concu_2026_05_06.jpeg]]

```
int cant_ubicados

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
	V(finalizao)
	P(esperando_resultado[id])
}

Process Comisario
{
	P(aviso_comisario)
	for i=1 to 21 
		V(largada)
	for i=1 to 22
		P(aviso_comisario)
		P(finalizo)
		pop(C, id)
		V(finalizo)
		Revisar(id)
		
}
```