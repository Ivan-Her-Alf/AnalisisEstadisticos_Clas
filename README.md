# 🌦️ Weather Forecasting: Comparative Classification Pipeline

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Status](https://img.shields.io/badge/Status-Production%20Ready-green)

## 📋 Resumen Ejecutivo

Este proyecto implementa un pipeline de **Machine Learning de extremo a extremo** para predecir condiciones climáticas (Lluvia vs. No Lluvia). 

El objetivo principal no fue solo maximizar la precisión, sino **comparar la robustez** entre métodos estadísticos tradicionales (Regresión Logística con regularización Lasso/ElasticNet) y métodos de conjunto modernos (Random Forest).

La solución ingesta datos automáticamente vía API, selecciona las variables más influyentes mediante técnicas de reducción de dimensionalidad y despliega un modelo final con una precisión del **100% en el conjunto de prueba**, proporcionando reglas de negocio claras e interpretables.

## 🏗 Arquitectura de la Solución

El flujo de trabajo automatizado consta de cuatro fases:

1.  **Ingesta Automatizada:** Conexión directa a la API de Kaggle para garantizar la reproducibilidad y actualización de datos sin intervención manual.
2.  **Ingeniería de Características (Feature Engineering):**
    * Escalado de variables para modelos sensibles a la magnitud (Lasso/Ridge).
    * Codificación de variables categóricas.
3.  **Selección de Variables (Feature Selection):**
    * Implementación de **Stepwise Selection (RFE)** para identificar el subconjunto óptimo de predictores.
    * Uso de **Lasso (L1)** para penalizar y eliminar variables con bajo poder predictivo.
4.  **Modelado Comparativo:** Entrenamiento y validación cruzada de 6 algoritmos distintos.

## 📊 Análisis de Resultados

Se evaluaron múltiples enfoques para determinar el mejor equilibrio entre precisión y complejidad.

| Modelo | Accuracy (Precisión) | Observación |
| :--- | :--- | :--- |
| **Decision Tree** | **1.0000** | Mejor desempeño y alta interpretabilidad. |
| **Random Forest** | **0.9987** | Extremadamente robusto, pero "caja negra". |
| Stepwise (RFE) | 0.9280 | Buen balance con menos variables. |
| Elastic Net | 0.9280 | Manejo eficiente de colinealidad. |
| Logistic Regression | 0.9267 | Línea base sólida. |
| Lasso (L1) | 0.9267 | Útil para simplificación del modelo. |

### Visualización del Rendimiento
![Model Comparison](output/model_comparison.png)

> **Insight Técnico:** La diferencia de rendimiento entre los modelos lineales (~92%) y los basados en árboles (~100%) sugiere que la relación entre variables climáticas (Humedad, Temperatura) no es lineal, sino que sigue umbrales de decisión estrictos.

## 🧠 Interpretación de Negocio (White Box Model)

Para un rol de Business Intelligence, la **explicabilidad** es clave. A diferencia de las "Cajas Negras", el modelo de Árbol de Decisión nos permite generar reglas de negocio claras basadas en los datos.

Analizando el árbol generado por el modelo:

![Decision Tree](output/decision_tree_viz.png)

**Reglas Descubiertas:**
1.  **El factor crítico:** La variable raíz es la **Humedad**.
    * Si la Humedad es baja (`<= 0.288`), el modelo predice **"No Rain"** con pureza total.
2.  **Condiciones secundarias:**
    * Si la humedad es alta, el modelo evalúa la **Cobertura Nubosa (Cloud Cover)**.
    * Si hay muchas nubes pero la **Temperatura** es baja (`<= 0.318`), el modelo pronostica **Lluvia**.

Esto permite a los stakeholders confiar en el modelo entendiendo la lógica detrás de la predicción.

## 🛠 Tecnologías

* **Python:** Lenguaje principal.
* **Scikit-Learn:** Modelado (Logistic, Trees, Random Forest), Preprocesamiento y Métricas.
* **Seaborn/Matplotlib:** Visualización de comparativas.
* **Kaggle API:** Ingesta de datos programática.

## 🚀 Instalación y Uso

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/tu-usuario/weather-prediction-ml.git](https://github.com/tu-usuario/weather-prediction-ml.git)
    ```
2.  **Instalar dependencias:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Configurar Credenciales:**
    Asegúrate de tener tu archivo `kaggle.json` en la carpeta `.kaggle` de tu usuario.
4.  **Ejecutar el Pipeline:**
    ```bash
    python src/main.py
    ```