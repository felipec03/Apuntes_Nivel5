![[Pasted image 20250108181603.png]]
a) Falso, el procesador no emite direcciones físicas, emite virtuales.
b) Falso, no siempre los procesos emiten direcciones válidas (seg fault).
c) Falso, peor implementación de compartición, se usan espacios compartidos.
d) Verdadero, movimiento entre niveles de memoria es responsabilidad de SO.

![[Pasted image 20250108181817.png]]
a) Falso, segmentación -> externa
b) Falso, paginación -> interna
c) Verdadero, paginación y buddy -> interna
d) Falso, buddy -> interna

![[Pasted image 20250108182030.png]]
a) Falso, en segmentación se suma, en paginación se concatena.
b) Falso, puede que memoria física sea distina a la memoria virtual, por lo que distinta distribución de bits
c) Verdadero, solamente porciones necesitadas pueden estar en memoria. "La misma tabla puede ser paginada para guardar memoria, con las porciones necesarias activas"
d) Falso, puede no ser válido -> page fault

![[Pasted image 20250108183310.png]]
a) No
b) No
c) No
d) Largo tira = $12$ -> $16^{12}$ posibilidades... $2^{4 \times 12} = 2^8 \cdot 2^{40} = 256 [TB]$
#### Para pregunta 5 y 6
![[Pasted image 20250108183616.png]]
- offset: $2048 = 2^{11} = 11 bits$
- bits página = $32 - 11 = 21[bits]$
- numero de páginas = $2^{21}$
- tira física = $128[MByte] = 2^{7} 2^{20} = 27 [bits]$, con $27 - 11 = 16[bits]$ tag.
- $\dfrac{2^{32}}{2^{27}} = \dfrac{2^5}{2^3} = 2[bytes]$
- Tabla de página = $2^{21} \cdot 2 = 4[MB]$
a) Falso, para el número de marco se necesitan 16 bits, osea 2 bytes.
b) Verdadero, ver cómputo.
c) Falso, se necesitan 21 bits.
d) Falso, se necesitan 16 bits.

![[Pasted image 20250108184659.png]]
- 0x01AEB211, sabiendo que offset es de 11 bits, hay que pasar a binario.
- 0x0000|0001|1010|1110|1011|0010|0001|0001
- Entonces, offset sería: 010|0001|0001, faltan 16 bits.
- 33 = 100001 -> para 16 bit: 0000000000100001
- Entonces, dirección en binario es: 0000000|0001|0000|1010|0001|0001
- Ahora, en hex: 0x00010A11
a) 0x00010A11, esa misma
b) no
c) no
d) no

#### Para las preguntas 7 y 8
![[Pasted image 20250108190803.png]]
- Tira virtual = 16 bits
- Offset = $2^{12} [bytes]$ -> $12[bits]$ 
- Tag Bits para direccionar segmentos = $16 -12 = 4 [bits]$, osease direcciona $2^4$ segmentos.
- bit presencia, bit modificación y 2 bits de control.
- Entrada en memoria = $16 + 12 + 4 = 32 [bits]$

| tag | offset | bits |
| :-: | :----: | :--: |
| 16  |   12   |  4   |

- tamaño tabla segmentos = $16 \cdot 4[KBytes] = 64[KB]$
a) El ancho de una entrada en la tabla de segmentos es de 4 bytes.
b) La tabla de segmentos tiene 64 entradas.
c) Ver cálculo completo.
d) La cantidad máxima de segmentos es $\dfrac{64}{4} = 16$

![[Pasted image 20250108192353.png]]
- Dirección virtual a binario = |0001|0010|1111|0000|
- 4 bits segmento = 0001
- 12 bits offset = 0010|1111|0000|
- Dirección base = 0x2020 = |0010|0000|0010|0000|
- Dirección física = Base + offset = |0010|0011|0001|0000|
- Dirección fisica hex = 0x2310, además
- $offset < segmentoCompleto \land |\text{0x2310}| = 16[bits]$ 

>[!INFO]
>Cómo el offset no se pasa, entonces no hay overflow -> no hay segmentation fault.

a) La dirección física es 0x2310 y es válida
b) Falso, si es válida por criterio de
c) Falso, no es la dirección.
d) Falso, no es la dirección.

![[Pasted image 20250108194219.png]]

- Entrada = 32 bits
- Offset = $2^{12}[Bytes]$ -> offset bits = $12 [bits]$
- Tag bits = $32 - 12 = 20[bits]$
- Tira para tablas = $20 - 12 = 8[bits]$.
- Páginas: $\dfrac{2^{20}}{2^{12}} = 2^8 = 256[KB]$ -> $\dfrac{256}{4} = 64[páginas]$ 

a) Se necesitan 12 bits de offset
b) Tamaño tabla de índices
c) Se reservan primeros 8 bits más significativos.
d) Tamaño máximo es $pesoPag + pesoPag\cdot Entradas$, osease
$4[KB] + 4 \cdot 2^{20} = 4100[KB]$

![[Pasted image 20250108223743.png]]

- Tira virtual = 32 -> Máximo tamaño mem virtual = $2^{32} = 4[GB]$
- Offset bits = $12[bits]$
- Al ser 4 entradas, entonces se necesitan 2 bits para direccionamiento
$$
TamañoMarco = Niveles \cdot TamañoEntrada
$$
- Entradas x Nivel = $4KB = N \cdot 64 [bits] = N \cdot 8[bytes]$ -> $N=2^9$
- 9 bits por entrada, entonces
 
**Memoria Virtual:**
- Offset universal
- nivel 1 -> direccionamiento
- entradas 1
- entradas 2 (copia para llegar a los 32 bits totales.)

| offset | nivel 1 | nivel 2 | nivel 3 |
| :----: | :-----: | :-----: | :-----: |
|   12   |    2    |    9    |    9    |

a) Falso, dir virtual de 32 bits -> 4GB máximo
b) Falso, para el primer nivel usa solamente 32 bytes, por lo que no alcanza a llenar una página.
c) Verdadero, para completar 32 bits, se necesitan de 3 niveles.
d) Dirección lógica necesita 12 bits de offset... $9+9+4+2 = 24$

![[Pasted image 20250108225613.png]]
- 

a) 
b) 
c) 
d) 

![[Pasted image 20250109092713.png]]
a) 
b) 
c) 
d) 

****
a) 
b) 
c) 
d) 