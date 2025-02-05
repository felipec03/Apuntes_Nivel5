>[!INFO]
>Un semáforo es una variable especial, objeto, usada para que dos o más procesos se señalicen mútuamente

- Variable entera, accedida solo por dos operaciones:
	- `wait(s)`: decrementa semáforo; si resultado es negativo, proceso se bloquea.
	- `signal(s)`: incrementa semáforo; si resultado es menor igual que 0, se despierta proceso bloqueado por wait.

### Pseudo Semáforo
> SO asegura que operaciones son atómicas, son compartidos por varios procesos/hebras.

```c
struct semaphore{
	int count;
	queueType queue;
}

void wait(semaphore s){
	s.count--;
	if(s.cout <0){
		queue.push(proceso);
		proceso.block();
	}
}

void signal(semaphore s){
	s.count++;
	if(s.count <= 0){
		queue.pop(proceso);
		proceso.ready();
	}
}
```

## Semáforo binario, lock o Mutex
>[!TIP]
>Solo puede tomar valores de 0 o 1, no se puede inicializar con otro valor.

```c
void wait(semaphore s){
	if(s.value == 1){
		s.value = 0;
	}
	else{
		queue.push(process);
	}
}

void signal(semaphore s){
	s.count++;
	if(s.count <= 0){
		queue.pop(proceso);
		proceso.ready();
	}
}
```

### Exclusión Mutua usando Semáforos
> Para esta solución se pueden usar semáforos binarios o contadores.

```c
semaphore s;
Process(i){
	while(true){
		// NO CRÍTICO
		// ...
		wait(s);
		SC();
		signal(s);
		// ...
		// NO CRÍTICO
	}
}

void main(){
	s = 1;
	parbegin(P(1), P(2), ..., P(n));
}
```

>[!NOTE]
>Cómo la cola de procesos del semáforo no está sujeta a prioridad, **no produce inanición, ni deadlock**, satisfaciendo los 4 criterios previos.

## Problema del Productor/Consumidor
- Una o más tareas **productoras** generan datos y se almacenan en buffer compartido
- Tarea **consumidor** consume datos del buffer uno a la vez
- **Sólo un agente puede acceder al buffer a la vez.**

![[Pasted image 20241111142744.png|550]]
### Buffer Circular
- in: posición donde ingresa el próximo item.
- out: posición del próximo elemento a consumir

![[Pasted image 20241111143027.png]]

### Buffer infinito, semáforos binarios dos soluciones.
```c
int n; // Contador de elementos del buffer
binary_semaphore s = 1; // Iniciar buffer
binary_semaphore_delay = 0; // Tiempo de espera

producer(){
	while(true){
		v = produce();
		wait(s);
		buffer[in] = v;
		in = (in+1)%sizeofbuffer;
		n++;
		if(n==1){
			signal(delay);
		}
		signal(s);
	}
}

consumer(){
	int m;
	wait(delay);
	while(true){
		wait(s);
		w = buffer[out];
		out = (out + 1)%sizeoffbuffer;
		n--;
		// Acá puede ocurrir error en caso de que no se haga copia de m y n
		m = n;
		signal(s);
		consume();
		if(m == 0){
			wait(delay);
		}
	}
}

void main(){
	n = 0;
	parbegin(producer, consumer);
}
```

- Semáforo `s` se usa para asegurar exclusión mutua.
- `delay` permite que el consumidor espere si el buffer está vacío.

>[!WARNING]
>Revisar en el libro si es solución correcta. El caso depende en el consumer, para decrementar n, cómo producer puede incrementar n, ocurren inconsistencias a largo plazo en el programa; en cambio, cuando se copia se evitan esas inconsistencias,  no importa si n incrementa, ¡solo se pregunta por valor correcto!
#### Otro caso hipotético
>[!NOTE]
>En caso de que se pregunte de inmediato, sin tener en cuenta utilización de `m`, entonces ocurre Deadlock, puesto que `wait(delay)` viene antes de `signal()`, entonces se quedaría esperando por siempre.

```c
consumer(){
	int m;
	wait(delay);
	while(true){
		wait(s);
		w = buffer[out];
		out = (out + 1)%sizeoffbuffer;
		n--;
		if(m == 0){
			wait(delay);
		}
		// Acá puede ocurrir error en caso de que no se haga copia de m y n
		signal(s);
		consume();
		
	}
}
```

