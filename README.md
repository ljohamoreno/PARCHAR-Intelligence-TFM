# 🇨🇴 PARCHAR Intelligence

### Sistema de inteligencia turística basado en analítica de reseñas

**PARCHAR Intelligence** es una propuesta de inteligencia turística desarrollada como Trabajo Fin de Máster que integra **Data Engineering, Procesamiento del Lenguaje Natural (PLN), Machine Learning y Business Intelligence** para transformar reseñas de viajeros en información analítica orientada a la toma de decisiones.

El nombre toma como referencia el verbo coloquial colombiano **“parchar”**, asociado a compartir, encontrarse y disfrutar de un lugar o experiencia, vinculando la identidad local del proyecto con la experiencia turística que busca analizar.

---

## 🎯 Objetivo

Diseñar y validar un pipeline analítico reproducible capaz de clasificar reseñas turísticas como **Positivas** o **No positivas**, evaluar el valor incremental de diferentes familias de variables y trasladar los resultados del modelado a una perspectiva de inteligencia turística.

---

## 📊 Dataset

El estudio principal utiliza un dataset analítico final compuesto por:

- **861 reseñas turísticas**
- **25 puntos de interés (POI)**
- **5 destinos turísticos colombianos**
- **733 reseñas positivas**
- **128 reseñas no positivas**
- Variables **textuales, estructurales y contextuales**

El conjunto fue dividido en:

- **TRAIN:** 688 observaciones
- **TEST:** 173 observaciones

El conjunto TEST permaneció reservado durante la selección, ajuste y análisis de sensibilidad de los modelos.

---

## 🧪 Diseño experimental

Se evaluaron siete especificaciones:

| Modelo | Especificación |
|---|---|
| **M0** | Baseline de clase mayoritaria |
| **M1** | Texto |
| **M2** | Estructura |
| **M3** | Contexto |
| **M4** | Texto + Estructura |
| **M5** | Texto + Contexto |
| **M6** | Texto + Estructura + Contexto |

La selección se realizó exclusivamente sobre **TRAIN** mediante validación cruzada estratificada.

Las métricas principales fueron:

- Balanced Accuracy
- Macro-F1
- F1 de la clase No positiva
- Recall de la clase No positiva

---

## 🏆 Modelo oficial

El protocolo de selección pre-test determinó como modelo principal:

### M4 — Texto + Estructura

El pipeline combina:

- representación textual mediante **TF-IDF**;
- variables estructurales del destino y POI;
- codificación e imputación dentro del pipeline;
- clasificador **LinearSVC**;
- `class_weight="balanced"`.

La selección del modelo y sus hiperparámetros se realizó antes de observar los resultados del conjunto TEST.

---

## 📈 Resultados finales

Resultados de **M4** sobre el conjunto TEST:

| Métrica | Resultado |
|---|---:|
| Balanced Accuracy | **0.8053** |
| Macro-F1 | **0.7869** |
| F1 No positiva | **0.6429** |
| Recall No positiva | **0.6923** |

**M1 — Texto** obtuvo una Balanced Accuracy de **0.8314** en TEST y se conserva como benchmark secundario.

M4 permanece como modelo oficial porque fue seleccionado mediante el protocolo pre-test definido exclusivamente sobre TRAIN; los resultados de TEST no se utilizaron para modificar retrospectivamente la selección.

---

## 🔬 Robustez y evidencia complementaria

El proyecto incorpora análisis adicionales orientados a evaluar la estabilidad y los límites de generalización del sistema.

### Sensibilidades pre-test

- **S1:** Complete Case
- **S2:** Post-COVID 2023–2026
- **S3:** Generalización por POI
- **S4:** Contexto reducido por colinealidad

### Experimento A — Escalabilidad intra-dominio

Evalúa el efecto de ampliar el volumen de entrenamiento utilizando nuevas reseñas correspondientes a los mismos POI del dominio original, manteniendo congelado el TEST oficial.

### Experimento B1 — Generalización territorial externa

Evalúa los modelos oficiales sobre un corpus independiente de **Nariño**, compuesto por POI y territorio no observados durante el entrenamiento.

Los experimentos complementarios no sustituyen los resultados oficiales del estudio principal y permiten caracterizar tanto oportunidades de escalabilidad como limitaciones de generalización.

---

## 🧠 Interpretabilidad

El modelo final incorpora análisis de coeficientes para identificar variables y términos asociados predictivamente con cada clase.

Los coeficientes se interpretan como **asociaciones predictivas**, no como relaciones causales.

También se realiza un análisis post hoc de errores sobre TEST para caracterizar falsos positivos y falsos negativos sin modificar el modelo final.

---

## 🏗️ Arquitectura analítica

El flujo general de PARCHAR Intelligence puede resumirse como:

**Fuentes de datos → Data Engineering → QA → NLP → Feature Engineering → Machine Learning → Evaluación → Interpretabilidad → Business Intelligence**

El proyecto incorpora controles específicos para:

- prevención de data leakage;
- independencia TRAIN / TEST;
- deduplicación textual;
- trazabilidad de transformaciones;
- reproducibilidad;
- protección del conjunto TEST;
- separación entre estudio principal y experimentos complementarios.

---

## 📊 Business Intelligence

Los resultados analíticos se trasladan a una capa de **Business Intelligence en Tableau**, orientada a perfiles no técnicos.

La solución permite explorar indicadores de experiencia turística y analizar diferencias entre:

- destinos;
- puntos de interés;
- categorías;
- periodos;
- señales de satisfacción e insatisfacción.

La capa BI funciona como apoyo a la interpretación y a la toma de decisiones, no como mecanismo de decisión automatizada.

---

## 🧳 Tourist Review Analyzer

Como prueba de concepto de productivización, PARCHAR Intelligence incorpora un **Tourist Review Analyzer**.

El prototipo permite introducir una nueva reseña y aplicar sobre ella el pipeline NLP y el modelo M4 previamente entrenado para obtener una clasificación:

**Positiva / No positiva**

El modelo no se reentrena durante la inferencia y las fotografías incorporadas en la experiencia demostrativa no participan en la predicción.

---

## 🛠️ Tecnologías

- **Python**
- **pandas**
- **NumPy**
- **scikit-learn**
- **TF-IDF**
- **LinearSVC**
- **Google Colab**
- **Tableau**
- **Git / GitHub**

---

## 📁 Estructura del repositorio

```text
PARCHAR-Intelligence-TFM/
│
├── README.md
├── .gitignore
├── notebooks/
├── reports/
├── figures/
└── docs/
```

La estructura será completada con los artefactos finales y reproducibles del proyecto.

---

## ⚠️ Alcance y limitaciones

Los resultados corresponden al dominio y a los datos analizados en el TFM.

La evidencia complementaria muestra que el desempeño puede variar cuando cambian las condiciones territoriales o el dominio de aplicación. Por esta razón, **PARCHAR Intelligence debe entenderse como una herramienta de apoyo analítico y no como un sistema de decisión automatizada**.

La aplicación a nuevos territorios requiere validación adicional y, cuando corresponda, incorporación de nuevos datos y reentrenamiento.

---

## 👩‍💻 Autora

**Leidy Johanna Moreno P.**

Trabajo Fin de Máster  
**Máster en Big Data, Data Science y Business Analytics**

Universidad Complutense de Madrid

2026

---

## 📌 Estado del proyecto

**TFM — versión final**

El repositorio documenta el desarrollo metodológico, los resultados y los artefactos reproducibles de **PARCHAR Intelligence**.
