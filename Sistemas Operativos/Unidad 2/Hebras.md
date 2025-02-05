## Hebra vs Proceso
**Dos conceptos claves de un proceso:**
1. Programa en ejecución, puede ser planificada.
2. Entidad que posee un programa ejecutable, area de datos globales, user stack, etc...
> El primero describe una hebra, el segundo describe los recursos de un proceso, **todo proceso tiene por lo menos una hebra.**
### Elementos de un Proceso
- Estado de ejecución
- Contexto de proceso
- Programa ejecutable (texto)
- Area de datos globales
- Stack de usuario y sistemas
- Identificadores

![[Pasted image 20241028140345.png]]

### Elementos de una Hebra
- Estado de Ejecución
- Contexto de Hebra
- Almacenamiento estático de memoria por variables locales
- Stack de ejecución
- ID de hebra
- Derecho a acceder a datos globales y recursos
![[Pasted image 20241028140519.png]]
## Concepto de Hebra
> Una hebra existe dentro de un proceso y usa sus recursos. Tiene un flujo de control independiente. Comparte con otras hebras del proceso los recursos dados. Hebra deja de existir una vez se termina su proceso. Hebra es lightweight.
- Demora menos crear y eliminar una hebra que un proceso.
- Demora menos en hacer cambio de contexto entre hebras de un mismo proceso, en vez de dos distintos.
- Cómo comparten archivos y memoria, no se requiere invocar rutinas de kernel para comunicación
- Permitiría ejecución paralela de hebras con varios procesadores.
>[!WARNING]
>Concurrencia $\neq$ Paralelismo

***
## Hebras a Nivel Usuario
>[!TIP]
>Toda la administración de hebras la realiza la aplicación misma (proceso) o por librerías de manejo de hebras
- **El Kernel no sabe de la existencia de las hebras**
- Scheduling es a nivel de procesos
![[Pasted image 20241028143632.png|500]]
## Hebras a nivel de kernel
>[!TIP]
>Kernel mantiene información de contexto por el proceso y por las hebras del proceso. La administración de hebras es realizada por el kernel.

![[Pasted image 20241028144509.png|470]]
## Symmetric Multiprocessing (SMP)
> En un sistema operativo SMP, **varios procesadores residen en la misma tarjeta** o el mismo chip (multicore).
> Todos **comparten un mismo bus para acceder la memoria**, dispositivos de I/O. Cada uno tiene su propia caché.

**SMP permite utilización de procesadores de forma simétrica**
- SO puede correr en cualquiera de los procesadores
- SO puede asignar hebras o procesos a cualquier procesador
- Posible que SO corra en más de un procesador

>[!INFO] 
>Sistema SMP junto con programación multihebras (multithreading), *a nivel de kernel,* permite paralelismo.

![[Pasted image 20241028144934.png]]
## API Pthreads
> [!HINT]
> API: Application Program Interface, librería del C.

1. **Administración de Hebras:** creación destrucción.
2. **Sincronización de Hebras:** 
	1. **Mutex**: Creación, inilcialización, locking.
	2. **Variables de condición**: mecanismo de comunicación entre hebras, comparten mtuex.

