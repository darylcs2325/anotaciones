---
tags:
  - Python
  - Estadística
---

## Riesgo y Retorno Financiero
El **Riesgo financiero** es una medida de la incertidumbre de los rendimientos futuros, es decir, es la dispersión o varianza de los rendimientos.

### Rendimiento Discreto (RD)

Es el cambio porcentual en el precio de un activo en un periodo determinado

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{RD=\frac{P_{final} - P_{inicial}}{P_{inicial}}}
\label{eq:rd}
\end{equation}
$$

Si queremos calcular el **rendimiento total bimestral**, teniendo el rendimiento por cada mes:

* $P_1=100\%$
* $P_2=-50\%$

El primer pensamiento sería que el rendimiento total es la suma de los rendimientos de los periodos, si es así, entonces el rendimiento total sería

$$RD_{Total}=P_1+P_2=50\%$$

Si nuestro capital inicial es de $\$/.100.00$, entonces al final tendríamos $\$/. 150.00$.

Para comprobar, vamos a calcular periodo por periodo, sabemos que,

$$
    RD=\frac{P_{final} - P_{inicial}}{P_{inicial}} \rightarrow P_{final}=(RD+1)*P_{inicial}
$$

* Periodo 1: $P_{1}=(100\%+1)*100 = \$200.00$
* Periodo 2: $P_{2}=(-50\%+1)*200 = \$100.00$

Como vemos, tenemos nuestro capital final (periodo 2) que indica que es $\$100.00$. Pero nuestro capital inicial es de $\$100.00$, entonces el rendimiento final no es del $50\%$ como se calculó anteriormente, sino que es del $0\%$. Nos podemos dar cuenta que el **Rendimiento Discreto Total no es aditivo**. Esto se debe a que el precio, monto o capital base cambia por cada periodo, por lo que la suma no es el correcto, es un cálculo compuesto.

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{RD_{Total}=(1+RD_1)(1+RD_2)(...) - 1 \label{eq:rd-total}}
\end{equation}
$$

Calculando el Rendimiento Discreto Total $\eqref{eq:rd-total}$, mediante esta última fórmula:

$$RD_{Total}=(1+100\%)(1-50\%) - 1 = 0\%$$

### Rendimiento Logarítmico (RL)

Es el logaritmo natural del cambio en el precio de un activo

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{RL=\ln(\frac{P_{final}}{P_{inicial}})}
\label{eq:rl}
\end{equation}
$$

Supongamos que el Rendimiento Total es mediante la suma de los rendimientos de sus periodos, entonces

$$RL_{Total}=\ln(\frac{P_{final}}{P_{intermedio}}) + \ln(\frac{P_{intermedio}}{P_{inicial}})$$

$$RL_{Total}=\ln(\frac{P_{final}}{\cancel{P_{intermedio}}}*\frac{\cancel{P_{intermedio}}}{P_{inicial}})$$

$$\therefore RL_{Total}=\ln(\frac{P_{final}}{P_{inicial}})$$

Como vemos, sí tiene sentido, en este caso no está el problema con los precios, montos, capitales intermedios ya que esto se eliminan al ser sumados para obtener el **Rendimiento Total**.

Tomando los valores del ejemplo anterior:

$$RL_{Total}=\ln(\frac{200}{100}) + \ln(\frac{100}{200}) = 0.6931 - 0.6931 = 0\%$$

Entonces, sumando los rendimientos de los periodos nos arroja un resultado del $0\%$ como **Rendimiento Logaritmico Total**.

### Datos OHCLV

Este tipo de datos se presenta comúnmente en el campo de finanzas, específicamente en bolsa de valores, criptomonedas, etc. Estas siglas representan a **Open, High, Close, Low, Volume**:

* **Open**: Es el precio de apertura de un periodo determinado (como por año, semana, día, 15min, 1min, etc.)
* **High**: Es el precio más alto alcanzado durante el periodo.
* **Low**: Es el precio más bajo que se alcanza durante el periodo.
* **Close**: Es el precio cuando se terminó el periodo.
* **Volume**: Es el volumen de transacciones, número total de unidades del activo que se negociaron durante el periodo seleccionado. Representa cuantas acciones, monedas o contratos cambiaron de mano durante el periodo.

Gráficamente se utiliza en las **Velas Japonesas**, ya que estas contienen estos cincos términos

<figure markdown="span">
  ![Image title](../../images/finanzas-1-gestion-riesgo-portafolio/velaJaponesa.png){ width=70% }
  <figcaption id="frontera_eficiencia">Vela Japonesa</figcaption>
</figure>

!!! info "Adjusted Close"
    El **precio de cierre ajustado** es el que se obtiene luego de realizar ajustes por:

    * Dividendos
    * Divisiones de acciones (Splits): Ocurre cuando el precio de una acción es muy alta y la empresa desea dividir para que sea más accesible a inversores minoristas.
    * Otros eventos corporativos
    
    Este precio es con el que se debe realizar el análisis financiero y cálculo de retornos

