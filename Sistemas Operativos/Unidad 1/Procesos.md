> Un **programa** es un **conjunto de instrucciones** en un lenguaje en particular. Cuando un programa es compilado, linkeado y cargado en memoria se transforma en un **proceso.**

- Programa en ejecución
- Instancia de programa corriendo en el computador

>[!Notemos]
>La labor principal del SO se basa en el control de procesos

### Programa a proceso
> Para que un programa pase a proceso para el SO, debe pasar por el pre procesador, compilador, ensamblador y enlazador.

```c
// Pre-procesador inteligente, sabe que debe incluir definiciones de stdio.h y la definición de word N cómo 10
#include <stdio.h>
#define N 10
void main(){
	int i = 0;
	for(i=0; i<N;i++){
	}
}
```

![[Pasted image 20241008135114.png]]

- **Pre-Procesador**:  Tiene por trabajo expandir los `#include` y reemplaza los macros de `#define`, entre otras operaciones. 
- **Compilador**: Convierte código fuente en C a ensamblador. Al momento de usar `gcc -s hello.c`, se genera el ensamblador del programa cómo archivo de texto plano.
- **Ensamblador**: Tiene por función traducir el programa en assembler a lenguaje de máquina. `hello.o` , archivo de objeto reubicable, generado por el ensamblador. 
- **Linkeador**: Se encarga de resolver las referencias pendientes, por ejemplo `hello.o` con `printf.o`. Produce finalmente un código ejecutable, listo para ser llevado a memoria y ejecutado y t**ransformado en proceso.**

>[!Notemos]
>Un programa ejecutable NO es un proceso

### Creación de procesos
> Un proceso puede ser creado en siguientes instancias:

1. Solicitud de usuario mediante comando `./hello.o`
2. Solicitud de un proceso ya existente, por medio de llamado a sistema

```c
#include <unistd.h>
int main(){
	pid = fork();
	return 0;
}
```

3. Mediante GUI.
4. SO puede crear varios procesos al momento de bootear.

**SO, se debe preocupar de crear y administrar los procesos.**
- Procurar y asegurar memoria principal para alojar el proceso y debe crear una estructura o registro que tenga una descripción del proceso.
***
### Bloque de control de proceso (Process Control Block)
> Nombre genérico dado a la estructura de datos que el Kernel maneja acerca del proceso. Contiene toda la información que describe al proceso en el sistema. Carnet de identidad del proceso.

- PID: Process ID.
- Estado: Que hace el proceso
- Prioridad
- Datos de contexto.

![[Pasted image 20241008141442.png]]

### Espacios de Direcciones de un Proceso en Memoria
>[!Observación]
>En memoria virtual, un proceso se organiza en segmentos.

- Texto: Código ejecutable
- Datos globales: Inicializados y no
- Heap: Memoria dinámica
- Stack de Usuario: Paso de parámetros en llamados a funciones y variables locales, el usuario puede manejarlo libremente.
- Stack de Kernel: Usado exclusivamente por el kernel y manejado por sistema operativo. Tanto cómo stack de usuario, cada proceso tiene los dos stacks.

> Al momento de booteo, el kernel se “instala” en las regiones más altas de la memoria virtual del proceso. Dicho de otra forma, **el kernel se mapea al espacio virtual de todos los procesos.**

![[Pasted image 20241008143848.png]]
### Contexto de un proceso
> Es toda la **información relacionada con el estado actual de ejecución del proceso**. Es el estado actual del procesador (entre otras cosas)

- Permite suspensión de procesos y reanudación
- Cada proceso tiene un contexto de ejecución.
- Incluye registros del procesador, Program Counter, Stack Pointer, EFLAGS, etc...
- Contexto de hardware, porción pequeña del contexto que contiene los registros que se deben cargar ántes de retomar la ejecución del proceso.

>[!Conclusión]
> Un **proceso**, se compone en tres componentes principales.
> Su **descriptor** (PCB), **espacio de direcciones y contexto.**

***
### Modelos de estado de un procesador
![[Pasted image 20241008144123.png]]
- Procesador es más rápido que I/O, es posible que procesos en memoria esperen a I/O.
- *"Swapear"* temporalmente procesos a disco para liberar memoria.
- Procesos swapeados a disco quedan bloqueados/suspendidos.
- Swapping es operación de I/O

