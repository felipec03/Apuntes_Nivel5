>[!Recordemos]
>Sobre el hardware específico, se crea un sistema operativo (kernel) que reside sobre una arquitectura específica.
### Registros del Procesador
#### Registros de Control
> Usuario programador no puede modificar esto por el SO.
- Program Counter, Instruction Register, Memory Buffer Register.
- PSW: Program Status Word
#### Registros de propósito general
> Util para el programador, pueden ser modificados.
- Permite optimizar uso de registros.
- Disponible para todos los programas y pueden ser referenciados por lenguaje de máquina.operativo
- Stack Pointer, `$s1, $t1`
#### Registros del sistema
> Registros usados por los Kernel del SO
- Dirección a tabla de interrupciones: Interrupciones ocurren por hechos asincrónicos.
- Dirección a tabla global de descriptores: Descriptor es el nombre del archivo, quien tiene acceso a archivo, soquete, etc... Descriptores están en alguna operación input/output.
- Dirección a tabla local de descriptores del proceso corriendo: Aquel que está en el procesador.

```c
#include <unistd.h>
void main()
{
	write(1, "Cuarentena cruel\n", 17);
}
```

- `write()` es un output de bytes por un descriptor. Pertenece al kernel del sistema operativo.
- Se pasa a modo kernel en ensamblador, para después devolverse a modo usuario.
- `printf()` al fin y al cabo pide servicios al kernel, por lo que el ensamblador salta a la librería.
### Ciclo de Instrucción (Ciclo Fetch)
> Las etapas depende de la arquitectura, pero en su etapa más básica el procesador debe ser capaz de ejecutar 4 ciclos. Este a menos que se le diga lo contrario, se hace infinitamente hasta completar instrucciones.

1. Fetch: Se busca Instrucción Pointer
2. Decode: Decodificación de instrucción
3. Execute: Ejecución de instrucción
4. Write-Back: 
### Interrupciones y excepciones
> Permite interrumpir el ciclo fetch. 
- Una excepción es un evento que requiera alterar la secuencia normal de ejecución y fuerza al procesador en ejecutar en modo privilegiado.
- La señal puede ser por I/O o por el mismo programa a ejecutar.

>[!Observación]
>El Hardware tiene precedencia por sobre el software, se encarga del manejo de señales.

![[Pasted image 20240924143223.png]]
- Interrupciones por dispositivo son manejadas por un **Programmable Interrupt Controller (PIC)**
- Se chequean y por medio de los Interrupt Request se decide dentro del ciclo de ejecución cuando realizar una interrupción.
##### Excepciones y Flujo de Control
> Toda excepción implica un cambio en el flujo de control, por ejemplo a una rutina del SO que se encarga de atender la excepción. Una vez atendido, se puede volver a la ejecución de procesos.
#### Interrupciones en x86
- Las interrupciones se les da valores de 0 a 255
	- 0-31: Interrupciones de procesador, viene del mismo hardware.
	- 32-255: Configuradas por SO. Sistema Operativo asigna interrupciones, no las genera.
- Trap: Interrupción de hardware por orden del proceso que se está ejecutando.

| IRQ     | Descripcion         | Tipo                |
| ------- | ------------------- | ------------------- |
| 0       | Error de división   | Fault               |
| 13      | Error de protección | Fault               |
| 14      | Fallo de página     | Fault               |
| 32-127  | Definido por SO     | Interrupción o Trap |
| 128     | Llamado a sistema   | Trap                |
| 129-255 | Definidas por SO    | Interrupción o Trap |
### Manejo de excepciones/interrupciones
> Cuando procesador detecta excepción, llama a un procedimiento del manejador de excepciones para atender la excepción. Este reside en el Kernel, es código.

1. Retorna el control a instrucción actual
2. Retorna el control a la próxima instrucción
3. Aborta el programa interrumpido
 
>[!Observación]
>Es importante recalcar que la detección de la excepción y la transferencia de control al manejador de interrupciones es realizada por el hardware, pero el manejo de la interrupción es realizada por software

El sistema operativo no tiene control total del sistema, también depende de que el hardware lo avise. Si el hardware le da el paso, sistema operativo puede tener control.
***
## Manejo de Interrupciones
Cada tipo de interrupción es asignado un entero único entre 0 y 255, también denominado IRQ. Cuando el sistema bootea, el SO instala la tabla de interrupciones, donde la entrada
k apunta al manejador específico a la interrupción k
>[!Observación]
>Cuando ocurre la interrupción, el sistema de control de interrupciones entrega el número de la interrupción. Luego, el hardware realiza un jump indirecto a la dirección de memoria del manejador correspondiente.
![[Pasted image 20241001140117.png]]

![[Pasted image 20241001140219.png]]

- Cada driver debe registrar su manejador de interrupciones.
- IRQ es único y diferente para cada dispositivo.
- request_irq y free_irq
**Tipos de excepciones**
1. Síncronas
2. Asíncronas
3. Intel

>[!Notemos]
>La única forma que el SO retome el control del sistema es mediante una excepción.
>El Hardware provee el mecanismo básico para interrumpir el proceso en ejecución.

![[Pasted image 20241001140555.png]]

### Almacenamiento y recuperación de registros
> Antes de saltar a la dirección del manejador de interrupciones, el procesador debe almacenar algunos registros del procesador. Estos se almacenan en el Stack Modo Kernel del proceso

### Clasificaciones de excepciones
- Interrupción: Señal proveniente de I/O.
- Trap: Excepción intencional.
- Fault: Potencialmente recuperable, puede retornar instrucción actual.
- Abort: Error no recuperable.
### Procesamiento de interrupciones múltiples
> Interrupciones son jerárquicas, por ejemplo, interrupciones de manejo de poder son de alta prioridad.
- Interrupción no mascareable: no puede ser ignorada, en cambio las mascareable, si pueden.
![[Pasted image 20241001141813.png]]
### Interrupciones y Multiprogramación
>[!Observación]
>Las interrupciones permiten que el procesador pueda ejecutar intercaladamente varios procesos, aumentando así la utilización del procesador. 

1. Inicialización: I/O se comunica con driver del dispositivo y prepara operación. Se hace por software.
2. Transferencia de Información: Datos fluyen del dispositivo a los buffers del SO. Se hace por hardware. Interrupciones permiten que el procesador atienda otros procesos mientras ocurre esta etapa.
3. Finalización: Moduo I/O finaliza la operación, libera memoria y termina comunicación.  con driver. Se hace por software.
> Polling is **a method where a device continuously checks the status of another device to determine if it needs attention**. This involves sending requests or queries to the device, waiting for a response, and repeating this process in a loop until the desired condition or data is obtained.
## Jerarquía de Memoria
- Memoria Caché.