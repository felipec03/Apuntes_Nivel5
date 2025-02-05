### Probabilidad vs Posibilidad
> Ejemplo: Equipo de fútbol que históricamente no llega al campeonato, tiene un ascenso esporádico y tiene que ir contra el mejor equipo en el campeonato. ¿Por que equipo apostaría?

>[!Observación]
>En este caso, es posible, osease que está en el reino de posibilidades. Pero, es poco probable, osease que tiene baja taza de ocurrencia.

- Posibilidades: Tiene 3 opciones, o pierde, empata o gana.
- Probabilidades: Tiene pocas probabilidades.
### Modelo Probabilista
- Elemento estocástico: En un proceso, si existe un componente aleatorio, pertenece a un modelo estocástico. Está sujeto a aleatoriedad.
- Elemento determinístico: En algún proceso, si se que algo va a ocurrir con certeza.
***
- **Experimento Aleatorio** ($E$): Cualquier experiencia donde interviene el azar.
- **Espacio Muestral** ($\Omega$): Conjunto de todos los resultados posibles de un experimento aleatorio.
- **Evento** ($A \in \Omega$): Es un subconjunto del espacio muestral del experimento aleatorio.

### Tipos de Espacios Muestrales
- **Espacio Muestral Discreto:** Corresponde a un conjunto finito o infinito numerable de resultados. (Ej: Lanzamiento de dados)
- **Espacio Muestral Continuo:** Contiene un intervalo, finito o infinito, de resultados números reales. (Ej: Longitud de onda de una señal en el espectro electromagnético)
### Conceptos Previos 
- Definición formal de una variable aleatoria: Función que asigna resultados de un experimento aleatorio a elementos del espacio muestral.
- Función de **distribución de variable aleatoria**: $F_x(x) =  \text{Pr} \{X \leq x \} , F_x: \Omega \rightarrow [0,1]$
- Eventos **mutuamente excluyentes**: $E_1 \cap E_2 = \phi$
- **Espacio muestral equiprobable**
	- Es un espacio muestra que consta de $N$ resultados posibles que tienen la **misma probabilidad**
	- La probabilidad de cada resultado es $\frac{1}{N}$
- **Eventos mutuamente excluyentes**
	- En espacio discreto, probabilidad de un evento $P(E)$, es igual a la suma de las probabilidades de los resultados en $E$. (*Varía entre variable discreta y continua*) ( $\sum_i^n P(E_i) = 1$)  o ($\int_i^n P(E(t))dt$)
