### Sección Crítica
>[!HINT]
>Sección Crítica: Trozo de código ejecutado por múltiples hebras.
## Criterio/Condición de Carrera
>[!HINT]
> Dos hebras pelean por un recurso de memoria compartida, por ejemplo...

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int a = 0;

// H1
void hebraUno(){
	if(a == 0){
		printf("Soy cero");
	}
	a++;
}

void hebraDos(){
	if(a != 0){
		printf("No soy cero");
	}
	a++;
}

int main(){
	pthread(hebraUno);
	pthread(hebraDos);
	return 0;
}
```

### Exclusión Mutua
> Requerimiento sobre una sección crítica, que dice que **sólo una hebra puede estar ejecutando dicha sección crítica.**
- EM **se define por los datos** que se acceden en ella, no el código.
- **Ejemplo**: En Stack concurrente, los métodos `pop()` y `push()` deben ser mutuamente excluyentes.
- Una sola hebra puede estar en una sección crítica a la vez, si están accediendo a datos compartidos son secciones crítica y hay que imponer exclusión mutua.
### Modelo de Concurrencia
Asumimos colección de tareas del tipo
```c
Process(i) {
	while (true) {
		// codigo no critico
		// ...
		enterCS();
		SC();
		exitSC();
		// ...
		// codigo no critico
	}
}
```
- El código es ejecutado concurrentemente por múltiples hebras sin necesidad de restricción.
- enterCS: acciones a realizar para entrar en sección crítica
- exitCS: acciones para salir de sección y habilita a otras hebras
- Código no crítico es cualquier código donde no se accede recursos compartidos
### Requerimientos para solución correcta de Exclusión Mútua
1. **Exclusión Mutua:** Cuando una hebra está en su sección crítica, ninguna otra hebra está en la sección.
2. Sin **Deadlock**: Todas las hebras quedan ejecutando `enterCS()`, bloqueadas o busy-waiting, por tanto ninguna entra.
3. Sin **inanición**: Si nunca logra entras a sección crítica.
4. **Progreso**: Si hebra ejecuta enterCS() y ninguna otra está en la sección crítica, se debe permitir a la hebra entrar a la sección crítica.
### Locks
> [!HINT]
> Función enter y exit pueden ser implementadas
- Hardware: Instrucciones nativas de procesador
- Software: Usar algoritmos e implementarlo en lenguaje de alto nivel.
- SO: Servicios
- Estructuras nativas.
>[!Atomicidad]
> Una operación es **atómica** si: No puede dividirse en partes, se ejecuta completamente o no se ejecuta, se ejecuta sin interrupción.
#### Ilusión de atomicidad
```c
inc(int &i) {             LOAD i, ACC // carga el acumulador
	i = i + 1;            INC ACC // incrementa el acumulador
}                         STORE ACC, i // almacena el resultado en memori
```
> Toda instrucción assembler es atómica, pero hasta la instrucción más simple de lenguaje de alto nivel puede no ser atómica.

### Solución por hardware 1: Deshabilitación de interrupciones
*Hebra se ejecuta hasta que invoca un llamado a SO o es interrumpida*, por ejemplo, el quantum. Para garantizar Exclusión Mutua, la hebra deshabilita las interrupciones del procesador justamente antes de entrar a Sección Crítica.

```c
while(true){
	deshabilitar_interrupt(); //  CLI()
	SC();
	habilitar_interrupts();   //  SLI()
}
```
- Sencillo y funciona para cualquier número de hebras.
- Habilidad de context switch queda limitada.
- No sirve para multiprocesador y no está libre de inanición.
### Solución por hardware 2 : `testset()`

>[!INFO]
>Uso de instrucciones especiales de máquina que realizan en **forma atómica más de una operación.**

```c
// Atómico para nosotros
boolean testset(int &i) {
	if (i == 0) {
		i = 1;
		return true;
	}
	else{
		return false
	}
}

T(i){
	while(true){
		// ...
		// Planificador hace que hebras siempre vean "puerta cerrada"
		while(!testSet(bolt)); // busy waiting, pregunta incesantemente
		SC();
		bolt = 0;
		// ...
	}
}

int bolt;
void main(){
	bolt = 0;
	parbegin(T(1), T(2), ... , T(n))
}

