```c
#include <stdio.h>
#include <unistd.h>
#include <pthreads>

pthread_mutex_t mutex[2];

void* writerA(void* param){
	// SECCIÓN CRÍTICA

	int i;
	for(i = 0; i<6;i++){
		pthread_mutex_lock(&mutex[0]);
		printf(param);
		fflush();
		pthread_mutex_unlock(&mutex[1]);
	}
	pthread_exit(NULL);

}

void* writerB(void* param){
	// SECCIÓN CRÍTICA
	int i;
	for(i = 0; i<6;i++){
		pthread_mutex_lock(&mutex[0]);
		printf(param);
		pthread_mutex_lock(&mutex[1]);
		signal(semA)
	}
	pthread_exit(NULL);
}

void main(){
	pthread_t threads[2];
	pthread_create(&threads[0], NULL, writerA, (void*) 1);
	pthread_create(&threads[1], NULL, writerB, (void*) 2);
}
```

