### Variables aleatorias discretas
>[!Definición]
Las variables aleatorias discretas son variables aleatorias cuyo intervalo de valores es finito o contablemente infinito
- Ejemplo: Número de goles, calificación de hoteles, goles de un partido de fútbol.
### Distribución de probabilidad
> La distribución de probabilidad de una variable aleatoria es una **función que asigna a cada suceso definido sobre la variable la probabilidad de que dicho suceso ocurra.**

- Ejemplo: Teléfonos en Mercado Libre con probabilidad de 0,2 de estafa.
### Función de probabilidad
> Una función de probabilidad (o masa de probabilidad) cumple con las siguientes propiedades:

**Esto aplica a variables discretas, si fuese continuo la variable puede dar resultados infinitesimales**
**Propiedades**
- No puede existir probabilidad negativa. $f(x_i) \geq 0$
-  $f(x_i) = P(X=x_i)$
- Suma de todas las probabilidades: $\sum_{i=1}^n (f(x_i)) = 1$

**Propiedades en distribución acumulada**
- $F(x) = P(X \leq x) = \sum_{X_{i \leq x}} f(x_i)$
- $0 \leq F(x) \leq 1$
- Si $x\leq y, F(x) \leq F(y)$
***
### Media y varianza
> Para una función de probabilidad para variables aleatorias discretas, se pueden calcular las siguientes métricas:

- Media, esperanza o valor esperado: Probabilidad que ocurra un evento por el evento en cuestión. Ejemplo, notas finales con ponderación.
$$\mu = E(x) = \sum_x xf(x)$$
- Varianza: Indicador de dispersión, cuanto se alejan los resultados de la media.
$$
\sigma^2 = V(x) = E(x-\mu)^2 = \sum_x (x- \mu)^2 f(x)
$$
- Desviación estandar: Raíz cuadrada de la varianza
$$
\sigma = \sqrt{\sigma²} = \sqrt{V(x)} 
$$
***
## Tipos de Distribuciones
### Ejemplo 1 - Sillas y Personas:
$n = 7, \ k=5$, permutaciones de 5 personas en 7 espacios
$$
P(7,5) = \frac{n!}{(n-r)!} = \frac{7!}{(7-5)!} = 2520
$$
### Ejemplo 2 - Donas
> No se llevan más de dos de verduras. Hay 6 tipos distintos, uno siendo de verdura y se deben llevar 24 neto.

$$
C = \binom{n+k-1}{k-1}, \ n=24 \ y \ k=\text{tipos}
$$

- Caso 1: No hay donas de verduras (n = 24 y k = 5)
$$
C_0= \binom{24 + 5 - 1}{5-1} = \frac{28!}{4!(28-4)!}
 = 20.475$$
- Caso 2: Se compra 1 dona de verdura (n = 23 y k = 5)
$$
C_1 = \binom{23+5-1}{5-1} = \frac{27!}{4!(27-4)!} = 17.500
$$
- Caso 3: Se compran 2 donas de verduras (n = 22 y k =5)
$$
C_2 = \binom{22+5-1}{5-1} = \frac{26!}{4!(26-4)!} = 14.950
$$
- Resultado: Cómo necesitamos Caso 1 o Caso 2 o Caso 3:
$$
Total = \binom{28}{4} + \binom{27}{4} + \binom{26}{4} = 52.925
$$
### Ejemplo 3 - Hackatón
> Pamela, Juan, Pedro y Javier han organizado un hackathon. Al finalizar el evento, se elaborará un ranking en el que no se permiten empates. ¿Cuántos rankings distintos pueden formarse en esta situación? Además, ¿cuántos rankings serían posibles si se permitieran empates?

- Caso 1: Sin empates, son permutaciones sin y es importante el órden. Con 4 participantes y 4 puestos posibles.
$$
P = \binom{4}{4} = 4! = 24
$$
- Caso 2: Con 1 empate: (combinación + permutación)
$$
C = \binom{4}{2} = \frac{4!}{2!(4-2)!} = 6.
$$
- Caso 3: Con 2 empates
- Caso 4: Con 3 empates
- Caso 5: Todos empatan
- Resultado: Caso1 + Caso2 + Caso3 + Caso4 + Caso5
***
## Media y Varianza de una variable aleatoria discreta
> Para una función de probabilidad se pueden calcular las siguientes métricas

- Media o valor esperado: $\sum_{x} xf(x) = \mu = E(x)$
- Varianza: $\sigma^2 = E(x-\mu)^2 =  \sum_x (x-\mu)^2f(x)$
- Desviación estandar: $\sigma$

