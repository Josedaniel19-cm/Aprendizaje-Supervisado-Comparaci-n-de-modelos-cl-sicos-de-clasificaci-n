# Aprendizaje-Supervisado-Comparacion-de-modelos-clasicos-de-clasificacio
#Jose Daniel Cabarcas Mass 
#Ciencia de datos II
1. Descripción del proyecto
# Este proyecto desarrolla un experimento de clasificación binaria para comparar el rendimiento de tres algoritmos clásicos de aprendizaje supervisado. Se utiliza el conjunto de datos Breast Cancer Wisconsin (Diagnostic) y se evalúa la capacidad de los modelos para distinguir entre diagnósticos benignos y malignos a partir de características numéricas de los núcleos celulares.
El trabajo hace énfasis en la preparación de los datos, el uso de pipelines de preprocesamiento, la validación cruzada y la evaluación final en un conjunto de prueba que se mantiene separado durante la selección del modelo.
Nota de uso: este proyecto tiene fines exclusivamente académicos. Los resultados del modelo no constituyen un diagnóstico médico ni deben utilizarse para tomar decisiones clínicas.

2. Objetivo
#Comparar tres modelos de clasificación mediante la métrica ROC AUC y la validación cruzada estratificada de cinco particiones, para identificar cuál presenta el mejor rendimiento y una mayor estabilidad en las evaluaciones realizadas.

3. # Dataset
- Nombre: Breast Cancer Wisconsin (Diagnostic).
- Fuente: Kaggle — Breast Cancer Wisconsin (Diagnostic) Data Set.
- Tipo de problema: clasificación binaria.
- Número de observaciones: 569 registros en la versión estándar.
- Variables predictoras: 30 variables numéricas relacionadas con el radio, textura, perímetro, área, suavidad, compactación, concavidad, puntos cóncavos, simetría y dimensión fractal de los núcleos celulares.
- Variable objetivo: diagnosis.
- Codificación de la variable objetivo: 0 = benigno (B) y 1 = maligno (M).
- Distribución original de las clases: 357 casos benignos (aproximadamente 62,7 %) y 212 casos malignos (aproximadamente 37,3 %).
Durante la limpieza se eliminan columnas que no aportan información predictiva útil, como el identificador id y la columna vacía Unnamed: 32, cuando están presentes. También se revisan los tipos de datos, los valores faltantes y los registros duplicados.

Descargar y ubicar los datos
Descarga el archivo CSV desde Kaggle, descomprímelo si es necesario y colócalo en la misma carpeta que el notebook con el nombre data.csv. El notebook busca ese nombre y algunas variantes; si hay varios archivos CSV en la carpeta, renombra el archivo correcto como data.csv para evitar confusiones.
4. Metodología
El experimento se desarrolla en las siguientes etapas:
1. Carga y exploración: revisión de las dimensiones, los tipos de datos, las estadísticas descriptivas y la distribución de las clases.
2. Limpieza: estandarización de los nombres de las columnas, eliminación de columnas irrelevantes, codificación de la variable objetivo y revisión de valores faltantes y duplicados.
3. División de los datos: 70 % para entrenamiento, 15 % para validación y 15 % para prueba. Se usa estratificación para conservar aproximadamente la proporción de las clases en cada partición.

4. Preprocesamiento: StandardScaler dentro de un Pipeline. De esta forma, el escalador se ajusta solamente con los datos de entrenamiento de cada partición y se reduce el riesgo de fuga de información (data leakage).
Entrenamiento y comparación: se entrenan tres clasificadores con sus hiperparámetros por defecto:- Regresión Logística (LogisticRegression).
- K Vecinos más Cercanos (KNeighborsClassifier).
- Árbol de Decisión (DecisionTreeClassifier).

Evaluación inicial: cálculo de ROC AUC en entrenamiento y validación.
Validación cruzada: aplicación de StratifiedKFold con cinco folds sobre el conjunto de entrenamiento; se registran las puntuaciones individuales, el promedio y la desviación estándar.
Evaluación final: selección de la Regresión Logística con base en los resultados anteriores y cálculo de su ROC AUC en el conjunto de prueba reservado.

