## Requerimientos de un Sistema de Administración de Memoria
#### Reubicación
> El programador no sabe donde se ubica en memoria el programa en memoria principal una vez ejecutado.

>[!INFO]
>Durante su ejecución, el proceso puede ser swapeado a disco y vuelto a ser cargado en alguna ubicación distinta en memoria.

**Las referencias a memoria deben ser traducidas a direcciones de memoria física**
#### Protección
- Los procesos no deberían ser capaces de referenciar direcciones de memoria fuera de su espacio de direccionamiento. Exceptuando memoria compartida.
- Es imposible chequear direcciones físicas en tiempo de compilación, se hace en tiempo de ejecución.
- Hardware provee mecanismo de protección.
>a **segmentation fault** (often shortened to **segfault**) or **access violation** is a [fault](https://en.wikipedia.org/wiki/Fault_(computing) "Fault (computing)"), or failure condition, raised by hardware with [memory protection](https://en.wikipedia.org/wiki/Memory_protection "Memory protection"), notifying an [operating system](https://en.wikipedia.org/wiki/Operating_system "Operating system") (OS) the software has attempted to access a restricted area of memory (a memory access violation) https://en.wikipedia.org/wiki/Segmentation_fault
#### Compartición
- Se debe permitir que los procesos compartan áreas de memoria
- Procesos en sincronía comparten el área.
![[Pasted image 20241125140315.png]]
#### Organización Local
- Módulos: programas se organizan en forma de módulos, estos se pueden compilar independiente de otros. 
- Hay distintos grados de protección a distintos módulos (read-only, execute-only). Módulos se pueden compartir entre procesos.
- **Segmentación.**
#### Organización Física
- Se organiza en niveles, transparente al programador.
- Mover datos de un nivel a otro es trabajo del OS.
## Esquemas de particionamiento
>[!TIP]
>Particionamiento de Memoria Virtual.

Se usan dos técnicas:
- Paginación (mismo tamaño)
- Segmentación.
### Particionamiento Físico de la Memoria Física
- Particiones del mismo tamaño
- Si el tamaño es menor o igual que el tamaño de la partición y existe una partición libre, entonces el proceso puede ser cargado en memoria.
- Si están todas ocupadas -> OS puede swapear a disco.
- Overlay y particiones  de distinto tamaño.
- **Fragmentación Interna:** Es lo que no se usa dentro de una partición, no puede ser usado.
- **Fragmentación Externa:** Se encuentra afuera de cualquier bloque de partición, puede ser utilizado.
### Particionamiento dinámico
> Si existe suficiente memoria, se crea una partición del mismo tamaño que la del proceso que se quiere cargar. **Varía número y tamaño de las particiones.**
> ![[Pasted image 20241125143823.png]]

>[!WARNING]
>  Eventualmente la memoria se llena de espacios pequeños inútiles.
>  Fragmentación externa -> Se puede remediar con compactación.
>![[Pasted image 20241125143916.png]]
#### Algoritmo de posicionamiento Dinámico
- **Best-Fit:** Usa bloque de memoria más pequeño entre los suficientemente grandes para alojar proceso. Lento e ineficiente.
- **Worst-Fit:** Terrible malo.
- **First-Fit:** Busca primer bloque que sirva. El mejor en términos de rendimiento.
- **Next-Fit:** Desde la última posición hasta el primer bloque que sirva. Peor rendimiento que First.
### Buddy System
> "*Va diviendo y pregunta si cabe o no cabe*". Funciona en potencias de 2. Es rápido y eficiente en memoria comparado con First-Fit.

![[Pasted image 20241125145228.png]]
1. Toda la memoria se considera una sola partición de $2^u$
2. Si llega un proceso de tamaño $s$ , tal que $2^{u-1} < s \leq 2^u$, todo el bloque es asignado.
3. Si no, **bloque se divide en dos buddies.**
4. Si $2^{u-2} < s \leq 2^{u-1}$, **se asigna** a uno de los buddies. 
	1. Si no, el **proceso de división continúa**, hasta que se genere un bloque de tamaño suficiente.
5. Cuando se libera un bloque de $2^u$, y su buddy está libre, ambos se mezclan en un solo bloque de $2^{u-1}.$

> "A Buddy System is a memory management and allocation algorithm that divides memory into the power of two and tries to satisfy a memory request as suitable as possible. It uses memory in halves to try to give the best fit."

![[Pasted image 20241125144938.png|550]]

>[!TIP]
>Se considera fragmentación interna y externa al mismo tiempo, tiene ambas. Y además tiene aspectos posicionamiento dinámico y estático. **Es un mix de todo.**

## Tipos de Direcciones
#### Memoria Lógica
- Dirección independiente de la ubicación real en memoria física, referencia memoria virtual.
- Esta se tiene que traducir a dirección física, parte de debe estar en memoria física.
#### Memoria Física
- Dirección absoluta en memoria principal, en RAM.
- Único espacio, se almacenan memoria de procesos direccionados en memoria.
#### Memoria Relativa
- Expresada en términos relativos a otra dirección.
- Desplazamiento de a partir de una dirección, entonces, se requiere de una dirección referencia.

![[Pasted image 20241126140356.png|550]]

>[!INFO]
>Proceso de Mapeo, debe existir una traducción entre memoria lógica y física.

## Paginación
> La memoria es dividida en bloques de tamaño pequeño y fijo, llamados **marcos**. Se dividen los procesos y se en bloques de distinto tamaño, llamados **páginas**. Las páginas tiene un valor relativamente pequeño, no más de $4\text{[Kb]}$

**SO Mantiene tabla de las páginas por cada proceso:**
- Contiene el marco donde cada página fue asociada.
- Dirección de memoria consiste de un número de página y offset dentro de página.

>[!WARNING]
>Solo la última página contiene fragmentación interna.

#### Tabla de páginas de un proceso
-  Número de páginas virtuales,

![[Pasted image 20241126142229.png]]

> Sale proceso en 4, 5, 6 de Swap out B, entonces, no se encuentra marcada en la tabla de páginas.

![[Pasted image 20241126142246.png]]

## Direccionamiento con paginación
> ¿Cuál es el método para traducir una dirección?

Cuando tamaño de la página es potencia de 2, se divide en 2 campos:
- Número de página
- Offset de la página

![[Pasted image 20241126142605.png|650]]

> Número de página es 1, entonces se consulta en tabla de página, que en posición 1 indica que se encuentra en 6, de ahí se obtiene la dirección en memoria física.
## Segmentación
> Particiona la memoria en segmentos lógicos desde el punto de vista del programador: Programa Principal, Datos y Funciones.
> Segmentos se enumeran, dependiendo del proceso. Se debe registrar en la tabla de segmentos, donde 

- Segmentos son de largo variable, no sufren de fragmentación interna, pero si de externa.
- Existe máximo tamaño de segmento. Se debe almacenar el largo actual para poder diferenciar segmentos.
- Direcccionamiento = Número de Segmento + Offset dentro de Segmento.

![[Pasted image 20241126144207.png]]

>[!WARNING]
>Si offset o suma se pasa del largo total, se considera segmentation fault.

## Carga y Enlazamiento de Programas
> La primera etapa en la creación de un proceso es cargar el programa a memoria principal, el programa puede tener uno o más de un módulo, los objetos (`program.o`), cuales son linkeados en **etapa linker.** **Loader** carga el módulo linkeado en memoria.

![[Pasted image 20241202135239.png]]

>[!INFO]
>Al importar un módulo el linker une el objeto asociado a la librería para la función pedida y lo pone en el contexto del espacio de direcciones del programa principal que reside en memoria principal.

![[Pasted image 20241202135602.png]]

### Carga
#### Carga Absoluta
- Módulo de carga siempre cargado en la misma dirección de memoria
- Todas las referencias a memorias, son a direcciones físicas
- Donde se carga se hace en tiempo de compilación
- Si se modifica y recompila el módulo, las direcciones cambian.
#### Carga Reubicable
- En vez de referencias absolutas, compilador genera **referencias relativas**, se hace en tiempo de carga
- Típicamente, direcciones son relativos a inicio del módulo
- Si el cargador decide cargar el módulo en la dirección $X$, entonces simplemente se suma $X$ a cada referencia en memoria.
- Si es interrumpido, vuelve a donde estaba.
#### Carga Dinámica
- Se toma decisión en momento de carga, las referencias a memoria pasan a ser virtuales, mientras que en las otras son a memoria física.
- El cargador NO traduce las referencias relativas al momento de cargar el módulo.
- La referencias son traducidas a direcciones físicas en tiempo de ejecución
### Linking
> La función de un linker es **tomar una colección de módulos objetos y producir un módulo darga**. Cada módulo puede contener referencias simbólicas a funciones de otros módulos. El linker resuelve estas referencias en módulo unificado con direcciones relativas.

- Linking Estático: `gcc -o lab lab1.c -lmath`, siempre y cuando esté en `/usr/lib/libmath.a`
- Linking Dinámico: `gcc -o lab lab1.c -lmath`, siempre y cuando esté en `/usr/lib/libmath.so`, `.so` es "shared object".

>[!TIP]
>Por temas de versionamiento, el linkeado estático no resulta conveniente por que puede estar cargando versiones antiguas de un módulo.

