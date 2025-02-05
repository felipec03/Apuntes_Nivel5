## Pruebas sobre la media de una distribución normal, varianza desconocida (una cola)
> Suponga que el fabricante afirma que la vida media de una ampolleta es de más de $10,000 [horas]$. En una muestra de $30$ ampolletas se encontró que sólo duran $9,900 [horas]$ en promedio. Suponga que la desviación estándar de la muestra es de $120 horas$. Con un nivel de significancia de $.05$, ¿podemos rechazar la afirmación del fabricante?

**Hipótesis Nula:** $H_0 = \mu \geq 10000 [horas]$
- Media muestral: $9900$
- Media poblacional: $10000$
- Desviación estandar de la muestra: $s = 120 horas$

> Suponga que el peso medio de los pingüinos rey encontrados en una colonia antártica el año pasado fue de $15.4 [kg]$. En una muestra de $35 \text{[pingüinos]}$ en la misma época de este año en la misma colonia, el peso medio de los pingüinos es de $14.6 [kg]$. Suponga que la desviación estándar de la muestra es de $2.5 [kg]$. Con un nivel de **significancia** de $0.05$, ¿podemos rechazar la hipótesis nula de que el peso medio de los pingüinos no difiere del del año pasado?

**Datos:**
- Media poblacional: $\mu = 15.4$
- Media muestral: $\bar{x} = 14.6$
- Tamaño muestra: $n = 35$
- Desviación estandar: $s = 2.5$
- Nivel de significancia: $\alpha = 0.05$

**Hipótesis Nula**: $H_0 := \mu = 15.4$, $H_a := \mu \neq 15.4$
Intervalo de confianza: $0.95$, al ser desigualdad se tienen dos colas. Que intersectan en:
$-t \frac{\alpha}{2}$ y $+t \frac{\alpha}{2}$. 

Para obtener los cortes de la cola:
$\iff -t_{0.025} = \text{qtest}(\frac{\alpha}{2}, n-1)$
$\iff -t_{0.025} = \text{qtest}(0.025, 34) = -2.03$
Entonces, los umbrales cortan en $\pm 2.03$

Ahora, para "desnormalizar", se utiliza la fórmula 
$$t = \frac{\bar{x} - \mu}{ \frac{s}{\sqrt{n}}}$$
$t = \frac{14.6 - 15.4}{\frac{2.5}{\sqrt{35}}} = -1.89$
Cómo vemos, trazando el gráfico por simetría, la correspondencia entre el -1.89 y el 14.6, al encontrarse dentro del intervalo de confianza, **no se puede rechazar la hipótesis nula.**

**Solución en R:**
![[Pasted image 20241203105013.png]]
```r
t = t.test(muestra, mu, nivel_confianza).
```

## Prueba de Contraste de Hipótesis
> Pruebas sobre la media de una distribución normal, se desconoce la varianza. Se pude usar tanto para distribuciones normales cómo no normales.

**Chi-Squared Test**
>[!WARNING]
>No es necesariamente simétrica, osease, no puede ser normal.
```r
x_alpha = qchisq(intervalo_confianza, (n-1))
```
### Ejercicio
> Se utiliza una máquina de llenado automática para llenar botellas con detergente líquido. Una muestra aleatoria de $20$ botellas da como resultado una varianza de la muestra del volumen en el llenado de $s2 = 0.0153 (onzas líquidas)$. Si la varianza del volumen de llenado excede $0.01 (onzas líquidas)$, una proporción inaceptable de botellas se llenará de manera insuficiente o excesiva. ¿Hay evidencia en los datos de la muestra que sugiera que el fabricante tiene un problema con las botellas? Use $\alpha = 0.05$ y suponga que el volumen de llenado tiene una distribución normal.

$$
H_0 := \sigma^2 \leq 0.01 
$$
$$
H_a := \sigma^2 > 0.01
$$
- Muestra = $n = 20$



Puede no ser normal, depende de los grados de confianza en el problema 
puede ser como una wea fea.
El parametro es la varianza, no la media, en este ejercicio.
Esta la varianza puesta en algún lugar.
La varianza poblacional, como estamos viendo la varianza, es nuestro parametro, lo que corta la linea es la representacion del 0.01.
El experimento es de una cola, con un grado de confianza de 95%, y el otro 5% a la izquierda, todo el umbral bajo el 95% muestra que no podemos rechazar la hipotesis nula.
Utilizamos la formula
$$
X^2_0 = ((n-1)S²)/o²_0 
$$
Considerando
$$
H_0: o^2 = o^2_0
$$

qchisq nos ayuda obteniendo la varianza cuadrada de cosas que no respetan la distribución normal
qchisq (1-alfa,df = n-1) // al parecer siempre se utiliza el n-1. Aca tambien entra en juego lo de que la muestra es mayor que 30 no?.
![[Pasted image 20241203111345.png]]

### Resumen de Pruebas Aproximadas a una Distribución Binomial de una cola
> Suponga que el $12\%$ de las manzanas cosechadas el año pasado en un huerto estaba podrida. Este año, $30$ de $214$ manzanas están podridas. Considerando un nivel de significancia de $.05$, ¿podemos rechazar la hipótesis nula de que la proporción de manzanas podridas en la cosecha permanece por debajo del $12\%$ este año?


## Bondad de Ajuste (Chi-Squared)
**Goodness-of-Fit Test Statistic**
$$
\chi^2 = \sum_{i=1}^{k} \frac{(O_i - E_i)^2}{E_i}
$$
> Permite predecir frecuencias, observados / esperados. La prueba permite estimar en varias dimensiones.

**Ejemplo:**
> Imaginemos que recolectamos rosas silvestres y encontramos que **81 eran rojas, 50 amarillas y 27 blancas**. Suponga que un artículo científico señala indica que en la región donde recopiló las flores, **la proporción de rosas rojas, amarillas y blancas es 3:2:1**. ¿Hay alguna diferencia significativa entre las proporciones observadas y las proporciones esperadas?

>[!WARNING]
>SI HACEMOS ALGO DE CARACTER NO PARAMÉTRICO Y CONCLUÍMOS CON MEDIA TENEMOS QUE TRAER LUBRICANTE A LA PRUEBA.
