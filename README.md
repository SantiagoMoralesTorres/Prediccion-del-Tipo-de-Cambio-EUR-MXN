# Predicción del Tipo de Cambio EUR/MXN

## Objetivo
El objetivo de este proyecto fue ajustar un modelo GARCH a la serie de tiempo de los últimos 10 años del tipo de cambio Euro/Peso Mexicano (EUR/MXN) para modelar la volatilidad de la serie con la finalidad de construir 1000 escenarios distintos del comportamiento futuro de la divisa en los próximos 5 días. Adicionalmente, se utilizó la volatilidad móvil dada por el modelo GARCH para calcular el Value at Risk de los últimos 2 años.

## Metodología
* Se analizó la serie y se obtuvo la serie de rendimientos logarítmicos para estacionalizar la serie.
* Se graficaron los autocorrelogramas simples y parciales de la serie de rendimientos y rendimientos al cuadrado, así como el histograma correspondiente.
* Se trató de ajustar en primera instancia algún modelo de tipo SARIMA.
* Se detectaron efectos de tipo ARCH en la serie de los retornos.
* Se ajustaron distintos modelos GARCH a la serie considerando distintos parámetros p,q, el tipo de media y la distribución del componente aleatorio, seleccionado el mejor modelo mediante el criterio BIC.
* Se ajustó el mejor modelo GARCH y se verificaron los supuestos como coeficientes significativos, residuales normales y no autocorrelacionados ni con efectos GARCH.
* Se ajustó la volatilidad de la serie y se predijo la volatilidad de los siguientes 5 días.
* Se calculó el VaR, utilizando solo los primeros 8 años de la serie para entrenar el modelo, y los últimos 2 para la estimación del VaR y su comparativa con los rendimientos reales obtenidos.
* Con la volatilidad predicha de los siguientes 5 días, se simularon 1000 escenarios posibles de la serie de los rendimientos, y con esta serie, se obtuvieron 1000 escenarios de la serie de tiempo del tipo de cambio EUR/MXN.
* Finalmente, se obtuvó la probabilidad de que la serie superara cierto valor (21 MXN, 21.5 MXN, 22 MXN, 22.5 MXN y 23 MXN), contando el número de escenarios que superaron el valor establecido en cualquiera de los 5 días respecto a los 1000 escenarios simulados totales.

## Resultados
* Los rendimientos mostraban una distribución leptocúrtica, similar a una t-student, con un fuerte sesgo a la derecha principalmente por los incrementos que tuvo la serie en ciertos periodos como la pandemia por COVID-19.
* Se observó que los rendimientos no están autocorrelacionados entre sí, pero los rendimientos al cuadrado sí, mostrando que el conocer el rendimiento actual no es suficiente para estimar si el siguiente rendimiento será positivo o negativo (Mercados Eficientes), aunque si es posible tener una idea de su magnitud absoluta.
* Un modelo de tipo SARIMA no es adecuado dado que no hay autocorrelaciones significativas en la serie de rendimientos, y por tanto, los coeficientes del modelo son 0 (no hay ajuste).
* Los mejores modelos GARCH para la serie son aquellos con media 0 y distribución t sesgada. Se ajustó un GARCH(1,2), el cual cumplía con todos los supuestos (coeficientes significativos y residuales normales, no autocorrelacionados y sin efectos ARCH).
* El VaR logra capturar las pérdidas espontáneas que sufre la serie, pues al registrarse una pérdida inmediatamente el VaR se actualiza para capturar este cambio en la volatilidad. Con un umbral del 95%, se tiene una eficiencia del 96%, y con un umbral del 99% se tiene una eficiencia del 99%, pero este último umbral sobreestima el riesgo en muchas ocasiones.
* Con los escenarios finales simulados, se obtuvieron las siguientes estimaciones de la probabilidad de que la serie supere cierto precio.
  
| Precio          | Probabilidad de superar el precio en los siguientes 5 días |
|-----------------|------------------------------------------------------------|
| 21 MXN          | 98.8%                                                      |
| 21.5 MXN        | 83.3%                                                      |
| 22 MXN          | 30.3%                                                      |  
| 22.5 MXN        | 4.7%                                                       |
| 23 MXN          | 0.1%                                                       | 
