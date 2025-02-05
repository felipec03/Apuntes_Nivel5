## Estructuras de Hardware y Control
> Direcciones a memoria son direcciones lógicas y son mapeadas a dirección física en tiempo de ejecución; se utiliza la propiedad de reubicación. Proceso puede ser particionado en páginas o segmentos, cuales no necesariamente son contiguos en memoria.

>[!INFO]
>No todas las páginas o segmentos de un proceso tienen que residir en memoria principal durante su ejecución.

>[!WARNING]
>La memoria virtual no es memoria secundaria. El espacio de un proceso no depende de memoria física, por algo reside en un mundo virtual. Memoria virtual y física son independientes.
## Ejecución de un Programa
> Sistema Operativo trae a memoria principal trae trozos del programa. Este conjunto de páginas o segmentos, se conoce cómo **conjunto residente**. 

>Si el proceso intenta acceder a alguna dirección que no se encuentre en el conjunto, se denomina **falla de paginación, o segfault** y SO manda interrupción.

>[!INFO]
>Es un error recuperable, el SO bloquea proceso y emite requerimiento para recoger trozos que necesite.

- SO le pide I/O a disco
- Otro proceso en cola de listos pasa a ejecutarse
- Cuando I/O esté listo, proceso fallido pasa a cola de listos.
### Ventajas del Modelo
- Mas procesos residen en memoria:
	- Solo se necesita una porción de cada uno
	- Con tantos procesos en memoria principal es muy probable que siempre exista un proceso en la cola de listos.
- Proceso puede ser más grande que toda la memoria principal disponible: Ilusión
- **Memoria Real**: Memoria principal
- **Memoria Virtual**: Visión lógica de la memoria, se requiere espacio en disco y permite multiprogramación y libera de restricciones de memoria real.

>[!TIP]
>Lo que puede pesar un proceso no está dictado por memoria física, si no mas bien por memoria virtual.
## Principio de Localidad
:V
## Memoria Virtual con Paginación
> Cada proceso tiene su tabla de páginas, cada entrada en la tabla tiene el número de marco en memoria principal donde la página en cuestión es cargada. 
- **Bit de Presencia** `(p)` : Indica si está en memoria o no.
- **Bit de Modificación** (`m`): Indica si página a sido modificada o no, desde la última vez que fué traída a memoria.

>[!INFO]
>Si bit m, no ha sido seteado, entonces no es necesario escribir la página a disco (swap out).
>![[Pasted image 20241202144828.png]]

> Recordar que esto es netamente a nivel de hardware, SO no hace nada acá.

![[Pasted image 20241202145101.png]]
 
 >[!TIP]
 >Cada proceso tiene su tabla de páginas, entonces dependiendo de las especificaciones por tabla, se tendrían N tablas para N procesos de un determinado tamaño. Podría utilizar una cantidad enorme de memoria.
## Esquema de Tabla de Dos Niveles
**Para un sistema de 32 bits con páginas de 8KB**
- $8KB = 2^3 * 2^{10} = 13 [bits]$ de offset.
- $32-13 = 19[bits]$ de número de página
Tabla de página:
- $AltoTabla = (2^{19})$, va desde el 0 al  $AltoTabla = (2^{19})$,.
- $LargoTabla = 4[Bytes] = 2^2 = 2[bits]$ va desde el 0.
Tamaño de la tabla:
$2^{19} \cdot 2^2 = 2 \cdot 2^20  = 2[MB]$

**¿Cuántos marcos está usando la tabla?** 
$$
\frac{\text{TamañoTabla}}{\text{TamañoPágina}}=\frac{2^{21}}{2^{13}} = 256 \text{[Marcos]}
$$
¿Cuantas entradas de Tabla de Página caben en un marco de página?
$$
\frac{TamañoPágina[B]}{TamañoAnchoPágina[B]}=\frac{2^{13}}{2^2} = 2^{11} \text{[Entradas]}
$$

**Ejemplo: Sistema de 32 bits, páginas de 4KB, largo de cada entrada de página 4 bytes.**

![[Pasted image 20241203140521.png]]

>[!TIP]
>Se necesita una tabla para ver las direcciones dentro de la tabla.

## Tabla Invertida de Páginas
> En el esquema de tabla invertida de páginas el tamaño es proporcional al tamaño del direccionamiento físico, y es independiente del número de procesos o páginas virtuales.