!!! info "Volumen"
    La importancia del volumen radica en la **Fuerza del movimiento del precio**. Un precio que fluctua con alto volumen es más significativo que uno que cambia con bajo volumen.


### Ejemplos

Se estará trabajando con los siguientes datos:

|       Date |   Open |   High |    Low |    Close |   Volume |  Adjusted |
|------------|--------|--------|--------|----------|----------|-----------|
| 2000-01-03 | 88.777 | 89.722 | 84.712 | 58.28125 | 53228400 | 38.527809 |
| 2000-01-04 | 85.893 | 88.588 | 84.901 | 56.31250 | 54119000 | 37.226345 |
| 2000-01-05 | 84.050 | 88.021 | 82.726 | 56.90625 | 64059600 | 37.618851 |
| 2000-01-06 | 84.853 | 86.130 | 81.970 | 55.00000 | 54976600 | 36.358688 |
| 2000-01-07 | 82.159 | 84.901 | 81.166 | 55.71875 | 62013600 | 36.833828 |


```py hl_lines="1-2 4-5 7-8" title="Rendimiento Discreto y Logaritmico Acumulado"

df = df.set_index("Date")
df = df.sort_values("Date") #(1)!

df["RC"] = df["Adjusted"].pct_change()
df["RC_Acumulativo"] = (1 + df["RC"]).cumprod() - 1 #(2)!

df["RL"] = np.log(df["Adjusted"]/df["Adjusted"].shift(1)) 
df["RL_Acumulativo"] = df["RL"].cumsum() #(3)!
```

1. Estos métodos son importantes de aplicar desde un inicio cuando se quiere calcular los Retornos, colocar como índice a la columna `Date` y ordenarlo de forma ascendente respecto al índice.
2. 
    * `#!py .pct_change()`: Se utiliza para calcular el rendimiento discreto, en fracción no en %.
    * `#!py .cumprod()` Se utiliza para calcular la suma acumulativa del producto del término que está a la izquierda.
3. 
    * `#!py .shift(1)`: Es el logarítmo natural del precio actual y el precio anterior.
    * `#!py .cumsum()`: Permite calcular la suma acumulativa.

|            |  Adjusted |        RC | RC_Acumulativo |        RL | RL_Acumulativo |
|------------|-----------|-----------|----------------|-----------|----------------|
| 2000-01-03 | 38.527809 |       NaN |            NaN |       NaN |            NaN |
| 2000-01-04 | 37.226345 | -0.033780 |      -0.033780 | -0.034364 |      -0.034364 |
| 2000-01-05 | 37.618851 |  0.010544 |      -0.023592 |  0.010489 |      -0.023875 |
| 2000-01-06 | 36.358688 | -0.033498 |      -0.056300 | -0.034072 |      -0.057947 |
| 2000-01-07 | 36.833828 |  0.013068 |      -0.043968 |  0.012983 |      -0.044964 |
| ...        |       ... |       ... |            ... |       ... |            ... |
| 2018-02-12 | 88.713272 |  0.010773 |       1.302578 |  0.010716 |       0.834029 |
| 2018-02-13 | 89.410004 |  0.007854 |       1.320662 |  0.007823 |       0.841852 |
| 2018-02-14 | 90.809998 |  0.015658 |       1.356999 |  0.015537 |       0.857389 |
| 2018-02-15 | 92.660004 |  0.020372 |       1.405016 |  0.020168 |       0.877557 |
| 2018-02-16 | 92.000000 | -0.007123 |       1.387886 | -0.007148 |       0.870408 |

---

## Distribución

Para poder entender la distribución es importante saber 4 conceptos.

* **Media** ($\mu$): Es el promedio de los datos
* **Varianza** ($\sigma^2$): La variabilidad de los datos ($\sigma$ es la volatilidad)
* **Asimetría** (Skewness): Es el sesgo que experimenta la distribución
* **Kurtosis**: Es la relación entre la altitud y la amplitud de la distribución, ayuda a entendersi los datos son más o menos propensos a generar valores extremos.

Teniendo en cuenta que la **Distribución Normal Estándar** tiene las siguientes características:

$$\mu=0, \sigma^2=1, \text{skewness}=0, \text{kurtosis}=3 (\text{exceso de kurtosis}=0)$$

### Media y Varianza

La importancia de la media radica en poder tener un solo valor que indica la estimación de todo el conjunto de datos, así poder realizar comparativas con otros activos.

Calculando el **retorno promedio diario** podemos **escalar** para hallar el **retorno promedio anual**.

$$RD_{anual}=(\prod_{t=1}^{252}(1+RD_t))-1$$

