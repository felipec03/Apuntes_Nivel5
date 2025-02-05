> [!INFO]
> Diversidad de dispositivos I/O y ss diferencias hacen difícil la tarea de proveer una solución uniforme.

#### Importante: Acceso Directo a Memoria (DMA)
- Procesador instruye a módulo DMA que transfiera uno o más bloques.
- Módulo DMA controla completamente la transferencia de datos entre dispositivo I/O y memoria principal, **sin que intervenga el procesador.**
### Buffering de I/O
#### Buffer Simple

- $T:$ Tiempo para leer el bloque
- $C:$ Tiempo de cómputo entre lecturas
- $M:$ Tiempo para mover datos desde SO a Memoria Usuario.

```c
read(file, image, 3*2024*2024*sizeof(int));
while(){
	read() // -> T
	process() // -> C
}
```

Al momento de hacer el cómputo, si el procesamiento ($T$), es muy corto
$$
\max{(C, T)} + M
$$

![[Pasted image 20241217150920.png]]

![[Pasted image 20241217150949.png]]

****

>[!WARNING]
>Entra hasta acá.

