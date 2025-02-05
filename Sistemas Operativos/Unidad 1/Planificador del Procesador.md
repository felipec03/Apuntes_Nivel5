## Planificación
> El planificador de un sistema operativo es el encargado de **decidir cuál proceso es asignado al procesador para ejecución.**

- **Módulo más importante del SO**
- Dependiendo del SO, planificador toma siguientes decisiones:
	- Cuál proceso entra al procesador
	- Cuanto tiempo puede ejecutarse
	- Si se desapropia el proceso ya planificado
	- Actualizar propiedades de planificación de procesos.
- *En SOs de tipo Unix y Linux, el planificador se ejecuta en el contexto del proceso que actualmente está en ejecución.*

### Tipos de Scheduling 
1. **Largo Plazo**: 
	1. Decide procesos admitidos al sistema
	2. Controla grado de multiprogramación, afecta utilización del procesador
	3. Mientras más procesos, cada uno recibe menor porcentaje de uso
2. **Media Plazo**:
	1. Decide si agregar o quitar programas que ya existan completa o parcialmente en memoria principal
	2. Parte de la función *swapping*
3. **Corto Plazo (despachador)**:
	1. Cual proceso entra al procesador
	2. Se ejecuta entre cada cambio de contexto, debe ser eficiente
	3. Eventos que puedan gatillar este plafinicador: Interrupción de reloj, error de ejecución, interrupciones I/O, system calls.
4. **Input / Output**:
	1. Decide cual requerimiento de I/O se hace primero

![[Pasted image 20241014145955.png]]
### Planificador es un optimizador
> **Planificar siempre involucra elegir un órden tal que se optimice** el uso de los recursos u optimice alguna medida de rendimiento o criterio.
- **Maximizar**: Throughput y utilización de procesador.
- **Minimizar**: Tiempo de espera, tiempos totales, tiempos de respuesta.

### Planificador de corto plazo
- Dos grandes criterios de optimización
- Para el **usuario**:
	- Turn Around Time o Tiempo de Reloj
	- Tiempo de espera y respuesta
- Para **sistema / procesado**r:
	- Utilización del procesador y throughput

> Un buen planificador debiera entonces **dar un buen servicio a diferentes tipos de procesos**, manteniendo buenos usos del sistema, sin favorecer demasiado unos procesos por sobre otros, y siendo capaz de responder a procesos con exigencias especiales. Todo esta debiera hacerlo en forma eficiente.

***

## Modo de Decisión
> Un planificador debiera ser capaz de interrumpir la ejecución de un proceso para planificar otro con más alta prioridad o con exigencias especiales

- **NO Apropiativo**: Puede sacar al actual proceso en ejecución
- **Apropiativo:** una vez que decide planificar un proceso, lo deja correr hasta que termina o hasta que éste libera voluntariamente el procesador
### Algoritmos Simples de Planificación
- **Prioridad (No apropiativo)**: Cada proceso tiene una prioridad asociada, prioridad 0 es la más alta; el de más alta, primero se ejecuta.
- **FCFS o FIFO (No apropiativo):** First come, first serve: Se ejecuta al proceso más antiguo.
- **Round-Robin (No apropiativo):** Por órden de llegada, pero sujeto a un quantum asociado a cada proceso.
- **SJF (apropiativo)**: Shortest Job First: El menor tiempo de proceso para ejecutarse va primero
- **SRT (no apropiativo)**: Shortest Remaining Time: El menor tiempo que le quede, versión desapropiativo, puede desapropiar proceso en función de algoritmo.
- **HRRN (no apropiativo) (Highest Response Ratio Next)**: se planifica aquel proceso cuya razón response ratio sea menor
> Los planificadores reales de los sistemas operativos son bastante complejos y combinan muchas de las técnicas de los algoritmos simples para asi poder dar un buen servicio a una gran variedad de tipos de procesos

### TAT 
**Ejemplo para FIFO:**
> El TAT, es el tiempo que demora un proceso en lo que entra a la cola de planificación y en lo que sale.
$$TAT(P) = P_{quantum-sale} - P_{quantum-entra}$$

Cómo tenemos un tiempo asociado, podemos calcular la utilización para distintos algoritmos y determinar cuales benefician más al usuario o al sistema operativo.

> Para el caso del round robin, la utilización es de un 90% mientras que la del FIFO es de un 97%, hay que tomar en consideración que cada vez que se haga un salto, existe un cambio de contexto. Quantum más pequeño -> Más overhead y más computacionalmente caro.

En RR; los procesos pequeños son más considerados a diferencia del FIFO, estos salen primeros. 

