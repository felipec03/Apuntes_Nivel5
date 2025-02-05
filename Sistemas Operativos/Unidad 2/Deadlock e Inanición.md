## Definición de Deadlock
>[!INFO]
>Bloqueo permanente de un conjunto de procesos que compiten por un recurso del sistema o cooperan mutuamente.

- En un conjunto de procesos en deadlock, cada proceso está **esperando** que suceda un evento, el cual **sólo puede ser generado por otro proceso en deadlock.**
- No existe una solución eficiente.
### Ejemplo Recursos Reusables
- Disk: D
- Tape: T

![[Pasted image 20241118145206.png]]
### Recursos Consumibles
- Creados y destruidos dinamicamente por procesos
- Deadlock se genera por ambos dependiendo de su utilización
- Ejemplo de deadlock: Requerimiento de memoria.

### Grafos de procuramiento de recursos
>[!info]
 Grafo dirigido que representa el estado actual de un sistema de recursos y procesos.
![[Pasted image 20241118145350.png]]

- Proceso = Círculo
- Recurso = Cuadrado.

### Ejemplos notables
![[Pasted image 20241118145513.png]]

>[!WARNING]
>Círculo en un recurso indican el número de instancias de dicho recurso.

## Condiciones para que haya Deadlock
>[!TIP]
 Tres condiciones para que ocurra un desastre
 
- **Exclusión mutua** entre recursos -> Solo un recurso puede usar un recurso a la vez
- **"Hold and wait"** -> Mientras que a un proceso se le asigne un recurso, puede esperar a cualquier otro. 
	- **Ejemplo filósofos**: Si filósofo agarra uno de los tenedores, va a esperar el otro tenedor.
- **No desapropiación:** No existe un planificador o ente superior.
*Hasta acá, es posible que exista deadlock, en cambio:*
- **Condición necesaria, Espera Circular:** Cadena cerrada de procesos, cada proceso mantiene al menos un recurso que es solicitado por otro proceso en la cadena.

## Prevención de Deadlock
>[!INFO]
 Encontrar métodos que garanticen que el sistema no entrará en deadlock.
 
**Método Indirecto:** Prevenir o evitar que se cumplan una de las tres condiciones para que haya Deadlock.

**Método Directo:** Algoritmo que **previene** la creación de la cadena de Deadlock
1. Ordenar en una lista lineal todos los procesos del sistema, con un número único cualquiera.
2. Si necesita recursos, se otorga de manera ascendente.
EJ: Si tuviera $P1, P2, P3$, con ID $8, 2, 3$ respectivamente: $P2 \Rightarrow P3 \Rightarrow P1$
- Evitar: Ir viendo en el momento
- Prevenir: En anticipación se busca mitigar.
## Evitar Deadlock (Avoidance)
> Dinámicamente se determina si se le puede asignar un recurso a un proceso en base si es que puede o no producir deadlock.

- Eficiente, pero requiere conocer la secuencia de requerimientos de los procesos.

>[!WARNING]
 Permite las tres condiciones necesarias vistas anteriormente, pero niega la asignación de un recurso si dicha asignación podría llevar a producir un deadlock

**Existen dos técnicas:**
#### Denegación inicio de ejecución de un proceso
...
#### Denegación asignación de un recurso (Algoritmo del Banquero)
- Se define el estado del sistema cómo la asignación actual de recursos a procesos, osea:
	- Vectores $R$ y $V$
	- Matrices $C$ y $A$
- **Estado Seguro:** Existe al menos una secuencia de asignación que no resulta en deadlock.
- Estado Inseguro = $\neg$ EstadoSeguro

**Ejemplo:**
![[Pasted image 20241119142056.png]]
- El vector $V = (0,1,1)$, no tiene suficiente para $P1, P3 \ \text{y} \ P4$, entonces, vemos que solamente puede correr $P2$
- $P2$ al ser ejecutado, ahora su fila en la matriz de alocación en memoria sería de $(6,1,2) + (0,0,1).$
- Ahora el vector de disponibles es de $V=(6,1,3)$, por lo que no se genera deadlock.

>[!TIP]
>Se está simulando si es que se tiene la cantidad de recursos para otorgar al proceso.

>[!INFO]
>Si se da cuenta de que un estado es inseguro, cómo fue determinado previamente, se ven todos los procesos se encuentran bloqueados en la matriz $C-A$, entonces se **retrocede a un estado seguro.**

### Características de Deadlock Avoidance
- Costoso, requiere matrices.
- Los procesos deben comunicar con anticipación la cantidad máxima de recursos que vayan a pedir.
- Procesos independientes sin sincronización entre ellos.
### Problema aplicado a Filósofos
- Se puede resolver por medio de un método indirecto: Enumeración de procesos, donde a cada tenedor y filósofo se le asigna un número. 
- Proceso == Filósofo, Tenedor == Recurso.

https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/7_Deadlocks.html
