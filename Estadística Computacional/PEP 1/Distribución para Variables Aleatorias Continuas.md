## Distribución Uniforme Continua
> Una Variable aleatoria continua $(X)$ sigue una distribución uniforme continua si puede tomar cualquier valor en un intervalo $[a, b]$, donde $X \sim U(a,b)$

- Definición: 
$$
f(x) = \frac{1}{b-a}; \ \   a<x<b
$$
- En R:
```r
# acumulada uniforme
punif(target, lowerlimit, upperlimit)
```
## Distribución Normal
> Sea $X$ una variable continua. Decimos que $X$ sigue una distribución normal con media $\mu$ y desviación estándar $\sigma$, denotada como: $X \sim N(\mu, \sigma, 2)$ si su función de densidad de probabilidad está dada por una campana simétrica que se concentra alrededor de $\mu$ y cuya forma depende de $\sigma$.

- Definición fea:
$$
f(x) = \frac{1}{\sigma 2 \pi} e^{-\frac{(x-\mu)}{2\sigma^2}}
$$
- Cómo es simétrica:
	- La media siempre está al medio
	- La desviación además es simétrica
![[Pasted image 20241022110128.png]]

## Normalización de Puntuación Exacta
$$
z = \frac{x - \mu}{\sigma}
$$
- Se usa para "normalizar" datos de una muestra.

## Aproximación de Binomial y Poisson:
- Ejercicios de Binomial:
- Ejercicios de Poisson:
***
## Distribución Exponencial
> La variable aleatoria $X$ que es igual a la distancia entre eventos sucesivos de un proceso de Poisson con un número medio de eventos.
> Generalización de experimento de Poisson, se tienen n instancias respecto del tiempo.

*El tiempo de revisión del motor de un avión sigue una distribución exponencial con media 22.*
1. Encontrar la probabilidad de que el tiempo de revisión sea menor a 10 minutos.
```r
# p(x<10) =? lambda = 22
lambda = 22
time = 10 # en minutos
pexp(time, rate=1/lambda)
```

2. ¿Cuál es el tiempo de revisión de un motor superado por el 10% de los tiempos de revisión?
```r
# p(x<10) =? lambda = 22
lambda = 22
suceso = 1 - 0.1
qexp(suceso, 1/lambda)
```

## Distribución Erlang y Gamma
> Generalización de la distribución exponencial para $r > 1$. Para asignar valor positivo a $r$, podemos definir la distribución gamma

*Si un componente eléctrico falla una vez cada 5 horas,*
1. ¿Cuál es el tiempo medio que transcurre hasta que fallan dos componentes?
El tiempo medio se hace con ecuación de tiempo medio  $r/ \lambda$
```r
# Tiempo medio = r/λ
#=====================
r=2 #r
lambda = 1/5
respuesta1 = r/lambda
print(respuesta1) #10 horas
```
2. ¿Cuál es la probabilidad de que transcurran 12 horas antes de que fallen los dos componentes?

## Distribución de Weibull
Usos
- Análisis de supervivencia
- Modelar procesos de fabricación, etc... 
El tiempo T en segundos que tarda en conectarse a un servidor durante un día laboral sigue una distribución de Weibull de parámetros α = 0.6 y β = 1/4, mientas que un fin de semana es una Weibull de parámetros $α = 0.24$ y $β = 1$. Preguntas:
1. Cuál es el tiempo medio que tardaremos en conectarnos en ambos tipos de día
```r
# Tiempo en la semana
scale_sem=0.6 #Scale (b en R)
shape_sem=1/4 #Shape (a en R)
media_sem = scale_sem * factorial((1+1/shape_sem)-1)
print(media_sem) #14.4 horas

# Tiempo en fin de semana
scale_fsem=0.24 #Scale
shape_fsem=1 #Shape
media_fsem = scale_fsem * factorial((1+1/shape_fsem)-1)
print(media_fsem) #0.24 horas
```
2. Calcular, para ambos tipos de día, la probabilidad de tardar más de 10 segundos en realizar la conexión
```r
x=0.1
res_sem=pweibull(0.1,shape=shape_sem,scale=scale_sem,lower.tail = F)
print(res_sem) #0.52785
res_fsem=pweibull(0.1,shape=shape_fsem,scale=scale_fsem,lower.tail = F)
print(res_fsem) #0.6592406
```
## Distribución LogNormal
*La vida útil (en horas) de un láser semiconductor tiene una distribución lognormal con
$θ = 10$ y $ω = 1,5$.*
¿Cuál es la probabilidad de que la vida útil exceda las 10 000 horas?
```r
theta <- 10
omega <- 1.5
probabilidad = 1 - plnorm(10000, theta, omega)
```
## Distribución Beta
*El servicio de una junta de velocidad constante en un automóvil requiere desarmado,
reemplazo de fuelle y armado. Suponga que la proporción del tiempo total de
servicio para el desmontaje sigue una distribución beta con $\alpha = 2,5$ y $β = 1$.*
¿Cuál es la probabilidad de que una proporción de desmontaje exceda $0,7$?
```r
alfa <- 2.5
beta <- 1
probabilidad = 1 - pbeta(0.7, alfa, beta)
```