**Entradas en tabla invertida:**
- Número de página
- PID
- Control bits
- Chain: Puntero a cadena de páginas que mapean a la mismas posición.

>[!TIP]
> Cada entrada corresponde a una "representación" de marco, las colisiones se tratan por linear probing, osease, por la chain se va consultando por cada número de página hasta llegar a algún marco libre, función determinista.

![[Pasted image 20241203143447.png]]

> A diferencia del sistema anterior, es una sola tabla para todos los procesos.

#### Tamaño
Asumiendo que memoria física es de $4[GB]$ con páginas de $8[KB]$
- Largo: $\text{NumeroMarcos} = \frac{MemoriaFisica}{TamañoPagia} = \frac{2^{32}}{2^{13}} = 2^{19} \text{[Marcos]}$
- Ancho: Depende, puede ser $16 [B]$
Así, el tamaño es de $2^{19} \cdot 2^4 = 8[MB]$

#### Desventajas
- **Colisiones**: Compromete el tiempo de acceso, cada colisión es un acceso a memoria principal. Muy lento.
## Translation Lookaside Buffer (TLB)
> Se usa una memoria cache de alta velocidad que almacena una **parte pequeña de la tabla de páginas**. En el contexto de administración de memoria, esta memoria se conoce por **TLB**. El TLB almacena el mapeo página -> Marco de las páginas más recientemente usada.

- Dada una dirección virtual, el procesador examina TLB, que es una pieza de hardware de **carácter asociativo.**
- **Hit**: Se recupera número de marco.
- **Miss**: Se busca en tabla de páginas.
	- Luego, se revisa si la página está en memoria principal, si no, page fault.
	- Se actualiza TLB.
![[Pasted image 20241203145320.png]]

#### Tiempo Promedio
$$
\bar{T} = hit_{TLB} \cdot t_{TLB} + (1-hit_{TLB}) \bigl[ t_{tlb} + hit_{TP} \cdot t_{MEM} + (1-h_{TP}) \bigr] t_{MEM} + t_{SEC}
$$
- Tiempo promedio de hit, más...
- Probabilidad de miss por tiempo de miss (penalty rate), ponderado...
- Tiempo de memoria, más...
- Tiempo de memoria secundaria.

### Funcionamiento de TLB
![[Pasted image 20241216135910.png]]
$$
TLB \neq Caché
$$
### Uso de la Caché
![[Pasted image 20241216141206.png]]
### Tamaño de páginas
- Página pequeña -> Menor fragmentación interna
	- Se requieren más páginas por proceso, tablas más grandes.
- Tablas más grandes -> Buena porción de la tabla en MV
> Memoria secundaria está diseñada para transferir eficientemente grandes bloques de datos, luego por todo lo anterior podríamos concluir que es mejor tener páginas más grandes

### Comportamiento típico de Paginación en un Programa
![[Pasted image 20241216141455.png]]
- $P=$ Tamaño de todo el proceso
- $W=$ Tamaño del conjunto de trabajo
- $N=$ Número de páginas en el proceso.

>[!INFO]
>Trashing: SO está ocupado unicamente con swap-in y swap-out, entonces procesos se ven afectados. Punto más alto en primer gráfico.
## Segmentación
> Particiona la memoria en segmentos de tamaño variable. Se hace en función de los programas
- Simplifica el manejo de estructuras dinámicas
- Permite que los programas sean modificados y recompilados con facilidad, agregando independencia.
- Facilita compartición de datos y protección.
### Tabla de Segmentos
> Contiene la dirección en memoria real donde se encuentra el segmento. Cada entrada tiene el largo del segmento.

![[Pasted image 20241216143007.png]]

![[Pasted image 20241216142929.png]]

## Segmentación Paginada
>[!TIP]
>Así funcionan los SO actuales. La memoria principal se divide en marcos, igual que paginación, además los segmentos del proceso igualmente se encuentran paginados.

![[Pasted image 20241216143219.png]]
#### Ejercicio
> Sistema de 32 bits con páginas de 8KB
- 8KB = 2^3 x 2^10 = 13 bits
- 32 - 13 = 19 bits.
Notemos que el tamaño máximo de un segmento es de 512MB -> 2^32 
- Numero de páginas = 512 x 2^20 / 2^13 = 16 bits, osease 2^16 páginas.

![[Pasted image 20241216144637.png]]

