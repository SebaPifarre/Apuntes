## Ejercicio 1
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

b) 1 7 8 2 9 10 3 4 5 6

c) 1 7 8 2 3 4 5 6 9 10 


## 2. Realice una solución concurrente de grano grueso (utilizando <> y/o ) para el siguiente problema. Dado un número N verifique cuántas veces aparece ese número en un arreglo de longitud M. Escriba las pre-condiciones que considere necesarias.

k -> cantidad de procesos
M -> longitud del arreglo

```
int Total := 0

Process Contar [id: 1..k]

```


