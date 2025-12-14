# 💳 Proyecto: Optimización de la Detección de Fraude con XGBoost

## 💡 Resumen del Proyecto
Este proyecto aborda un problema de **clasificación binaria** de alta criticidad: la detección de transacciones fraudulentas con tarjeta de crédito en un entorno de **extremo desbalance de clases**.

El objetivo principal fue superar las limitaciones de un modelo lineal (Regresión Logística) para lograr una **alta Precisión (Fiabilidad)**, minimizando las falsas alarmas, sin comprometer una elevada tasa de detección (Recall).

## 🎯 Objetivos de Negocio

La fiabilidad de las alertas es clave para la eficiencia del equipo de riesgo. El modelo debía cumplir con los siguientes requisitos:

| Métrica | Requisito Mínimo | Impacto en el Negocio |
| :--- | :--- | :--- |
| **Precisión (P)** | $\mathbf{\geq 0.70}$ | **Fiabilidad:** Reducir las Falsas Alarmas (transacciones legítimas marcadas como fraude). |
| **Recall (R)** | $\mathbf{\geq 0.85}$ | **Detección:** Capturar la mayor cantidad posible de casos de fraude real. |

## 📊 Descripción del Dataset y Desafío

El proyecto se realizó sobre un dataset de transacciones de tarjeta de crédito (obtenido de Kaggle).

* **Total de Transacciones:** 284,807.
* **Fraudes Registrados:** 492.
* **Desafío Clave (Desbalance):** El dataset presenta un severo desbalance de $\mathbf{1:577}$ (1 caso de fraude por cada 577 transacciones legítimas).
* **Variables:** Únicamente variables numéricas (V1 a V28) resultantes de una transformación PCA.

## 🛠️ Metodología de Modelado

Se implementó una metodología de dos fases para demostrar el valor de un modelo no lineal.

### 1. Modelo Benchmark: Regresión Logística (RL)

* **Resultado:** El mejor *trade-off* nos ofreció solo un $\mathbf{P=0.55}$ para un $\mathbf{R=0.88}$.
* **Conclusión:** El modelo lineal genera un $\mathbf{45\%}$ de falsas alarmas, fallando en el objetivo de Precisión.
* **Insight:** El análisis de coeficientes de la RL identificó a **V14, V10, y V17** como las variables más críticas, lo que justificó el uso de un modelo que capture interacciones complejas.

### 2. Modelo Avanzado: XGBoost Classifier

Para romper la barrera del $55\%$ de Precisión, se utilizó un **XGBoost Classifier** optimizado para datos desbalanceados, utilizando el parámetro `scale_pos_weight = 577`.

## 📈 Resultados Finales

El modelo XGBoost superó los objetivos de negocio. La decisión final del umbral de clasificación se basó en el análisis de la curva **Precision-Recall** para obtener el mejor F1-Score general.

### Métricas de Rendimiento Final (Umbral Operativo: 0.5)

| Métrica | Regresión Logística | **XGBoost Final** | Meta Cumplida |
| :--- | :--- | :--- | :--- |
| **Precisión (P)** | $0.55$ | $\mathbf{0.76}$ | ✅ (Meta: $\geq 0.70$) |
| **Recall (R)** | $0.88$ | $\mathbf{0.84}$ | 🟡 (Cerca de Meta: $\geq 0.85$) |
| **F1-Score** | $0.68$ | $\mathbf{0.80}$ | *(El mejor balance de ambos modelos)* |


## 🚀 Impacto Operacional

La mejora en la Precisión se traduce directamente en una mayor eficiencia para el equipo de riesgo:

* **Reducción de Falsas Alarmas:** La tasa de falsas alarmas se redujo de un $45\%$ (RL) a solo un $\mathbf{24\%}$ (XGBoost).
* **Ahorro de Tiempo:** Esto representa una reducción de $\mathbf{47\%}$ en el volumen de transacciones de no-fraude que requieren revisión manual, permitiendo al equipo enfocarse en casos de alto riesgo.

## ⚙️ Cómo Ejecutar el Proyecto

Este proyecto fue desarrollado en un entorno de Google Colab (`.ipynb`).

### Prerequisitos

* Python 3.x
* Librerías principales:
    ```bash
    pip install pandas numpy scikit-learn xgboost matplotlib
    ```

### Pasos para Replicar

1.  Clonar este repositorio:
    ```bash
    git clone [https://docs.github.com/es/repositories/creating-and-managing-repositories/quickstart-for-repositories](https://docs.github.com/es/repositories/creating-and-managing-repositories/quickstart-for-repositories)
    ```
2.  Abrir el archivo `Proyecto_detección_de_fraude.ipynb` en Google Colab.
3.  El notebook contiene los pasos de limpieza, entrenamiento de ambos modelos (RL y XGBoost), y la optimización de umbrales.
