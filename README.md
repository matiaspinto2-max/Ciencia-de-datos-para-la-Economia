[README.md](https://github.com/user-attachments/files/30171212/README.1.md)
# Predicción del Precio de Bitcoin con Machine Learning

## Réplica y extensión del paper de Chen (2023)

**Asignatura:** Ciencia de Datos para la Economía
**Profesor:** Luis Cuevas Parra
**Año:** 2026

---

## Paper seleccionado

Chen, J. (2023). *Analysis of Bitcoin Price Prediction Using Machine Learning*. Journal of Risk and Financial Management, 16(1), 51. DOI: [10.3390/jrfm16010051](https://doi.org/10.3390/jrfm16010051)

Revista: JRFM — Q2 (Economics and Econometrics / Finance, SCImago 2024)

## Objetivo

Replicar los modelos de Random Forest Regression y LSTM que usa Chen (2023) para predecir el precio diario de cierre de Bitcoin, y agregar un modelo adicional (Gradient Boosting) para comparar el desempeño.

## Estructura del proyecto

```
.
├── Bitcoin_Chen_Replica_Portafolio.ipynb   # Cuaderno principal
├── requirements.txt                         # Dependencias
└── README.md
```

## Cómo ejecutar el cuaderno

1. Clonar el repositorio:
```bash
git clone https://github.com/matiaspinto2-max/Ciencia-de-datos-para-la-Economia.git
cd Ciencia-de-datos-para-la-Economia
```

2. Instalar las dependencias:
```bash
pip install -r requirements.txt
```

3. Abrir y ejecutar el cuaderno:
```bash
jupyter lab Bitcoin_Chen_Replica_Portafolio.ipynb
```

El cuaderno descarga los datos automáticamente desde Yahoo Finance (`yfinance`) al ejecutarse, así que solo se necesita conexión a internet, no hay archivos externos que agregar.

## Modelos implementados

| Modelo | Tipo | Origen |
|---|---|---|
| Random Forest Regression | Ensemble (bagging) | Paper |
| LSTM (red apilada, 4 capas) | Deep learning (RNN) | Paper |
| Gradient Boosting Regressor | Ensemble (boosting secuencial) | Propuesto por el grupo |

Elegimos Gradient Boosting como modelo adicional porque, a diferencia de Random Forest (que promedia árboles independientes) y de LSTM (que modela dependencia temporal), construye los árboles de forma secuencial corrigiendo el error del anterior, lo que suele funcionar bien en series financieras ruidosas. Se usó `loss='huber'` para que sea más robusto a los outliers de retorno diario que se ven en el análisis exploratorio.

## Datos utilizados

Todo se descarga con `yfinance`, siguiendo el mismo tipo de variables de mercado que usa el paper original:

- Bitcoin (Open, High, Low, Close, Volume)
- Otras criptomonedas: Ethereum, Litecoin, XRP, Dash, Dogecoin
- Commodities: oro, plata, cobre, petróleo WTI
- Tasa del Tesoro de EE.UU. a 10 años
- Índices bursátiles: S&P 500, Dow Jones, NASDAQ, Nikkei 225, CSI 300
- Tipos de cambio: DXY, EUR, GBP, JPY, CAD, AUD, SGD, CNY, RUB

**Periodo:** 2015-03-31 a 2022-04-01 (el mismo del paper original).

**División temporal:** se usan dos sub-periodos de entrenamiento/prueba, en orden cronológico, para no filtrar información del futuro:

- Periodo 1: entrenamiento 2015-03-31 a 2018-03-31, prueba 2018-04-01 a 2018-09-30
- Periodo 2: entrenamiento 2018-10-01 a 2021-09-30, prueba 2021-10-01 a 2022-04-01

## Métricas de evaluación

- RMSE (Root Mean Squared Error)
- MAPE (Mean Absolute Percentage Error)
- DA (Directional Accuracy, % de aciertos en la dirección del cambio de precio)
- Test de Diebold-Mariano (diferencia de desempeño entre pares de modelos)
- Test de Clark-West (cada modelo contra un benchmark ingenuo de caminata aleatoria)

## Resultados principales

| Periodo | Modelo | RMSE | MAPE (%) | DA (%) |
|---|---|---|---|---|
| 1 | Random Forest | 348.3 | 3.84 | 52.5 |
| 1 | LSTM | 2463.7 | 33.25 | 53.6 |
| 1 | Gradient Boosting | 352.8 | 3.82 | 54.1 |
| 2 | Random Forest | 2472.8 | 3.76 | 52.2 |
| 2 | LSTM | 18785.6 | 30.44 | 50.5 |
| 2 | Gradient Boosting | 2941.0 | 4.15 | 47.8 |

Random Forest es el modelo con mejor desempeño global (MAPE más bajo o empatado en ambos periodos) y supera a LSTM de forma estadísticamente significativa (Diebold-Mariano, p < 0.001 en ambos periodos). Sin embargo, el test de Clark-West muestra que ningún modelo le gana de forma significativa a un pronóstico ingenuo de caminata aleatoria (todos los p-valores superan 0.10), lo que matiza qué tan útil es realmente cada modelo en la práctica. La discusión completa está en la Sección 9 del cuaderno.

## Referencias

- Chen, J. (2023). Analysis of Bitcoin Price Prediction Using Machine Learning. *JRFM*, 16(1), 51.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5-32.
- Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. *Neural Computation*, 9(8), 1735-1780.
- Friedman, J. H. (2001). Greedy Function Approximation: A Gradient Boosting Machine. *Annals of Statistics*, 29(5), 1189-1232.
- Diebold, F. X., & Mariano, R. S. (1995). Comparing Predictive Accuracy. *Journal of Business & Economic Statistics*, 13(3), 253-263.
- Clark, T. E., & West, K. D. (2007). Approximately Normal Tests for Equal Predictive Accuracy in Nested Models. *Journal of Econometrics*, 138(1), 291-311.

## Notas y limitaciones

- Los resultados de LSTM pueden variar un poco entre ejecuciones por la inicialización aleatoria de pesos.
- El paper original usa variables on-chain (hash rate, dificultad de minería) y de atención pública (Google Trends) que no están disponibles en `yfinance`; esta réplica se limita a variables de precios de mercado.
- La LSTM se entrenó con menos épocas y sin una búsqueda extensa de hiperparámetros, por tiempo y capacidad de cómputo.
- Los datos se descargan en tiempo real desde Yahoo Finance, así que puede haber pequeñas diferencias frente a la fuente original de los autores (ajustes de precios, gaps de fin de semana en cripto vs. mercados tradicionales).
