[README.md](https://github.com/user-attachments/files/30171158/README.md)
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

Replicar los modelos de Random Forest Regression y LSTM propuestos por Chen (2023) para la predicción del precio diario de Bitcoin, y proponer un modelo adicional (XGBoost) para comparar el desempeño.

## 🗂️ Estructura del Proyecto

```
.
├── Bitcoin_Price_Prediction_ML.ipynb   # Cuaderno principal
├── requirements.txt                     # Dependencias
├── README.md                           # Este archivo
└── data/                               # (Generado automáticamente por el notebook)
```

## 🚀 Instrucciones de Ejecución

### 1. Clonar el repositorio
```bash
git clone https://github.com/[usuario]/bitcoin-price-prediction-ml.git
cd bitcoin-price-prediction-ml
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
jupyter lab Bitcoin_Price_Prediction_ML.ipynb
```

**Nota:** El cuaderno descarga los datos automáticamente desde Yahoo Finance y FRED al ejecutarse. Se requiere conexión a internet.

## 📊 Modelos Implementados

| # | Modelo | Tipo | Origen |
|---|--------|------|--------|
| 1 | Random Forest Regression | Ensemble (Bagging) | Paper |
| 2 | LSTM | Deep Learning (RNN) | Paper |
| 3 | XGBoost | Ensemble (Boosting) | Propuesto |

## 📈 Datos Utilizados

- **Bitcoin (BTC-USD):** Precio de cierre diario
- **Ethereum (ETH-USD):** Precio de cierre diario
- **S&P 500 (^GSPC):** Índice bursátil
- **Oro (GC=F):** Precio del oro
- **Petróleo WTI (CL=F):** Precio del crudo
- **Índice del Dólar (DX-Y.NYB):** Fortaleza del USD
- **Tasas de interés:** Federal Funds Rate y Treasury 10Y (FRED)

**Período:** 2018-01-01 a 2023-12-31

## 🔬 Métricas de Evaluación

- RMSE (Root Mean Squared Error)
- MAE (Mean Absolute Error)
- R² (Coeficiente de Determinación)
- Test de Diebold-Mariano (comparación estadística)

## 📚 Referencias

- Chen, J. (2023). Analysis of Bitcoin Price Prediction Using Machine Learning. *JRFM*, 16(1), 51.
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5-32.
- Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. *Neural Computation*, 9(8), 1735-1780.
- Chen, T., & Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *KDD '16*.

---

## ⚠️ Notas

- Los resultados de LSTM pueden variar entre ejecuciones debido a la inicialización aleatoria de pesos.
- Se recomienda GPU para un entrenamiento más rápido del LSTM.
- Los datos se descargan en tiempo real; los resultados dependen del período disponible en las APIs.