$$RL_{anual}=\sum_{t=1}^{252}\log{\frac{P_t}{P_{t-1}}}$$

De los datos históricos, se puede calcular el rendimiento diario promedio, suponiendo que estos rendimientos promedio se da durante un año en condiciones similares.

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{
    RD_{anual}=(1+RD_{prom})^{252}-1
    }
\label{eq:rd_anual}
\end{equation}
$$

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{
    RL_{anual}=252*RL_{prom}
    }
\label{eq:rl_anual}
\end{equation}
$$

$$
\begin{equation}
\bbox[3pt, border: 1pt solid #00adc3]{
    \sigma_{anual}=\sigma_{diaria}*\sqrt{252}
    }
\label{eq:varianza_anual}
\end{equation}
$$


!!! success "Escalar anualmente"
    Estos cálculos no son para hacer una predicción para saber el retorno que se tendrá en un nuevo año, difícilmente se cumpla esto debido a otros los factores. Escalar anualmente respecto al promedio diario de los datos históricos, nos permite poder comprarar el rendimiento un activo con otros.

```py title="Retorno Discreto y Logarítmica anual"
# Retono Discreto promedio diario y anual
RD_prom = np.mean(df["RC"])
RD_anual = (1+RD_prom)**252-1

# Retorno Logaritmico promedio diario y anual
RL_prom = np.mean(df["RL"])
RL_anual = 252*RL_prom

# Desviación estándar (volatilidad) diaria y anual
sigma_diaria = np.std(df["RC"])
sigma_anual = sigma_diaria*np.sqrt(252)

# Varianza diaria y anual
varianza = sigma_diaria**2
sigma_anual = varianza**2
```

|      |RD      | RL    |Desv. Estándar|Varianza|
|------|--------|-------|--------------|--------|
|Diaria|0.00037 |0.00019|0.01934       |0.00037 |
|Anual |0.09985 |0.04810|0.30703       |0.09426 |


En finanzas que una distribución tenga un sesgo positivo es una buena señal, ya que indicaría una mayor probabilidad de rendimientos altos.

### Skewness (Asimetría)

Una $\text{asimetría} > 0$, indica una falta de normalización, y que estás sesgado a la derecha. Es muy importante saber si la distribución de los datos no está normalizado ya que cuando se quiera hacer un modelo de predicción, la normalización puede ser un requisito.

Una $\text{asimetría} < 0$, indica falta de normalización y que está sesgado a la izquierda, es decir, hay mayor probabilidad de un rendimiento negativo de alto impacto.

### Kurtosis

El kurtosis mide la forma de las colas, indica qué tan propensa es una distribución a generar eventos extremos. En finanzas, un alto exceso de kurtosis indica un alto riesgo de eventos extremos (grandes pérdidas o ganancias).

Si se utiliza la biblioteca `scipy` se debe tener en cuenta que no se calculo el kurtosis en sí, sino que se calcula el exceso de kurosis. Se sabe que en una distribución normal el **kurtosis = 3**, un valor más alto o más bajo, se le denomina **exceso de kurtosis**. Por lo tanto, en `scipy` si el exceso de kurtosis es igual 0, estaríamos hablando posiblemente de una distribución normal (esto es porque resta 0 al kurtosis para hallar el exceso de kurtosis), si en caso queremos hallar el kurtosis simple, debemos sumar +3 al exceso arrojado por `scipy`.

Es muy importante que no haya valores nulos a la hora de calcular la asimetría y el kurtosis.

```py title="Exceso de Kurtosis"
from scipy.stats import skew, kurtosis

df_clean = df["RC"].dropna()

print("asimetría: ",skew(df_clean))

exceso_kurtosis=kurtosis(df_clean)
print("Exceso de kurtosis: ",exceso_kurtosis)
print("Kurtosis: ", exceso_kurtosis+3)

```

|     Medida       |Valor    |
|------------------|---------|
|asimetría         |0.21935  |
|Exceso de kurtosis|10.31457 |
|kurtosis          |13.31457 |

Hasta ahora hemos visto 4 características que nos ayuda a tener una idea acerca de la distribución de nuestros datos, sin embargo, estos no son suficientes para estar seguros que tenemos una distribución normal (más cuando el skewness y kurtosis son aproximaciones). Lo que nos podría ayudar a estar seguro de tener una distribución normal son:

* Histogramas y gráficos de densidad
* QQPlot (Quantile-Quantile Plot)
* Pruebas de normalidad
    * Shapiro-Wilk test
    * Kolmogorov-Smirnov test
    * Anderson-Darling test
    * Jarque-Bera test

El uso de las **histogramas más pruebas estadísticas** es buena para tener una confirmación robusta, también está la opción de **QQPlot** o **Jarque-Bera test** para conjuntos de datos grandes.