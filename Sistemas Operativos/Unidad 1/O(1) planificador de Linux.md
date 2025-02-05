## Los ticks (Hz)
> El system timer es el reloj que interrumpe al procesador (ciclo de ejecución) con una cierta frecuencia. Por ejemplo, en arquitecturas i386, `(include/asm-i386/param.h)`, se define:
```c
// Código del Kernel ...
#define HZ 1000 // internal kernel time frequency
```
Entonces, el **timer hace tick 1000 veces por segundo.** Para un procesador de $1[GHz]$
$$
\frac{10^9}{10^3} = 10^6 [ \frac{ciclo}{tick} ]
$$
## Los Jiffies
> El kernel mantiene una variable llamada **jiffies** que **contiene el número de ticks que han ocurrido desde que se booteo el sistema** `(<linux/jiffies.h>)`

```c
extern unsigned long volatile jiffies;
```
Cuando bootea, jiffie = 0 y cada tick se incrementa. Hay HZ ticks/segundo, entonces hay HZ jiffies/segundo. -> Los ticks y jiffies **permiten que el kernel tenga una noción de tiempo.**
## Nuevo scheduler
- Disponer un runqueue para cada procesador
- Asignar quantum más grandes a procesos más importantes
- Asingnar procesos a procesadores, afinidad. Se explota afinidad de la caché.
- Implementación de algoritmo en $O(1)$
## Estructura general
> Cada procesador tiene un active y un expired runqueue con 140 colas de prioridades cada una.
- Active: Tiene quantum
- Expired: No tiene quantum
*** 
- 1 - 99: Tarea tiempo real. Real time siempre se planifica ántes.
- 100 - 140: Tarea Normal de usuario. (Spotify)

![[Pasted image 20241017172150.png]]

- **Prioridad Estática**: para cada proceso se le asigna una prioridad.
- **Política de Planificación:** determina cómo ese proceso es planificado en su procesador

>[!INFO]
>Siempre se planifica primero la runqueue activa no vacía con mas alta Static Priority (número mas bajo).
## Políticas para tareas de Tiempo Real
> Linux soporta políticas de planificación para tareas tiempo real y para tareas normales. Un proceso con política de tiempo real tiene un SP entre 0 a 99. Y tiene **dos políticas** de planificación `SCHED_FIFO` y `SCHED_RR`.

## Políticas para tareas normales
> Tareas normales son t**odos aquellos procesos (hebras) que no requieren mecanismos especiales de real-time.** Generalmente son las tareas ejecutadas **por usuarios de un sistema**
- `SCHED_OTHER`:
- `SCHED_BATCH`:
- `SCHED_IDLE`:

**En `SCHED_OTHER` las tareas tienen una prioridad estática y una prioridad dinámica.**
- SP por defecto es 120.
- $-20 \leq \text{nice} \leq 19$
- DP se calcula según SP y el tiempo promedio que la tarea ha estado durmiendo.
$$
DP = \max{(100, \min{(SP - bonus + 5.139)})}
$$
- Se planifica **proceso que está en frente de la cola no vacía con mayor DP.**
- Proceso `SCHED_OTHER` puede ser desapropiado por tareas FIFO y RR.
## `sleep_avg` y bonus
- Cuando un **proceso se despierta** (se desbloquea), el tiempo que estuvo durmiendo **se suma a su tiempo total durmiendo.**
- Cuando el **proceso sale del procesador**, se **substrae** de `sleep_avg` el **tiempo que estuvo en el procesador.
- Bonus mide que porcentaje del tiempo la tarea ha estado bloqueada.
	- 5 = neutral
	- 10 = favorece prioridad en 5
	- 0 = desfavorece prioridad en 5

## ¿Cual proceso planificamos?

## Quantum de `SCHED_OTHER`
>[!TIP]
> Quantum no depende de la prioridad dinámica (DP)
![[Pasted image 20241017175427.png]]
- $100 \leq SP \leq 139$ es la **prioridad estática.**
- Menor valor de SP -> Mayor prioridad
- Tareas de mayor prioridad obtienen quantum más largo
## Procesos Interactivos
En Linux, se dice que un proceso es interactivo, si:
$$
\text{bonus} - 5 \geq SP/4 - 28
$$
- Proceso con prioridad 100 pasa a ser interactivo cuando tiempo promedio dormido es mayor que $200[ms]$
- Proceso con SP por defecto, $SP = 120$, se vuelve interactivo si su tiempo promedio dormido es mayor que $700[ms]$
- Procesos de baja prioridad (139), **nunca serán interactivos.**
## Interactividad de los procesos
> Existe una interrupción que permite al scheduler realizar tareas rutinarias y tomar decisiones respecto del proceso actualmente en el procesador En Linux, cada 1 ms se invoca el manejador de interrupcciones.

