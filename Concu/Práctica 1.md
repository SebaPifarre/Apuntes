
![[ej1.png]]

P1
1) Comparación x igual 0
2) 
	Load Pos Memoria y, Reg acumulador
	Add Reg acumulador, 2
 		<mark style="background: #FF5582A6;">Store Reg acumulador, Pos memoria x</mark>

P2
3) Comparación x mayor 0
4) Load Pos Memoria x, Reg Acumulador
5) Add Reg Acumulador, 1
6) Store Reg Acumulador, Pos Memoria x

P3
7) Load Pos Memoria x, Reg Acumulador
8) Mult Reg Acumulador, 3
9) Load Pos Memoria x
10) Store Pos Memoria x

a) Ejecución "normal" 1 2 3 4 5 6 7 8 9 10

b) 