```

>  **Busy-waiting**: es lo mismo que **spin-lock**: Cuando una **tarea espera poder entrar a la SC** ejecutando instrucciones. **Monoprocesador** sufre de pérdida masiva de tiempo esperando spin-lock, en cambio multiprocesador introduce paralelismo.

>[!WARNING]
>Ocurre mala suerte entre el planificador del SO y la sección crítica de código.

### Solución por hardware 3: `exchange()`

>[!INFO]
>`exchange(a, b)` intercambia los valores contenidos en las direcciones de a y b de manera atómica.

```c
// Atómico para nosotros
void exchange(int a, int b) {
	int tmp;
	tmp = a;
	a = b;
	b = tmp;
}

// key_i = key de algun posible i
T(i){
	int keyi;
	while(true){
		keyi = 1;
		while(keyi != 0){
			exchange(keyi, bolt); // busy waiting
		}
		SC();
		exchange(keyi, bolt);
	}
}

// Compartido
int bolt;
void main(){
	bolt = 0;
	parbegin(T(1), T(2), ... , T(n));
}

```

- **Desventaja**: No se puede aprovechar el tiempo durante spin-lock en monoprocesador.

### Solución por Software 1
>[!INFO]
>Solución para dos hebras, ambas comparten la variable `turno`

```c
int turno;
T0 {
	while(true){
		while(turno !=0);
		SC();
		turno = 1;
	}
}

T1 {
	while(true){
		while(turno !=1);
		SC();
		turno = 0;
	}
}

void main(){
	turno = 0;
	parbegin(T0, T1)
}
```

- **Garantiza exclusión mutua:** el turno solamente puede ser 0 o 1; satisface sección crítica.
- Aplicable a solo dos hebras: 
- Alternación estricta de ejecución: secuencial total, no concurrente. (T0, T1, T0, T1, ...)
- No satisface progreso: **No aplica concurrencia.**
- **No produce deadlock**: turno no puede tener más de dos valores.
### Solución por Software 2

>[!INFO]
>Arreglo booleano `flag[]` compartido por hebras.

```c
boolean flag[2]
T0{
	while(true){
		while(flag[1]);
		flag[0] = true;
		SC();
		flag[0] = false;
	}
}

T1{
	while(true){
		while(flag[']);
		flag[1] = true;
		SC();
		flag[1] = false;
	}
}

void main(){
	flag[0] = flag[1] = FALSE;
	parbegin(T0, T1);
}
```

- No satisface el requerimiento de exclusión mutua, solución mala.

### Solución por Software 3
> Problemas más evidentes de soluciones previas es que una tarea puede cambiar su estado después que otra area haya consultado dicho estado, pero antes que la primera haya entrado a la sección crítica.
> Intentemos solucionar este problema cambiando el orden de las dos instrucciones involucradas

### Solución de Peterson
```c
boolean flag[2]
int turno; 

T0{
	while(true){
		// ...
		flag[0] = true;
		turno = 1;
		while (flag[1] && turno == 1)
		SC();
		flag[0] = false;
		// ...
	}
}

T1{
	while(true){
		// ...
		flag[1] = true;
		turno = 0;
		while (flag[0] && turno == 0)
		SC();
		flag[1] = false;
		// ...
	}
}

void main(){
	flag[0] = flag[1] = FALSE;
	parbegin(T0, T1);
}
```

- Satisface todos los 4 requerimientos previos: Exclusión Mutua, No produce Deadlock e inanición y genera progreso.
- Funciona sólo para dos hebras y usa busy-waiting.

>[!TIP]
>Solución caballerezca: Uno le da el paso al otro, pero al mismo tiempo en caso de que pase uno, el otro queda en spin-lock.

#### Demostración por Contradicción
**Claim: La solución no satisface la exclusión mutua, en base al código se determina**
**H0: **
```c
(flag[1] == T && turno == 1) = F
!(flag[1] == T && turno == 1) = T
!(flag[1] == T) || !(turno == 1) = T 
(flag[1] == F || turno == 0 = T) // En el programa, se setea que flag[1] == V
FALSO
```

**H1:**
```c
(flag[0] == T && turno == 0) = F
!(flag[0] == T && turno == 0) = T
!(flag[0] == T)|| !(turno == 0) = T
(flag[0] == F || turno == 1) = T // En el programa, se setea que flag[0] == V
FALSO...
```
$$
\text{Al llegar a una contradicción, la aseveración hecha en un comienzo es falsa} \iff \text{Solución si satisface} 
$$

