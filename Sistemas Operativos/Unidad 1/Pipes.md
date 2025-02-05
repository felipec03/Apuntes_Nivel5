> Método de comunicación unidireccional entre dos procesos.

```c
#include <unistd.h>
// fd[2] arreglo de dos enteros que representa el pipe
// fd = {0,1} son descriptores
// fd[0] : descriptor para lectura
// fd[1] : descriptor de escritura

int pipe(int fd[2]);
fd[0] = read();
fd[1] = write();


// pipe no se pudo ejecutar
if(pipe() == -1){
	// ERROR
}
// enviar 5 caracteres
write(fd[1], "hola", 5);
read(fd[0], &buffer, s);


```

**Etapas para comunicar 2 procesos.**
1. Un proceso crea el pipe
2. El mismo proceso crea un hijo `fork()`
3. El padre cierra el lado de escritura `fd[1]`
4. El hijo cierra el lado de lectura `fd[0]`
5. Comienza transmisión de datos

**Para poder comunicar los dos procesos se utiliza pipe antes de fork, cómo fork es copia de proceso padre. La única forma que tiene el hijo de saber el pipe, es heredando del padre.**

```c
#include <unistd.h>
int main(){
	int* fd;
	int pipe(int fd[2])
	fork();
}
```
![[Pasted image 20241021141426.png]]
## `dup()` y `dup2()`
> Duplica un descriptor

```c
#include <unistd.h>
int dup(int fd); // Entrega un número, asigna alias de fd.
int dup2(int fd, int fnew); // Acá solicito que alias sea fnew.
```

- Descriptor 0: **STD_IN** - Terminal
- Descriptor 1: **STD_OUT** - Terminal
- Descriptor 2: **STD_ERR** - Terminal
