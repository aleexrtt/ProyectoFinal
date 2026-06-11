# Proyecto Final - Modelación del Aprendizaje con Inteligencia Artificial
#Equipo: 
Diego Zermeño Viramontes | A01611907
Jesús Higuera Alanís | A01708955
Luis Alejandro Ruiz Trejo | A01613115
Ximena González González | A01708366

## Predicción de precios financieros con Machine Learning

Este repositorio contiene el proyecto final de la materia **Modelación del Aprendizaje con Inteligencia Artificial**. El objetivo del proyecto es evaluar y comparar modelos de aprendizaje automático para predecir el precio de cierre ajustado (`Adj_Close`) del siguiente día usando datos financieros históricos obtenidos desde Yahoo Finance.

El proyecto se desarrolla en un notebook de Python y compara un modelo base ingenuo contra modelos supervisados de regresión.

---

## Objetivo del proyecto

Predecir el valor de `Adj_Close` del día siguiente a partir de variables financieras del día actual. Las variables que se utilizaron fueron precio de apertura, máximo, mínimo, cierre ajustado y volumen.

La variable objetivo se construyó desplazando la columna `Adj_Close` un día hacia adelante, de forma que el modelo aprenda a estimar el precio ajustado del siguiente periodo.

---

## Contenido del repositorio

```text
.
├── Proyecto de la clase de modelación del aprendizaje con inteligencia artificial.ipynb
├── 'Declaración de Uso de Inteligencia Artificial Generativa.pdf'
├── yahoo_data.xlsx
├── requirements.txt
└── README.md
```

### Descripción de archivos

- `Proyecto de la clase de modelación del aprendizaje con inteligencia artificial.ipynb`: notebook principal del proyecto. Carga de datos, limpieza, Variable objetivo, separación entrenamiento/prueba, entrenamiento de modelos, evaluación y visualizaciones.
- `yahoo_data.xlsx`: base de datos financiera utilizada para ejecutar los experimentos.
- `requirements.txt`: dependencias necesarias para ejecutar el proyecto.
- `README.md`: instrucciones para realizar la predicción.
- `Declaración de Uso de Inteligencia Artificial Generativa.pdf`: declaración de uso de IA generativa

---

## Dependencias requeridas

El proyecto requiere Python 3.10 o superior y las siguientes librerías principales:

```txt
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
scikit-learn>=1.3.0
openpyxl>=3.1.0
jupyter>=1.0.0
notebook>=7.0.0
```

### Uso de cada dependencia

- `pandas`: carga, limpieza y manipulación del dataset.
- `numpy`: cálculos numéricos
- `matplotlib`: generación de gráficas y visualizaciones.
- `scikit-learn`: entrenamiento y evaluación de modelos de Machine Learning.
- `jupyter` / `notebook`: ejecución del notebook del proyecto.

---

## Modelos implementados

El proyecto compara los siguientes enfoques:

1. **Baseline ingenuo**
   - Supone que el precio ajustado de mañana será igual al precio ajustado de hoy.
   - Sirve como punto mínimo de comparación para validar si los modelos realmente dan valor predictivo.

2. **Decision Tree Regressor**
   - Modelo supervisado basado en árboles de decisión.
   - Se usa con profundidad para reducir el riesgo de sobreajuste.

3. **Random Forest Regressor**
   - Modelo de ensamble basado en múltiples árboles de decisión.
   - Permite mejorar la estabilidad de predicciones frente a un árbol individual.



---

## Variables utilizadas

### Variables predictoras

```python
features = ["Open", "High", "Low", "Close", "Volume"]
```

Estas variables representan información disponible del día actual.

### Variable objetivo

```python
df["Target"] = df["Adj_Close"].shift(-1)
```

La variable `Target` representa el precio de cierre ajustado del siguiente día.

---

## ¿Cómo replicarlo?

La predicción sigue esta secuencia:

1. Carga del archivo `yahoo_data.xlsx`.
2. Renombrado de columnas para evitar caracteres especiales.
3. Conversión de la columna `Date` a formato fecha.
4. Ordenamiento cronológico de los registros.
5. Revisión general del dataset.
6. Creación de la variable objetivo `Target`.
7. Selección de variables predictoras.
8. División cronológica de datos:
   - 80% entrenamiento.
   - 20% prueba.
9. Entrenamiento de modelos:
   - Baseline ingenuo.
   - Decision Tree Regressor.
   - Random Forest Regressor.
10. Evaluación con métricas de regresión.
11. Comparación de resultados.
12. Visualización de predicciones contra valores reales.

---

## Métricas de evaluación

Los modelos se evalúan usando:

- **MAE**: error absoluto medio.
- **MSE**: error cuadrático medio.
- **RMSE**: raíz del error cuadrático medio.
- **R²**: proporción de variabilidad explicada por el modelo.

Estas métricas nos ayudan a comparar tanto el tamaño promedio del error como la capacidad general ante nuevos datos.

---

## Serie Temporal Financiera

Como se trabaja con una serie temporal financiera, la división de datos se realiza sin `shuffle`. Esto es importante porque mezclar los datos rompería el orden cronológico. 

La separación usada es:

```python
split_index = int(len(X) * 0.80)

X_train = X.iloc[:split_index]
X_test  = X.iloc[split_index:]
y_train = y.iloc[:split_index]
y_test  = y.iloc[split_index:]
```

De esta forma, el modelo se entrena con datos antiguos y se prueba con datos futuros esto es con el motivo de predecir nueva información. 

---

## Limitaciones del proyecto

- El modelo usa únicamente variables históricas OHLCV (Variables usadas para medir el comportamiento de un instrumento financiero). 
- La predicción financiera es sensible a eventos externos no capturados por el dataset.
- La división 80/20 respeta el tiempo, pero podría mejorarse con validación tipo `TimeSeriesSplit`.
- Los modelos de árboles pueden predecir patrones no lineales, pero no modelan dependencias temporales complejas.

---

## Posibles mejoras

- Agregar variables derivadas como retornos, volatilidad, rango diario y medias móviles.
- Implementar validación cruzada temporal con `TimeSeriesSplit`.
- Probar ventanas temporales como variables predictoras.
- Se puede mejorar con un LSTM