### Axiomas de probabilidades
>[!Def]
>La probabilidad es un número que se asigna a cada miembro de una colección de eventos de un experimento aleatorio y satisface las siguientes propiedades:
- $P(\Omega) = 1$, donde $\Omega$ es el espacio muestral
- $0 \leq P(A) \leq 1$, para cualquier evento $A$
- Para dos eventos $A_1$ y $A_2$ que son mutuamente excluyentes $A_1 \cap A_2 = \emptyset$ ; $P(A_1 \cup A_2) = P(A_1) + P(A_2)$
- $P(\emptyset) = 0$
- $P(A') = 1 - P(A)$
***
### Unión de eventos y Reglas de adición
1. Probabilidad de unión: $P(A\cup B) = P(A) +P(B)-P(A \cap B)$
2. Probabilidad de unión entre eventos mutuamente excluyentes: $P(A\cup B) = P(A)+P(B)$
3. Probabilidad de unión ante tres eventos: 
4. Probabilidad de muchos eventos mutuamente excluyentes:

> Calcular la probabilidad de que al lanzar dos dados, salga un número 1 en el primer dado o un numero 1 en el segundo dado $P(A \cup B)$
- $P(A)$ : Probabilidad de sacar un 1 en el primer dado: $1/6$
- $P(B)$ : Probabilidad de sacar un 1 en el segundo dado: $1/6$
- $P(A \cap B)$ : Probabilidad de sacar un 1 en ambos dados: $1/36$
$$P(A \cup B) = 1/6 + 1/6 - 1/36 = 11/36$$
Ejemplo:
> En un juego de mesa de carreras de autos se lanzan dos dados, avanzando una posición el auto cuyo identificador equivale a la suma de ambos dados:
### Probabilidades Condicionadas
>[!Definición]
>Sabiendo que ocurrió B, estoy preguntando por la probabilidad de A.

$$P(A|B) = \frac{P(A \cap B)}{P(B)}$$
>Un 60% de los hombres que entran a un supermercado compran cerveza, el 20% de los hombres que ingresan compran pañales y un 10% de los hombres compran pañales y cerveza. *¿Cuál es la probabilidad de que un hombre lleve pañales, dado que llevó cerveza?*

$$P(A|B) = \frac{10\%}{60\%} = 16.67\% $$
### Reglas de intersección y multiplicación
1. Regla de multiplicación
2. Regla de probabilidad total
3. Generalización
### Regla de Independencia
1. Dos eventos serán independientes si:
	1. $P(A|B) = P(A)$
	2. $P(B|A) = P(B)$
	3. $P(A \cap B) = P(A) \cdot P(B)$
2. N eventos son independientes entre sí, si es que:
$$P(E_1 \cap ...\cap E_n) = P(E_1) \times \dots \times P(E_n)$$
### Teorema de Bayes
>El teorema de Bayes es útil cuando queremos actualizar nuestras creencias sobre un evento a medida que obtenemos nueva información. Por ejemplo, si sabemos la probabilidad de que llueva y vemos nubes oscuras, podemos usar el teorema de Bayes para calcular la probabilidad actualizada de que llueva dado que vemos las nubes.
- Tenemos que: $P(B|A) = \frac{P(A|B)P(B)}{P(A)}$
- Así: $P(B|A) = \frac{P(A|B) P(B)}{P(A|B)P(B)+P(A|\hat{B} P(\hat{B}))}$
- Suponiendo que $B$ tiene $n$ particiones, $B_1, \dots, B_n$, entonces:
$$
P(B_j|A) = \frac{P(A|B_j) \times P(B_j)}{\sum_{j=1}^k P(A|B_j) \times P(B_j)}
$$
***
### Ejemplo: Probabilidad que 2 personas tengan el mismo cumpleaños en la clase
- Debemos encontrar la intersección:
- Buscamos el complemento del suceso, $\hat{X}$
$$
P(\hat{X}) = \frac{365}{365} \times \frac{364}{365} \times \dots \times \frac{365-30}{365} = \frac{365 }{365^{31}} = 0,268
$$
- Entonces, para encontrar el suceso, calculamos $1-\hat{X}$
$$P(X) = 1 - 0,268 = 0,732 = 73,2\%$$
### Conteo 
- Suponiendo que los eventos a calcular son independientes entre sí
- Si queremos el número total de eventos lo podemos calcular cómo:
$$n_1 \times n_2 \times \dots \times n_r = n^r$$
### Permutaciones y Combinaciones
> Dado un conjunto, una **permutación** es la **organización** de sus elementos, teniendo en cuenta su orden. Por otro lado, en una **combinación**, el **orden no es relevante**.

- Las **permutaciones** corresponden a diferentes agrupaciones de elementos donde el orden sí importa.
$$n! = n \times (n-1)!$$
- El número de permutaciones de un subconjunto de $r$ elementos seleccionados
$$P_r^n = n \times (n-1) \times \dots \times (n-r-1) = \frac{n!}{(n-r)!}$$
- El número de permutaciones de $n = n_1+ \dots+ n_r$ elementos de diferente tipo
$$P^n_r = \frac{n!}{n_1! \times n_2! \times \dots \times n_r!}$$
***
> Las combinaciones son diferentes agrupaciones con todos los elementos donde el orden no importa.

- El número de **combinaciones** para un subconjunto de $r$ elementos que pueden ser seleccionados entre $n$ elementos es denotado como
$$C^n_r = {n\choose k}= \frac{n!}{r!(n-r)!}$$
![[Pasted image 20241008105848.png|center]]

