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

pre-condición -> M es divisible por k

```
int Total := 0
vec Array[];

Process Contar [id: 0..k-1]
{
	int Parcial:=0;
	int cant := M DIV k;
	
	for i:= id*cant to (id+1)*cant-1 do
		if vec[i] = N then
			Parcial:=Parcial + 1
			
	<Total := Total + Parcial>;
	
}
```


## Ejercicio 3
![[ej3.png]]

a) 
```
int cant = 0; int pri_ocupada=0; int pri_vacia=0; int buffer[N];
Process Productor
{while(true)
	{produce elemento
	<await (cant < N); cant++;
	buffer[pri_vacia]=elemento;>
	pri_vacia=(pri_vacia + 1) mod N;
	}
}

Process Consumidor
{while(true)
	{<await (cant>0); cant--;
	elemento=buffer[pri_ocupada];>
	pri_ocupada=(pri_ocupada+1) mod N;
	consume elemento	
	}
}
```

b)
```
int cant = 0; int pri_ocupada=0; int pri_vacia=0; int buffer[N];
Process Productor
{while(true)
	{produce elemento
	<await (cant < N); cant++;
	buffer[pri_vacia]=elemento;
	pri_vacia=(pri_vacia + 1) mod N;>
	}
}

Process Consumidor
{while(true)
	{<await (cant>0); cant--;
	elemento=buffer[pri_ocupada];
	pri_ocupada=(pri_ocupada+1) mod N;>
	consume elemento	
	}
}
```

## Ejercicio 4

Resolver con SENTENCIAS AWAIT (<> y <await B; S>). Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola, cuando un proceso necesita usar una instancia del recurso la saca de la cola, la usa y cuando termina de usarla la vuelve a depositar.

```
cola recursos[5];
Process Proceso
{
	<await (not cola.isEmpty) desencolo>
	//utilizo recurso
	encolo
}
```

## Ejercicio 5

En cada ítem debe realizar una solución concurrente de grano grueso (utilizando <> y/o ) para el siguiente problema, teniendo en cuenta las condiciones indicadas en el item. Existen N personas que deben imprimir un trabajo cada una.
a) Implemente una solución suponiendo que existe una única impresora compartida por todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar el orden. Existe una función Imprimir(documento) llamada por la persona que simula el uso de la impresora. Sólo se deben usar los procesos que representan a las Personas.
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
c) Modifique la solución de (a) para el caso en que se deba respetar el orden dado por el identificador del proceso (cuando está libre la impresora, de los procesos que han solicitado su uso la debe usar el que tenga menor identificador).
d) Modifique la solución de (b) para el caso en que además hay un proceso Coordinador que le indica a cada persona que es su turno de usar la impresora.

a)
```
bool ocupada=false
Process Persona[1..N]
{
	<await (not ocupada); ocupada = true;>
	imprimir;
	ocupada = false;
}
```

b) 
```
bool siguiente=-1;
cola C; // Cola sin prioridad
Process Persona[1..N]
{
	< if(siguiente == -1) siguiente=id; else Agregar(C, id) >
	<await (siguiente==id);>
	imprimir;
	< if (C.isEmpty) siguiente = -1; else siguiente = Sacar(C); >
}
```

c)
```
bool siguiente=-1;
colaEspecial C; //cola con prioridad de id
Process Persona[1..N]
{
	< if(siguiente == -1) siguiente=id; else Agregar(C, id) >
	<await (siguiente==id);>
	imprimir;
	< if (C.isEmpty) siguiente = -1; else siguiente = Sacar(C); >
}
```

d)
```
cola C;
bool ocupada = false;
Process Coordinador
{
	while(true)
	{
		<await (not cola.isEmpty); siguiente = Sacar(C); ocupada=true;>
		<await (not ocupada)>
	}
}

Process Persona[1..N]
{
	Agregar(C,id);
	<await (siguiente==id);>
	//imprimiendo
	ocupada = false;
}

```
