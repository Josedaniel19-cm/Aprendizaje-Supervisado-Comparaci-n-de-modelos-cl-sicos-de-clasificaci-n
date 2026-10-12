# Aprendizaje-Supervisado-Comparacion-de-modelos-clasicos-de-clasificacio
Descripción del proyecto
Este proyecto compara tres modelos clásicos de aprendizaje supervisado para clasificar diagnósticos benignos y malignos a partir del conjunto de datos Breast Cancer Wisconsin (Diagnostic), disponible en Kaggle. El propósito es evaluar su rendimiento y su capacidad de generalización mediante la métrica ROC AUC y validación cruzada.
Este análisis tiene fines académicos y no debe utilizarse como herramienta de diagnóstico médico.

Dataset
- Fuente: Breast Cancer Wisconsin (Diagnostic) — Kaggle
- Variable objetivo: diagnosis.
- Codificación: B = 0 (benigno) y M = 1 (maligno).
- Predictores: 30 variables numéricas relacionadas con características de los núcleos celulares.
- Preparación: se eliminan las columnas identificadoras o vacías, se revisan los valores faltantes y duplicados, y se comprueba la distribución de las clases.
Descarga el archivo CSV desde Kaggle y guárdalo como data.csv en la carpeta del proyecto. El notebook espera encontrar ese archivo en la misma carpeta; si está en otra ubicación, actualiza la ruta de carga en el código.

Metodología
1. Exploración y limpieza: revisión de dimensiones, tipos de datos, estadísticas descriptivas y distribución de las clases.
2. División de los datos: 70 % para entrenamiento, 15 % para validación y 15 % para prueba. Se utiliza estratificación para conservar aproximadamente la proporción de las clases.
3. Preprocesamiento: StandardScaler dentro de un Pipeline, para que el escalador se ajuste únicamente con los datos de entrenamiento de cada partición y evitar fuga de información.
4. Modelos comparados:
   - Regresión Logística (LogisticRegression)
   - K Vecinos más Cercanos (KNeighborsClassifier)
   - Árbol de Decisión (DecisionTreeClassifier)
5. Evaluación: ROC AUC en entrenamiento y validación, validación cruzada estratificada de cinco folds sobre el conjunto de entrenamiento y evaluación final con el conjunto de prueba reservado.

6. Resultados
Los resultados obtenidos en la ejecución del notebook fueron:
| Modelo | ROC AUC entrenamiento | ROC AUC validación | Fold 1 | Fold 2 | Fold 3 | Fold 4 | Fold 5 | ROC AUC medio CV | Desviación estándar CV |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Regresión Logística | 0.9972 | 0.9965 | 0.9960 | 0.9967 | 0.9867 | 1.0000 | 0.9834 | 0.9926 | 0.0064 |
| K Vecinos más Cercanos | 0.9975 | 0.9779 | 0.9950 | 0.9963 | 0.9810 | 0.9697 | 0.9769 | 0.9838 | 0.0104 |
| Árbol de Decisión | 1.0000 | 0.8561 | 0.9200 | 0.9067 | 0.9567 | 0.8810 | 0.9210 | 0.9171 | 0.0245 |

La Regresión Logística fue seleccionada por obtener el mayor ROC AUC medio en validación cruzada y la menor desviación estándar. En la evaluación final alcanzó un ROC AUC de 0.9988 en el conjunto de prueba. K Vecinos más Cercanos también tuvo un rendimiento alto, mientras que la diferencia entre entrenamiento y validación del Árbol de Decisión sugiere posible sobreajuste.
ROC AUC mide la capacidad de discriminar entre las dos clases; no significa que todas las predicciones sean correctas ni sustituye una evaluación clínica.

Conclusión
En este experimento, la Regresión Logística mostró el mejor equilibrio entre rendimiento y estabilidad en las evaluaciones realizadas. La validación cruzada permitió comparar los modelos de forma más robusta, mientras que el conjunto de prueba reservado ofreció una comprobación final de su capacidad de discriminación sobre datos no utilizados durante el entrenamiento ni la selección del modelo.
