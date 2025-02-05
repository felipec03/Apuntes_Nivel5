## Estadística Inferencial
**Emplea:**
- Teorema del Límite Central
- Intervalos de Confianza
- Muestrea
- Error
- Contraste de hipótesis
**Con el objetivo de:**
- Inferir
- Prever
- Predecir
### Estimación de Parámetros
- Estimación Puntual
	- Método de Momentum
	- Máximaa verosimilitud
	- Mínimos cuadrados (no)
	- Métodos Bayesianos (más avanzado)
- Estimación por intervalo
### Teorema del Límite Central
> Establece que en muestras grandes, la distribución de la media se aproxima a una distribución normal, independientemente de la distribución de la población original.

>[!INFO]
>Si se toman infinitas muestras de una población, y de cada una se toma la media, entonces, va a tender a una **distribución normal.**
$$
\mu \to \infty = \text{distribución normal}
$$

### Intervalos de Confianza
> Generalmente se trabajan con distribuciones normales normalizadas.

#### Ejercicio:
- Desviación poblacional: 0,4
- Muestra: 100
- Media Muestral = 4,602
Estimar media poblacional:
- n = 100
- promedio = 4,602
- desviación = 0,4/srqt(100)
- $Z_{-\alpha / 2}$ = `qnorm()`
- $Z_{\alpha/2}$ = `qnorm()`
$$
\therefore 4.52 < \mu < 4.68
$$
**Ejercicio Neumáticos en Clase 13**
### Distribución de Frecuencia
- Bernoulli -> Binomial -> Geométrica
- Normal -> Otras continuas
### ¿Cómo estimar el tamaño de la muestra?
> Para una estimación con la fórmula

$$
n = \frac{Z_{\alpha} \cdot \sigma}{Err} 
$$
> Para una varianza menor, el $n$ debería disminuir considerablemente, el peor de los casos es cuando la varianza es muy grande, por ejemplo $\sigma = 0.5$.

### Pruebas de Contraste de Hipótesis
...
#### Contraste de Hipótesis para 3 o más muestras
> No basta con hacer las combinaciones de a pares, puesto que existen distintos errores asociados a los pares calculados, ejemplo:
> Queremos conocer si existe una diferencia entre las calificaciones de tres carreras de la facultad de ingeniería.
- Test Anova
- Test Kruskal-Wallis

>[!TIP]
>No hacerlo de a pares, usar los métodos previamente definidos.
### Coeficiente de Correlación
> Medida estadística que describe la relación entre dos variables y su dirección.
- Pearson: Correlación lineal entre dos variables
- Kendall: Concordancia lineal entre dos variavbles ordenadas.
- Spearman: Correlación no paramétrica entre dos variables.

>[!INFO]
>Todas siguen la misma lógica:
> $-1=$ Fuertemente Negativa
> $0=$ Sin correlación
> $1=$ Fuertemente positiva

**Además notemos**: $Correlación \centernot{\implies} Causa$
![[Pasted image 20241217112125.png|400]]

### Modelos de Regresión Lineal y Coeficiente de Correlación
> Modelar relación entre dos variables por medio de una regla que minimice el promedio de la distancia perpendicular entre los puntos, osease, **mínimos cuadrados.**

> Análisis de Regresión se usan para **predecir** el valor de una o más **variables dependientes**basándose en el valor de **otras variables independientes**.

- Función en R: `lm(dependiente, independiente)`, linear model.
#### ¿Cómo evaluamos?
- Pendiente (ANOVA F)
- Bandas de confianza
- Coeficiente de Determinación: $R^2$
- Residuos
>[!TIP]
>Mientras más asteriscos que hayan salido de `lm()`,  más fuertemente ligado.
- $R^2:$ Mientras más cercano a 1, mejor, ¿cómo se interpreta? 
	- Un $R^2 \cdot 100 \%=n\%$ de las variables dependientes, pueden ser "explicadas" por las variables independientes.
	- Mi modelo puede explicar un n% de las "ventas", variable independiente.
- Pero, ¿para más variables, aumenta la fidelidad del modelo?

>Tomemos el ejemplo visto en clase, si queremos ver donde invertir para publicidad dentro de Youtube, Facebook y Periódicos, se puede modelar por la siguiente función en un modelo lineal.
>Los valores para Facebook, Youtube y los Periódicos, se ve
$$Y(f, y, p) = 0.19f + 0.05y +-0.001p + 3.5$$
- Vemos que para los coeficiente para Facebook y Youtube hay gran valor de significancia, osease, si se inviertiera 0, afectaría enormemente.
- En cambio, el coeficiente para Periódico no afecta mucho si se inviertiera cero.
> Finalmente, podemos concluir que vale más invertir en Facebook y Youtube, más que solamente Youtube cómo visto previamente.