5. Métrica de evaluación: ROC AUC
El área bajo la curva ROC (ROC AUC) mide la capacidad de un clasificador para ordenar o distinguir las observaciones de la clase positiva frente a la negativa. En este experimento, la clase positiva es el diagnóstico maligno (1). Un valor más cercano a 1 representa una mayor capacidad de discriminación; un valor cercano a 0,5 indica una capacidad similar al azar.
ROC AUC no representa directamente el porcentaje de predicciones correctas y no debe interpretarse como una garantía de acierto para cada observación.
6. Resultados
  6.1 Comparación general de los modelos

| Modelo | ROC AUC entrenamiento | ROC AUC validación | ROC AUC medio CV | Desv. estándar CV |
|---|---:|---:|---:|---:|
| Regresión Logística | 0.9972 | 0.9965 | 0.9926 | 0.0064 |
| K Vecinos más Cercanos | 0.9975 | 0.9779 | 0.9838 | 0.0104 |
| Árbol de Decisión | 1.0000 | 0.8561 | 0.9171 | 0.0245 |

### 6.2 Resultados de los cinco folds

| Modelo | Fold 1 | Fold 2 | Fold 3 | Fold 4 | Fold 5 |
|---|---:|---:|---:|---:|---:|
| Regresión Logística | 0.9960 | 0.9967 | 0.9867 | 1.0000 | 0.9834 |
| K Vecinos más Cercanos | 0.9950 | 0.9963 | 0.9810 | 0.9697 | 0.9769 |
| Árbol de Decisión | 0.9200 | 0.9067 | 0.9567 | 0.8810 | 0.9210 |

### 6.3 Evaluación final del modelo seleccionado

La Regresión Logística fue evaluada una vez más sobre el conjunto de prueba, que se había mantenido reservado durante la selección del modelo.

| Métrica | ROC AUC |
|---|---:|
| Validación | 0.9965 |
| Validación cruzada (media) | 0.9926 |
| Prueba final | 0.9988 |
Modelo	ROC AUC entrenamiento	ROC AUC validación	ROC AUC medio CV	Desv. estándar CV
Regresión Logística	0.9972	0.9965	0.9926	0.0064
K Vecinos más Cercanos	0.9975	0.9779	0.9838	0.0104
Árbol de Decisión	1.0000	0.8561	0.9171	0.0245




6.4 Interpretación: 
La Regresión Logística obtuvo el mejor promedio de ROC AUC en validación cruzada (0.9926) y la menor desviación estándar (0.0064), lo que indica un rendimiento alto y relativamente consistente entre los folds. Además, su ROC AUC en validación fue 0.9965 y en la prueba final alcanzó 0.9988.
K Vecinos más Cercanos también mostró un buen desempeño, aunque sus puntuaciones fueron algo inferiores y su variabilidad fue mayor. El Árbol de Decisión alcanzó un ROC AUC de 1.0000 en entrenamiento, pero obtuvo 0.8561 en validación y un promedio de 0.9171 en validación cruzada. Esta diferencia es indicio de posible sobreajuste.
Con base en las evaluaciones realizadas, la Regresión Logística se seleccionó como el modelo con mejor desempeño general entre los tres comparados. El resultado de prueba es cercano a los resultados previos, aunque corresponde a una sola partición y no garantiza el mismo rendimiento en todos los datos futuros.

7. Visualizaciones incluidas:
El notebook presenta gráficos para complementar la comparación de modelos, entre ellos:
- Distribución de las clases benignas y malignas.
- Comparación del ROC AUC en entrenamiento, validación y validación cruzada.
- Distribución de las puntuaciones ROC AUC obtenidas en los cinco folds.
- Comparación del desempeño de la Regresión Logística en validación, validación cruzada y prueba final.

Conclusiones
  
El experimento muestra que la selección de un modelo debe considerar no solo el desempeño en entrenamiento, sino también su resultado en validación y su estabilidad en validación cruzada. En este caso, la Regresión Logística presentó el mejor equilibrio entre una puntuación ROC AUC elevada y una variabilidad reducida, y mantuvo un rendimiento similar en la evaluación final. La comparación también permitió identificar señales de posible sobreajuste en el Árbol de Decisión.

