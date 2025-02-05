#### Pregunta 1
> Sistema de 14 bits, con páginas de 4KB, tabla de segmentos. 

asig | dir base
0.  | 0x1000
1.  | 0x0000
2.  | 0x3000
3.  | ...

```asm
.code 0x0240: PUSH #0x1108
	  0x0244: CALL SIN
	  0X0248:
	  .
	  .
	  .
.SIN 0X0360: MOVE (SP), R2
	 0X0364: MOVE R2, R3
	 .
	 .
	 .
	 RETURN
```

a) ¿Donde comienza el programa principal en memoria física?
Dirección Virtual -> Dirección física, entonces: 
$4KIB = 2^2 \cdot 2^{10} [B] = 2^{12} = 12[bitsOffset]$
$14 -12 = 2 \Rightarrow 4[segmentos]$

0x0240_{hex} = 00[00]|0010|0100|0000
$00_2$ = $0_{10}$ , entonces cae en segmento 0.
Así, dirección física  = Dirección Base + Offset
Entonces, dirección física = 0x1000_16 + 0x0010|0100|0000

b) ¿Si el `SP = 0x1108`, donde está el SP en memoria física?
Acá, es la misma lógica que el anterior
Dir Física = Dir Base SP + Offset
#### Pregunta 2
> Una aplicación necesita $10GB$ de **RAID 1** para almacenar archivos de log, $20GB$ de **RAID 5** para archivos de datos y $20GB$ de **RAID 5** para Sistema Operativo ($20[GB]$). Suponga que todos los discos son de $10GB$ de capacidad, ¿cuantos discos en total se necesitan?

- Para archivos log: 2 discos
- Para archivos de datos: 2 discos para 20 GB y uno adicional para almacenar datos.
- Para sistema operativo: 2 disco y otro disco adicional para datos.
<-> Se necesitan 8 discos.
#### Pregunta 4
> Considere el siguiente array RAID 5

| Disk 0   | Disk 1   | Disk 2   |
| -------- | -------- | -------- |
| 0XFA     | 0X35     | **0XCF** |
| 0X18     | **0X39** | 0X2C     |
| **0X11** | 0X01     | 0X10     |
Donde bold text: Bloque de prioridad.

> Muestre el estado final del array después de escribir `0xAA0F71C2` cargado en el bloque 1.

Bloque 1 = 0X35

`0xAA(1)|0F(3)|71(5)|C2(7)`

> Cuando es una escritura, hay dos lecturas en paralelo más la escritura. 
> Nuevo bloque paridad = Dato Antiguo + Dato Nuevo + Antiguo Dato Paridad, usar XOR.

a) $NuevoBloqueParidad = 0x35 \oplus 0xAA \oplus 0xCF$ = 0x50

> Ahora, se debe reescribir stripe

b) 0x0F + 0x01 = 0x7E

c) 0x11 + 0x01 + 0xC2 = 0XD2

| Disk 0   | Disk 1   | Disk 2   |
| -------- | -------- | -------- |
| 0XFA     | **0XAA** | **0X50** |
| **0X0F** | 0X7E     | **0X71** |
| 0Xd2     | **0XC2** | 0X10     |