>[!Observación]
> Utilización depende del manejador del sistema operativo, si es más eficiente en la planificación de procesos, entonces tendrá mejor utilización del procesador.

$$
U = \frac{q}{q+\tau}
$$

- $\tau$ : tiempo que necesita el SO para hacer sus labores.
- $q$ : tiempo de usuario proceso.

***
## Cambio de contexto
> Un cambio de contexto ocurre cada vez que el procesador comienza o reanuda la ejecución de un proceso distinto (al actual).

**¿Cuando ocurren?**
1. **Trap**: Interrupción asociada a error de ejecución de instrucción.
2. **Interrupción de evento** externo a la ejecución del proceso.
3. Interrupción del reloj, **timeout**.
4. **Fallo de memoria**: Dirección no está en DRAM.
5. **System Call**, I/O.}

>[!Observación]
>Un cambio de contexto o cambio de proceso no es lo mismo que un cambio del **modo de ejecución del procesador**

### Labores en un cambio de contexto
- Guardar el contexto del proceso incluyendo el PC y otros registros
- Actualizar el PCB del proceso que actualmente está en ejecución
- Mover el PCB de la cola de listo a la cola apropiada.
- Seleccionar otro proceso para ejecución
- Actualizar el PCB del proceso seleccionado
- Actualizar las estructuras de administración de memoria
- Restaurar el estado del procesador con el contexto del proceso elegido

>[!WARNING]
>Cambio de Contexto $\neq$ Cambio de Modo de Ejecución

### Ejecución del sistema operativo
**Ejecución usando el contexto de un proceso usuario (SO no es proceso)**:
- No tiene su propio contexto.
- Cada vez que se solicita servicio de SO, se llama a rutina de SO sin cambio de contexto
- Hay cambio de modo usuario a modo kernel
- Número pequeño de tareas: ejemplos, cambio de contexto puede ejecutarse fuera de contexto usuario
**Ejecución basada el procesos (SO se trata cómo proceso)**
- Implementa las tareas del SO cómo colección de procesos del sistema
- Cada proceso tiene su propio contexto
- Útil en sistemas multiprocesadores. 
### Cambio del modo de ejecución
1. **Modo usuario:** No privilegiado, no puede ejecutar instrucciones de kernel.
2. **Modo kernel:** Procesador puede hacer cualquier tipo de instrucción.

- Para SO basado en **contexto de proceso usuario** es necesario realizar cambio en el **modo de ejecución** cada vez que un proceso invoca llamado al sistema.
- **Polling**: Procesador sigue checkeando para **interrupciones** durante ciclo fetch.
	- Si hay una interrupción pendiente: PC = dirección inicial del manejador de interrupciones y cambia el modo del procesador de modo usuario a modo kernel, para que manejador pueda hacer instrucciones de kernel.

## Creación de procesos en Unix
> `fork()` es el llamado al sistema para crear un nuevo proceso

```c
#include <unistd.h>
pid_t fork(void);
```

- `fork();` crea un proceso hijo, copia exacta del proceso padre. Único distinto es el `pid.`
- Este retorna:
	- El `pid` del hijo, en el proceso padre.
	- 0 en el proceso hijo.
	- -1 si es que ocurrió un error.
- Se llama una vez, pero se ejecuta dos veces.
### Identificadores
> En sistemas Unix-like cada proceso tiene al menos seis identificadores asociados a el

### Familia `exec()`
- Si `fork()` genera un hijo, clon del proceso, ¿cómo sería posible crear procesos hijos distintos al padre?
- `execve()` **ejecuta** un **nuevo programa** en proceso que realiza la invocación.
- `execve()` no crea nuevo proceso, más bien **sobre-escribe el programa actual con uno nuevo.**

```c
#include <unistd.>
int execve(const char* filename, char* const argv[], char* const envp[]);
```

- `filename`: camino absoluto del programa ejecutable
- `argv[]`: arreglo de argumentos por linea de comando
- `envp[]`: medio ambiente de ejecución del proceso
- `excve()`: no retorna si hubo éxito.

***