### Buffer Finito Circular, solución con semáforos contadores
```c
binary_semaphore s = 1; // Iniciar buffer
semaphore full = 0;
semaphore empty = sizeoffbuffer();

producer(){
	while(true){
		v = produce();
		wait(empty);
		wait(s);
		put_in_buffer(v);
		signal(s);
		signal(full);
	}
}

consumer(){
	while(true){
		wait(full);
		wait(s);
		w = take_from_buffer();
		signal(s);
		signal(empty);
		consume(w);
	}
}
```

- **Empty**: Contar número de espacios vacíos
- **Full**: Contar número de espacios utilizados
- **Semáforo s**: Usado para asegurar exclusión mutua.

## Problema de los filósofos comensales
> Cinco filósofos están sentados alrededor de una mesa con una fuente de spaghetti, **cuando un filósofo no está comiendo, está filosofando**. Cómo carecen de habilidades manuales, **requieren de dos tenedores para comer, habiendo solo cinco.** Cuando uno o dos de los tenedores están siendo ocupados por el filósofo de al lado, el filósofo vuelve a filosofar. Cuando un filósofo deja de comer, deja los dos tenedores en la mesa.

### Solución 1 (naive)
...
### Solución 2 (restricción)
```c
asdasdas
```
## Problema de lectores/escritores
> Escritores modifican archivo, lector lee archivo. Notar que un lector no puede escribir a un archivo, entonces existen dos grupos distintos. Además, puede existir más de un lector leyendo un archivo.

### Solución 1 (naive)
```c
int readcount;
semaphore x = 1;
semaphore wsem = 1;

void reader(){
	while(true){
		// Exclusión mutua -> sección crítica
		wait(x);
		readcount++;
		// Si soy el primer lector
		if(readcount == 1){
			// Bloqueo a escritores, no puede ingresar escritor
			wait(wsem);
		}
		// Libera sección crítica y lee.
		signal(x);
		// Empieza a leer
		READ();
		// sección crítica
		wait();
		// Si no soy el primer lector, no me preocupo de escritores
		// Decrementa número de lectores
		readcount--;
		// Si es el último lector, "abre la puerta" a los escritores
		if(readcount == 0){
			// Se abre la puerta a escritores
			signal(wsem);
		}
		// Libera sección crítica
		signal(x);
	}
}

void writer(){
	while(true){
		// Si logra ingresar, bloquea lectores
		wait(wsem);
		WRITE();
		signal(wsem);
	}
}

int main(){
	readcount = 0;
	parbegin(reader, writer);
}
```

- **Lectores** tienen **prioridad** -> existe **inanición** para los **escritores**.
- Nada impide que vengan los lectores.
### Solución 2 (priority swap)
```c
int readcount;
semaphore x = 1;
semaphore y = 1;
semaphore z = 1;
// Write semaphore
semaphore wsem = 1;
// Read semaphore
semaphore rsem = 1;

void reader(){
	while(true){
		// Exclusión mutua, varias capas
		// X y Z son mutuamente exclusivos
		wait(z);
			// Espera a los escritores, tienen prioridad
			wait(rsem);
				// Mientras ocurra esto, se bloquea rsem
				wait(x);
				readcount++;
				if(readcount == 1){
					wait(wsem);
				}
				signal(x);
			signal(rsem);
		signal(z);
		// Varios lectores
		READ();
		wait(x);
		readcount--;
		// último lector hace lo mismo que ejemplo anterior
		// cede el paso a escritor si es el último
		if(readcount == 0){
			signal(wsem);
		}
		signal(x);
	}
}


void writer(){
	while(true){
		// Exclusión mutua -> sección crítica
		wait(y);
		writecount++;
		// primer/único escritor
		if(writecount == 1){
			wait(rsem);
		}
		signal(y);
		signal(rsem);
		WRITE();
		signal(wsem);
		wait(y);
		writecount--;
		if(writecount == 0){
			signal(rsem);
		}
		signal(y);
	}
}

int main(){
	readcount, writecount = 0;
	parbegin(reader, writer);
}
```
- Ahora se pregunta si es que es el primer escritor con su propio semáforo, para este caso **los escritores tiene la prioridad.**
- El orden de llegada de las hebras depende del orden de llegada, en `rsem`.
- Dependiendo del orden, si llegan muchos escritores, se puede decir que la inanición la sufren los lectores.
### Problema típico con semáforos
- Posible error de programador al momento de la inicialización del semáforo

## Monitor
> Palabra conocida, el monitor es un objeto que **provee exclusión mutua** sin que se preocupe el programador, este garantiza que pasen errores de exclusión mutua.

