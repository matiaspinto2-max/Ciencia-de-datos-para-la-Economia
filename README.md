[README.md](https://github.com/user-attachments/files/30171186/README.md)
# 📈 Predicción del Precio de Bitcoin con Machine Learning

## Réplica y Extensión del Paper de Chen (2023)

**Asignatura:** Ciencia de Datos para la Economía
**Profesor:** Luis Cuevas Parra
**Año:** 2026

---

## 📄 Paper Seleccionado

> Chen, J. (2023). *Analysis of Bitcoin Price Prediction Using Machine Learning*. Journal of Risk and Financial Management, 16(1), 51.
> DOI: [10.3390/jrfm16010051](https://doi.org/10.3390/jrfm16010051)
> Revista: JRFM — Q2 (Economics and Econometrics / Finance, SCImago 2024)

## 🎯 Objetivo

Replicar los modelos de Random Forest Regression y LSTM propuestos por Chen (2023) para la predicción del precio diario de cierre de Bitcoin, y proponer un modelo adicional (Gradient Boosting) para enriquecer la comparación.

## 🗂️ Estructura del Proyecto

```
.
├── Bitcoin_Chen_Replica_Portafolio.ipynb   # Cuaderno principal
├── requirements.txt                         # Dependencias
└── README.md                                # Este archivo
```

## 🚀 Instrucciones de Ejecución

### 1. Clonar el repositorio
```bash
git clone https://github.com/matiaspinto2-max/Ciencia-de-datos-para-la-Economia.git
cd Ciencia-de-datos-para-la-Economia
```

### 2. Crear entorno virtual (recomendado)
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
```

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
```

### 4. Ejecutar el cuaderno
```bash
jupyter lab Bitcoin_Chen_Replica_Portafolio.ipynb
```

**Nota:** El propio cuaderno descarga todos los datos automáticamente desde Yahoo Finance (`yfinance`) al ejecutarse — no requiere archivos externos ni claves de API, solo conexión a internet.

## 📊 Modelos Implementados

| # | Modelo | Tipo | Origen |
|---|--------|------|--------|
| 1 | Random Forest Regression | Ensemble (Bagging) | Paper |
| 2 | LSTM (red apilada, 4 capas) | Deep Learning (RNN) | Paper |
| 3 | Gradient Boosting Regressor | Ensemble (Boosting secuencial, `loss='huber'`) | Propuesto por el grupo |

**Por qué Gradient Boosting:** a diferencia de Random Forest (bagging de árboles independientes) y de LSTM (dependencia secuencial temporal), Gradient Boosting corrige de forma secuencial el error del árbol anterior, lo que suele capturar mejor relaciones no lineales residuales en series financieras ruidosas. Se usa `loss='huber'` para dar robustez frente a los outliers de retorno diario identificados en el análisis exploratorio.

## 📈 Datos Utilizados

Todos los datos se obtienen vía `yfinance` (Yahoo Finance), replicando el tipo de variables de mercado relacionadas que usa el paper original:

- **Bitcoin (BTC-USD):** Open, High, Low, Close, Volume
- **Otras criptomonedas:** Ethereum, Litecoin, XRP, Dash, Dogecoin
- **Commodities:** Oro, Plata, Cobre, Petróleo (WTI)
- **Renta fija:** Tasa del Tesoro de EE.UU. a 10 años
- **Índices bursátiles:** S&P 500, Dow Jones, NASDAQ, Nikkei 225 (JP225), CSI 300
- **Tipos de cambio:** DXY, EUR, GBP, JPY, CAD, AUD, SGD, CNY, RUB

**Periodo:** 2015-03-31 a 2022-04-01, igual al de la muestra original de Chen (2023).

**División temporal:** dos sub-periodos de entrenamiento/prueba (no una partición aleatoria), respetando el orden cronológico para evitar fuga de información desde el futuro:
- **Periodo 1:** train 2015-03-31 a 2018-03-31 · test 2018-04-01 a 2018-09-30
- **Periodo 2:** train 2018-10-01 a 2021-09-30 · test 2021-10-01 a 2022-04-01

## 🔬 Métricas de Evaluación

- **RMSE** (Root Mean Squared Error)
- **MAPE** (Mean Absolute Percentage Error)
- **DA** (Directional Accuracy — % de aciertos en la dirección del cambio de precio)
- **Test de Diebold-Mariano** (significancia de la diferencia de desempeño entre pares de modelos)
- **Test de Clark-West** (cada modelo vs. benchmark ingenuo de caminata aleatoria)

## 📌 Resultados principales

| Periodo | Modelo | RMSE | MAPE (%) | DA (%) |
|---|---|---|---|---|
| 1 | Random Forest | 348.3 | 3.84 | 52.5 |
| 1 | LSTM | 2463.7 | 33.25 | 53.6 |
| 1 | Gradient Boosting | 352.8 | 3.82 | 54.1 |
| 2 | Random Forest | 2472.8 | 3.76 | 52.2 |
| 2 | LSTM | 18785.6 | 30.44 | 50.5 |
| 2 | Gradient Boosting | 2941.0 | 4.15 | 47.8 |

**Random Forest** obtiene el mejor desempeño global (MAPE más bajo o empatado en ambos periodos) y supera a LSTM de forma estadísticamente significativa (Diebold-Mariano, p < 0.001 en ambos periodos). Sin embargo, el test de Clark-West muestra que **ningún modelo supera de forma significativa** al pronóstico ingenuo de caminata aleatoria (todos los p-valores > 0.10), lo que matiza la utilidad predictiva real de los tres modelos. El detalle completo de la discusión está en la Sección 9 del cuaderno.

## 📚 Referencias

- Chen, J. (2023). Analysis of Bitcoin Price Prediction Using Machine Learning. *JRFM*, 16(1), 51.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5-32.
- Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. *Neural Computation*, 9(8), 1735-1780.
- Friedman, J. H. (2001). Greedy Function Approximation: A Gradient Boosting Machine. *Annals of Statistics*, 29(5), 1189-1232.
- Diebold, F. X., & Mariano, R. S. (1995). Comparing Predictive Accuracy. *Journal of Business & Economic Statistics*, 13(3), 253-263.
- Clark, T. E., & West, K. D. (2007). Approximately Normal Tests for Equal Predictive Accuracy in Nested Models. *Journal of Econometrics*, 138(1), 291-311.

---

## ⚠️ Notas y limitaciones

- Los resultados de LSTM pueden variar levemente entre ejecuciones debido a la inicialización aleatoria de pesos.
- El paper original incorpora variables on-chain (hash rate, dificultad de minería) y de atención pública (Google Trends) no disponibles vía `yfinance`; esta réplica se limita a variables de precios de mercado.
- La red LSTM se entrena con menos épocas y sin búsqueda exhaustiva de hiperparámetros, por restricciones de tiempo/cómputo.
- Los datos se descargan en tiempo real desde Yahoo Finance; pequeñas diferencias frente a la fuente original de los autores (ajustes de precios, gaps de fin de semana en cripto vs. mercados tradicionales) pueden introducir variaciones menores respecto a la réplica exacta.