### Política de Fetch
> Cómo se determina cuándo una página es cargada en memoria principal.
- Por Demanda: Cuando se hace referencia a memoria dentro de la página
	- Al comienzo, mucho page fault.
- Prepaginación: Junto con la página que no está, se traen otras
	- Se explota localidad espacial.
### Política de Ubicación
- Donde ubicar en memoria real
### Política de Reemplazo
> Cuando no hay marcos libres (memoria llena), se determina que página es reemplazada. Se debería sacar la página que minimiza los fallos de página, osease menos probable de ser referenciada en un futuro.
- Frame Locking (bit de cerradura):
	- Cuando está cerrado, no se puede reemplazar.
	- SO, buffer de dispositivo, I/O.
### Algoritmos básicos de reemplazo
- **Algoritmo Óptimo:** Selecciona la página cual tiempo hasta la próxima referencia sea máximo. 
	- Sabe el futuro, debe saber cuanto se demora la página en ser referenciada, no se puede implementar en la práctica.
- **Least Recently Used:** Selecciona la página que no ha sido referenciada durante más tiempo.
	- Por principio de localidad, es menos probable que página seleccionada por LRU que sea referenciada prontamente.
	- Es costoso puesto que se tiene que almacenar el tiempo, en cambio se puede implementar con un stack de referencia de páginas para "simular" tiempos.
- **FIFO:** Se reemplaza la página que lleva más tiempo en memoria.
	- Simple, pero mal rendimiento.
- **Política de Reloj:** Aproximación de LRU eficientemente, se utiliza un **bit de uso** para cada **marco**. 
	- Cuando se carga marco con página, bit = 1; Cuando página es referenciada, bit = 1.
	- Cuando se necesite reemplazar, se busca secuencialmente hasta el primero 0, cuando se encuentre, se reemplaza. 
		- **En esa misma secuencia**, si encuentra un 1, se cambia por un 0.
	- Le da una segunda oportunidad a la página.

>[!TIP]
>Mientras la memoria no esté llena, ninguno de estos algoritmos se aplica. Además, queremos evitar trashing.

![[Pasted image 20241217140047.png]]

#### Anomalía de Belady
>En FIFO, si se aumenta memoria, aumentan los fallos de página. Es impredecible, por eso se considera anomalía.
#### Algoritmos de Reemplazo
> En grandes cantidades de memoria da lo mismo el algoritmo, en cambio, se puede apreciar la siguiente jerarquía.

![[Pasted image 20241217141734.png]]
### Buffering de Página
> La página reemplazada es agregada a una de dos listas dependiendo si fue modificada o no. (dirty bit)
- No fue modificada -> Lista de páginas libres (cola)
- Fue modificada -> Lista de páginas modificadas (cola); deben llevarse a disco en algún instante.

>[!TIP]
>Se busca hacer I/O grande, I/O chico es muy costos, mas vale hacer uno grande de una. Entonces, la idea es hacer un buffer que vaya guardando a disco y una vez listo, este es liberado y llevado a la cola de páginas libres.

### Tamaño del Conjunto Residente
- **Tamaño Fijo:**  Número máximo por proceso, si hay page fault se encuentra y se reemplaza.
- **Tamaño Variable:** Número de marcos asignados puede cambiar, si el proceso tiene muchos fallos de página -> SO puede hacer crecer conjunto residente (más páginas), y viceversa puede quitar marcos.

![[Pasted image 20241217143051.png|600]]
### Política de Limpieza
>Indica cuándo una página que ha sido modificada debe escribirse a memoria secundaria
- **Limpieza por demanda:** 
- **Pre-Limpieza:**
### Control de Carga y Trashing
- Número de procesos que residen en memoria principal, nivel de multiprogramación.
- **Si son pocos:** Es probable que en algún momento todos estén bloqueados por I/O y procesos es inutilizado.
- **Si son muchos:**  El tamaño del conjunto residente será muy pequeño, causando alta taza de page fault -> Trashing.

![[Pasted image 20241217143427.png|500]]
### Suspensión de Procesos
> Determina cual de los procesos residentes son suspendidos, ninguno es estrictamente mejor que otro, cada uno tiene pros y cons.

- **Procesos con prioridad más baja:** 
- **Proceso que produce el fallo:**
- **Último proceso activado:**
- **Proceso con el conjunto residente más pequeño:**
- **Proceso más grande:**