- Variables locales se acceden dentro del monitor, por funciones del monitor.
- Para ingresar, proceso invoca funciones del monitor.
- Sólo un proceso puede estar siendo ejecutado en monitor -> EM.
- **Variables de condición:**
	- Si un proceso necesita suspenderse, hasta una condición.
	- Mecanismo de sincronización para suspender proceso dentro del monitor y reanudar su ejecución.
- Herramienta nueva de sincronización.

#### Estructura del Monitor
>[!NOTE] 
>Hebras ingresan en FIFO

![[Pasted image 20241112143211.png]]
### Variables de condición
- `wait() (cwait)`: Siempre bloquea la ejecución del proceso llamado en la variable de condición.
- `signal (csignal)`: Se libera la exclusión mutua de la variable de condición, resume ejecución de algún proceso bloqueado por `wait()`.
- Se parecen a las de semáforo, pero no son lo mismo; cumplen una función completamente distinta.

### Productor/Consumidor con monitor
>[!TIP]
>poner el buffer dentro del monitor

**Dos métodos principales del monitor:**
1. `append(x)`: agrega el item $x$ al buffer
2.  `v = take()`: saca un item del buffer y lo retorna en $v$

> Dos procesos clientes del monitor: `Producer()` y `Consumer()`. Sólo uno de los procesos puede estar ejecutando métodos del monitor a la vez

```cpp
monitor M {
	char buffer[N];
	int in, out;
	int count;
	cond notfull, notempty;
	public:
		M() : in(0), out(0), count(0) {}
		void append(char x);
		char take();
};

void M::append(char x) { 
	if (count == N){
		Wait(notfull);
	}
	buffer[in] = x;
	in = (in+1) % N; 
	count++; 
	Signal(notempty); 
}

char M::take() {
	if (count == 0){}
		Wait(notempty);
	}
	v = buffer[out];
	out = (out+1) % N;
	count--;
	Signal(notfull);
}
```

## Mutex
>Un "mutex" es la forma más básica de sincronización y provee el mismo servicio que un semáforo binario. Se utiliza para **implementar acceso exclusivo a las secciones críticas.**

>[!WARNING]
>Mutex debe ser inicializado, si no sería error de ejecución.

```c
#include <pthread.h>
pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER
pthread_mutex_lock(&m);
pthread_mutex_unlock(&m);
```

### Variables de condición de mutex
> Las variables de condición requieren de un mutex, no pueden trabajar por si mismas, necesita de una sección crítica.

### Productor consumidor con pthreads
```c
#include <pthread.h>
typedef struct {
	int buf[QUEUESIZE];
	int head, tail;
	int full, empty;
	pthread_mutex_t mutex;
	pthread_cond_t notFull, notEmpty;
} buffer_t;

void main() {
	buffer_t *buffer;
	// Hebras de consumidor y productor.
	pthread_t pro, con;

	// Ambas hebras comparten el buffer
	buffer = bufferInit();
	pthread_mutex_init(&buffer->mutex, NULL);
	pthread_cond_init(&buffer->notFull, NULL);
	pthread_cond_init(&buffer->notEmpty, NULL);

	pthread_create(&pro, NULL, producer, (void *) buffer);
	pthread_create(&con, NULL, consumer, (void *) buffer);
	
	pthread_join (pro, NULL);
	pthread_join (con, NULL);
	
	exit 0;
}

```

#### Productor
```c
void *producer(void *arg)
{
	int v;
	buffer_t *buffer;
	buffer = (buffer_t *) arg;
	while (true) {
		v = produce();
		// Inicia sección crítica
		pthread_mutex_lock (&buffer->mutex);
		while (buffer->full) {
			// Cierra la puerta
			pthread_cond_wait (&buffer->notFull, &buffer->mutex);
		}
		put_in_buffer(buffer, v);
		// Cede el paso
		pthread_cond_signal(&buffer->notEmpty);
		// Termina sección crítica
		pthread_mutex_unlock(&buffer->mutex);
	}
}
```

#### Consumidor
```c
void *consumer(void *arg)
{
	int v;
	buffer_t *buffer;
	buffer = (buffer_t *) arg;
	
	while (true) {
		v = produce();
		// Inicia sección crítica
		pthread_mutex_lock (&buffer->mutex);
		while (buffer->full) {
			// Cierra la puerta
			pthread_cond_wait (&buffer->notEmpty, &buffer->mutex);
		}
		put_in_buffer(buffer, v);
		// Cede el paso
		pthread_cond_signal(&buffer->notFull);
		// Termina sección crítica
		pthread_mutex_unlock(&buffer->mutex);
	}
}
```

## Barrera para hebras
...
