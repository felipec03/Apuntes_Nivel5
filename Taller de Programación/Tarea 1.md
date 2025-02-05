>[!Fecha Entrega]
>21 Octubre

## Algoritmo A*
### Descripción del Algoritmo
Algoritmo de pathfinding. En un laberinto con paredes, un inicio y un final encuentra un camino más corto entre el inicio y el final. Generalización de Dijkstra.
### Solución Naive
Suponiendo que se recorre la matriz por anchura, entonces se estaría barriendo diagonalmente la matriz, esto es sumamente ineficiente por que estaría pasando por varios caminos irrelevantes.
### Familia de algoritmos de A* Search
- **Ejemplos:**
	1. Cubo Rubik
	2. Puzzle 8
	3. Laberintos
- Se modela el problema como un sistema representado por un estado.
- **¿Cual es la sucesión de operaciones para llegar a un determinado estado final?**
- Existe un costo ppor operación, buscamos menor costo posible.
- Se puede subdividir en etapas por objetivos.
### Representación del Problema
**Problema:**
- 2 operaciones
- Cada estado es distinto
- Árbol
- Ejemplo con dos soluciones
Se genera un árbol de estados. Cada nodo debe ser único.

### Ejemplo: Bidones de Agua
- 2 bidones A y B; de 5 y 3 litros respectivamente.
- 6 operaciones disponibles
	-  llenarA
	-  llenarB
	-  vaciarA
	-  vaciarB
	-  trasvasijar A->B
	-  trasvasijar B->A
- *Objetivo: Disponer A con 4 litros.*
### Pseudocódigo
*Leer PPT Classroom*
### Heap (Priority Queue)
*Leer PPT*
***
Necesitamos una clase estado que almacene:
- $a_0$: que tiene el actual de la primera vasija
- $a_1$: que tiene el actual de la segunda vasija
- `parent`: que es un puntero a los estados anteriores
- `operacion`: que contiene cómo string la operación realizada

Además, necesitamos dos estructuras para almacenar dos
**Mínimo comun múltiplo == 1 => Existe combinación para todo bidón**

***
### Generalizar para N
```cpp
State{
	int *a;
	int n;
	int prioridad;
}
```

### Class Operation
```cpp
void A*(){
	while(!open.isEmpty()){
		e = open.pop();
		e1 = operacionLLenar(e)
		if(e1 != nullptr && open.find(e1) == false){
			open.push(e1);
		}
	}

}
```

Operation ->operaciónLlenar(i), operaciónVaciar(i), operacionTrasvasijar().

Crear clase operación para generalizar para N bidones.
```cpp
Operation *o;
for(i to n){
	e1 = o->operate(i);
	if e1 != // ASD
}
```
### Heurística
> Implementar alguna heurística, nos interesa obtener el camino que esté mas cerca con alguna aproximación buena.

**Necesitamos una cola de prioridad, para poder sacar ordenar mejor la cosa.**

### Optimizar CloseSet
Es posible hacer un hash table que sea all.
