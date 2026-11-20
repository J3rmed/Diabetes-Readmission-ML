# Predicción de readmisión hospitalaria a 30 días en pacientes diabéticos

Proyecto del curso **Modelos y Simulación de Sistemas II** (Universidad de Antioquia).

**Autores:** Jefferson Stiven Lizarazo Arias · Juan Ricardo Vélez Ciro

Se predice, al momento del alta, si una hospitalización de un paciente diabético termina en una readmisión en menos
de 30 días, con el conjunto *Diabetes 130-US Hospitals for Years 1999–2008* (UCI / Kaggle).

## Contenido

| Archivo | Qué contiene |
|---|---|
| Avance 1 - Descripción del problema y EDA | Entregado (`informe/`) |
| `informe_final.pdf` | Informe en formato IEEE (8 páginas) |
| `notebooks/01_analisis_exploratorio.ipynb` | Sección 2: descripción y análisis exploratorio de los datos |
| `notebooks/02_modelos_readmision.ipynb` | Sección 4: validación, experimentos de diseño, 6 modelos, ajuste de hiperparámetros y evaluación en prueba |
| `notebooks/03_reduccion_dimension.ipynb` | Sección 5: análisis individual de variables, PCA y UMAP |
| `data_set/` | `diabetic_data.csv` y `IDS_mapping.csv` |
| `resultados/` | Tablas (`.csv`, `.json`) y figuras generadas por los notebooks |

## Cómo reproducir los resultados (Google Colab)

1. Descargue el conjunto de datos de Kaggle o UCI (<https://doi.org/10.24432/C5230J>) y copie
   `diabetic_data.csv` e `IDS_mapping.csv` en `Mi unidad/.../Proyecto_final/data_set/` de su Google Drive.
2. Abra los notebooks en Colab **en orden** (01 → 02 → 03). La primera celda de configuración monta el Drive y busca
   automáticamente la carpeta `Proyecto_final/data_set`. Los resultados se guardan en `Proyecto_final/resultados/`.
3. El notebook 03 lee `resultados/mejores_modelos.json`, generado por el notebook 02.

Para ejecutar localmente: defina la variable de entorno `PROYECTO_DIR` con la carpeta que contiene `data_set/` (el notebook 01 usa `DATA_DIR`, la carpeta `data_set/` misma)
y ejecute los notebooks con Jupyter.

### Dependencias

Python ≥ 3.10, `numpy`, `pandas`, `scipy`, `scikit-learn` ≥ 1.2, `matplotlib`, `seaborn`, `umap-learn`
(el notebook 03 lo instala si no está).

## Decisiones metodológicas principales

- **Unidad de análisis:** todas las hospitalizaciones depuradas (99 340), con **validación por grupos de paciente**
  (`StratifiedGroupKFold` sobre `patient_nbr`): ningún paciente aparece a la vez en entrenamiento y validación/prueba.
- **Partición:** 20 % de los pacientes para prueba (no se usa hasta el final); validación cruzada de 5 folds por grupos
  en el 80 % restante para ajustar hiperparámetros.
- **Preprocesamiento sin fuga:** todo lo que usa estadísticos de los datos (escalado, categorías frecuentes, PCA, UMAP)
  se ajusta dentro de cada fold solo con entrenamiento.
- **Desbalance:** pesos por clase (`class_weight="balanced"`) en los modelos que los admiten y umbral de decisión
  elegido con el índice de Youden; no se usa SMOTE (justificación en el informe).
- **Métrica principal:** AUC-ROC, con intervalos de confianza del 95 % (folds y *bootstrap* por paciente en prueba).

## Responsabilidad sobre el uso IA

## Video Explicativo:





