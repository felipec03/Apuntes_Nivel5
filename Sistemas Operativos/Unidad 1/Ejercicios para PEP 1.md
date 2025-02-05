## Pregunta 1
> Un planificador utiliza la siguiente fórmula para estimar la siguiente ráfaga de procesador de un proceso. 
- $\tau_{n+1} = \alpha \cdot t_n + (1-\alpha) \tau_n$
- $t_n = \text{rafaga anterior}$
- $\tau_n = \text{estimación anterior}$
- $0 \leq \alpha \leq 1$
> ¿Cuales son las implicancias en la estimación de la ráfaga y a qué algoritmo se acerca?
1. $\tau_0 = 100[ms]$ y $\alpha = 0$
2. $\tau_0 = 10 [ms]$ y $\alpha = 0.99$

**Respuesta:** Ambos algoritmos están sujetos a quantum
1. $\tau_1 = 0+(1-0)100 = 100[ms] = \tau_2 = \tau_n$ , entonces el tiempo anterior de ráfaga no cambia. No se pueden sacar datos de tiempo, entonces tienda a Round Robin.
***
1. $\tau_{n+1} = 0.99 \times t_n + \text{algo chico}$
- Mientras mas pase el tiempo, se van haciendo **irrelevantes las estimaciones anteriores,** por lo que tiende a FIFO.
## Pregunta 2
```c
#include <unistd.h>
int main(){
	int pid1 = 1;
	int pid2 = 1;
	pid1 = fork();
	pid2 = fork();
	if(pid1 != 0 && pid2 != 0){
		printf("A\n");
	}
	if(pid1 != 0 || pid2 != 0){
		printf("B\n");
	}
	else{
		printf("C\n");
	}
	
	return 0;
}
```
- Padre: PID1 = 1; PID2 = 1
- Hijo1: PID1 = 0; PID2 = 1
- Hijo2: PID1 = 1; 
## Pregunta 3
> Considere un sistema computacional que no hace I/O, planificador Round Robin, con quantum de tiempo $q$ segundos y system timer con $HZ$ veces por segundo. El overhead total del system timer es de $r$ segundos y el tiempo de servicio promedio de los procesos es igual a $q$. Asumiendo que siempre están llegando nuevos procesos a la cola de listos y que el tiempo de planificación es despreciable, responda con justificación:

1. **¿Cual es el TAT normalizado de los procesos?** 
- TAT normalizado $= \frac{TAT}{TiempoServicio}$
- $TAT =\frac{q + (r \cdot HZ \cdot q)}{q}$

3.  **¿Cual sería el TAT normalizado cuando r tiende a 0 y que efecto tiene sobre los procesos?**
- $TAT =\frac{q + (r \cdot HZ \cdot q)}{q} \Rightarrow \frac{q}{q} = 1$
$$
HZ = [ \frac{tick}{s} ]
$$
$$
\frac{q}{1\over HZ} \Rightarrow q \cdot r \cdot HZ
$$
$$
\tau = \frac{1}{HZ}
$$
***