### Distribución Uniforme
> Una **variable aleatoria discreta** $(X)$ tiene una **distribución uniforme** $(X \sim U[p])$ discreta si para cada valor en su rango tiene igual probabilidad

- **Definición**:
	- $f(x_1) = \frac{1}{n}$ y $X \sim U[a, a+1, a+2, ..., b]$, con $a \leq b$

- **Propiedades**:
	- $\mu = E(x) = \frac{b+a}{2}$
	- $\sigma^2 = \frac{(b-a +1)^2 +1}{12}$

### Distribución de Bernoulli
> Una **distribución de Bernoulli** $(X -B[p])$ considera una variable aleatoria $X$ que mide el número de éxitos de un único experimento aleatorio considerando dos eventos posibles: $\text{exito(1)} \lor \text{fracaso(0)}$, cuando $p$ es la probabilidad de éxito. 

- **Definición**:
	- $f(x) = p^x(1-p)^{1-x}; \ x\in \{0,1\}$
- **Propiedades**:
	- Media o valor esperado: $\mu = E(x) = \sum_x xf(x) = p$
	- Varianza = $p - p^2$

### Distribución Binomial
> La **distribución binomial cuenta el número de éxitos** $(X \sim Bin[n,p])$ en una secuencia de $n$ ensayos de Bernoulli independientes entre sí, con una probabilidad fija $p$ de ocurrencia del éxito entre los ensayos

- **Definición**: 
	- $x \in \{ 0, 1, 2, \dots, n\}$
	- $f(x) = \binom{n}{x}p^x(1-p)^{n-x}$
- **Propiedades**:
	- $\mu = n \times p$
	- $\sigma^2 = np(1-p)$
### Distribución Geométrica
> En distribución geométrica $(X \sim G[p])$ calcula el número de experimentos (sin éxito) que debemos realizar para **obtener un primer evento favorable usando una distribución de Bernoulli**. Esto es, considerando. *Cuantas veces tengo que hacer el experimento para obtener el primer éxito.*

- Definición:
	- asd
	- asd
- Media o valor esperado
	- asdasd
	- asdasd
### Distribución Binomial Negativa
> Una distribución binomial negativa es una **extensión del caso anterior**, correspondiendo $X$ al número de **ensayos que debemos realizar hasta obtener $r$ casos favorables**

- Definición:
	- $x \in \{ 0,1,2,3, \dots ,r, r+1 \}$
	- $f(x) = \binom{x-1}{r-1} p^r(1-p)^{x-r}$
- Propiedades
	- asd
	- asd

### Distribución Hipergeométrica
> La distribución hipergeométrica modela la probabilidad de obtener un número específico de éxitos al extraer elementos sin reemplazo de una población finita. En este contexto, se define como sigue:
- $N$: Tamaño total de la población
- $K$: Numero total de éxitos
- $n$: Tamaño de la muestra
- $k$: número de éxitos deseados en la muestra.
> Se utiliza en situaciones donde no se devuelve lo extraído, como en muestreos o loterías, y sus parámetros permiten calcular la probabilidad de obtener exactamente.

- Definición
	- $x \in \text{max} \{0, n+K-N \}$
	- La weá larga: $$f(x) = \frac{\binom{K}{x} \binom{N-K}{n-x} }{\binom{N}{n}}$$
### Distribución de Poisson
> Expresa,**a partir de una frecuencia de ocurrencia media** $(\lambda)$, la probabilidad de que ocurra $(X- P[\lambda])$ un **determinado número de eventos durante cierto período de tiempo** $(T)$. Concretamente, se especializa en la **probabilidad de ocurrencia de sucesos con probabilidades muy pequeñas**, o sucesos "raros".

**Definición:**
$$
f(x) = \frac{e^{-\lambda t} (\lambda T)^x}{x!} ; \ x \in \{0, 1, 2, \dots \}
$$

>[!Observación]
>$k$ es el número de ocurrencias del evento o fenómeno $\lambda$, es un parámetro positivo que representa el número de veces que se espera que ocurra el fenómeno durante un intervalo dado.

- Valor esperado:
- Varianza:
***
### Aplicación de distribuciones en R

>[!TIP]
> Existen 4 posibles sufijos para las `distribuciones` previamente vistas en R

- `ddistribucion`: "Probability function"
- `pdistribucion`:  "Cumulative distribution function"
- `rdistribucion`: "Pseudorandom number generation"
- `qdistribucion`: "Quantile function"
