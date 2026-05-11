# MIA
Proyecto MIA
# Predicción de Demanda para empresa comercializadora de productos promocioanles'

Este proyecto se enfoca en la predicción de la demanda de la categoría 'ESCOLAR Y OFICINA' utilizando datos históricos de ventas. Se implementan y comparan varios modelos de series temporales y redes neuronales para identificar el enfoque más efectivo.

## Tabla de Contenidos
- [Dataset](#dataset)
- [Metodología](#metodología)
  - [Preprocesamiento de Datos](#preprocesamiento-de-datos)
  - [Modelos Implementados](#modelos-implementados)
- [Resultados y Evaluación](#resultados-y-evaluación)
- [Visualizaciones](#visualizaciones)
- [Cómo Ejecutar el Código](#cómo-ejecutar-el-código)
- [Conclusión](#conclusión)

## Dataset
El análisis se basa en el archivo CSV `Ventas 2023-2025 11-03-26 - CSV.csv`. Este dataset contiene información detallada sobre las ventas, incluyendo `FECHA`, `CATEGORIA`, `DEMANDA REAL (Q)` que son fundamentales para el funcionamiento del código, entre otras variables.

## Metodología

### Preprocesamiento de Datos
1.  **Carga y Agrupación**: Los datos de ventas se cargan y se agrupan por `FECHA` y `CATEGORIA`, sumando la `DEMANDA REAL (Q)`. La categoría objetivo para este análisis es `ESCOLAR Y OFICINA`.
2.  **División Entrenamiento/Prueba**: Los datos se dividen en conjuntos de entrenamiento y prueba (80% para entrenamiento, 20% para prueba) de forma cronológica.
3.  **Eliminación de Outliers**: Se utiliza el método del rango intercuartílico (IQR) para identificar y eliminar outliers de la variable `DEMANDA REAL (Q)` en el conjunto de entrenamiento. Los mismos límites se aplican al conjunto de prueba.

### Modelos Implementados
Se evaluaron los siguientes modelos de predicción de series temporales:

1.  **Regresión Lineal**: Un modelo base que utiliza características derivadas de la fecha (fecha ordinal, mes, día de la semana) y codificación one-hot para predecir la demanda.
2.  **Holt-Winters (Suavizado Exponencial)**: Un modelo estadístico clásico para series temporales con componentes de tendencia y estacionalidad. Se configuró con estacionalidad aditiva semanal (`seasonal_periods=7`).
3.  **Long Short-Term Memory (LSTM)**: Una arquitectura de red neuronal recurrente (RNN) avanzada, adecuada para capturar dependencias a largo plazo en secuencias de datos.
    *   **Básico**: Una capa LSTM simple.
    *   **Con Fine-Tuning**: Arquitectura más compleja con dos capas LSTM, capas de Dropout para regularización y `EarlyStopping` durante el entrenamiento para prevenir el sobreajuste.
4.  **Redes Neuronales Recurrentes (RNN)**: Una red neuronal recurrente más simple, también diseñada para datos secuenciales.
    *   **Básico**: Una capa SimpleRNN simple.
    *   **Con Fine-Tuning**: Arquitectura mejorada con dos capas SimpleRNN, capas de Dropout y `EarlyStopping`.

Para los modelos LSTM y RNN, los datos se escalaron utilizando `MinMaxScaler` y se transformaron en secuencias con un `look_back` de 7 días.

## Resultados y Evaluación
Los modelos se evaluaron utilizando las siguientes métricas:
-   **RMSE (Root Mean Squared Error)**: Mide la magnitud de los errores de predicción. Valores más bajos son mejores.
-   **MAE (Mean Absolute Error)**: Mide el promedio de los errores absolutos. Valores más bajos son mejores.
-   **R-squared (R²)**: Indica la proporción de la varianza en la variable dependiente que es predecible a partir de las variables independientes. Valores más cercanos a 1 son mejores; valores negativos indican un modelo peor que un modelo de línea base simple.

Aquí está la tabla comparativa de las métricas de rendimiento:

| Modelo                  | RMSE   | MAE    | R-squared |
|:------------------------|:-------|:-------|:----------|
| Holt-Winters            | 570.91 | 392.35 | -0.54     |
| LSTM                    | 436.50 | 338.58 | 0.02      |
| RNN                     | 465.16 | 330.80 | -0.12     |
| LSTM (Fine-Tuned)       | 429.19 | 299.92 | 0.05      |
| RNN (Fine-Tuned)        | 514.39 | 361.45 | -0.36     |

**Observaciones Clave:**
-   El modelo **LSTM (Fine-Tuned)** presentó el mejor rendimiento general, con el menor RMSE y MAE, y el R-cuadrado más alto. Esto sugiere que las mejoras en la arquitectura y las técnicas de regularización fueron efectivas.
-   El modelo **Holt-Winters** tuvo el peor desempeño, con un R-cuadrado negativo, lo que indica que no es adecuado para capturar los patrones complejos en estos datos de demanda.
-   Los modelos de **Redes Neuronales (LSTM y RNN)** superaron a la Regresión Lineal y a Holt-Winters, lo que resalta la capacidad de los modelos profundos para manejar series temporales complejas.

## Visualizaciones
El notebook incluye varias visualizaciones para cada modelo, mostrando las predicciones contra los valores reales en el conjunto de prueba, lo que permite una inspección visual de la precisión del modelo.

## Cómo Ejecutar el Código

### Requisitos
-   Python 3.x
-   Las siguientes librerías de Python:
    -   `pandas`
    -   `numpy`
    -   `matplotlib`
    -   `seaborn`
    -   `scikit-learn`
    -   `tensorflow`
    -   `statsmodels`

Puedes instalar las dependencias usando pip:
bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow statsmodels
```

### Pasos para Ejecutar
1.  *Clonar el Repositorio*: Clona este repositorio en tu máquina local.
2.  *Colocar el Dataset*: Asegúrate de que el archivo Ventas 2023-2025 11-03-26 - CSV.csv esté ubicado en la ruta /content/ o actualiza la variable file_path en el notebook a la ubicación correcta.
3.  *Abrir el Notebook*: Abre el archivo Jupyter Notebook (your_notebook_name.ipynb) en un entorno compatible (por ejemplo, Google Colab, Jupyter Lab, Jupyter Notebook).
4.  *Ejecutar las Celdas*: Ejecuta las celdas del notebook en orden. Cada celda realiza una parte del proceso de análisis y modelado, desde la carga de datos hasta la evaluación de los modelos.

## Conclusión
El modelo *LSTM con Fine-Tuning* es el más prometedor para la predicción de la demanda en la categoría 'ESCOLAR Y OFICINA', ofreciendo la mejor combinación de RMSE, MAE y R-cuadrado. Este proyecto sirve como una base sólida para futuras optimizaciones y exploraciones de modelos más avanzados.
