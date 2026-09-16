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

El conjunto fue dividido de forma estratificada en:

- **TRAIN:** 688 observaciones
- **TEST:** 173 observaciones

El conjunto **TEST permaneció reservado durante la selección y ajuste de los modelos**. Su apertura se realizó una única vez para la evaluación final del estudio principal.

---

## 🧪 Diseño experimental

Se evaluaron siete especificaciones para analizar el valor incremental de las diferentes familias de información:

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

- **Balanced Accuracy**
- **Macro-F1**
- **F1 de la clase No positiva**
- **Recall de la clase No positiva**

Este diseño permitió analizar no solo qué especificación obtenía mejor desempeño, sino también cuánto valor incremental aportaban las variables estructurales y contextuales frente al texto.

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

La selección del modelo y sus hiperparámetros se realizó **antes de observar los resultados del conjunto TEST**.

---

## 📈 Resultados finales

Resultados de **M4** sobre el conjunto TEST:

| Métrica | Resultado |
|---|---:|
| Balanced Accuracy | **0.8053** |
| Macro-F1 | **0.7869** |
| F1 No positiva | **0.6429** |
| Recall No positiva | **0.6923** |

En términos operativos, M4 identificó correctamente **18 de las 26 experiencias No positivas** presentes en TEST.

Como benchmark secundario, **M1 — Texto** obtuvo una Balanced Accuracy de **0.8314** en TEST.

Aunque M1 presentó descriptivamente un resultado superior en esta métrica, **M4 permanece como modelo oficial** porque fue seleccionado mediante el protocolo pre-test definido exclusivamente sobre TRAIN.

Los resultados de TEST no se utilizaron para modificar retrospectivamente la selección del modelo.

---

## 🔬 Robustez y evidencia complementaria

El proyecto incorpora análisis adicionales orientados a evaluar la estabilidad, escalabilidad y límites de generalización del sistema.

### Sensibilidades pre-test

- **S1:** Complete Case
- **S2:** Post-COVID 2023–2026
- **S3:** Generalización por POI
- **S4:** Contexto reducido por colinealidad

### Experimento A — Escalabilidad intra-dominio

Se evaluó el efecto de ampliar el conjunto de entrenamiento mediante **778 nuevas reseñas elegibles** correspondientes a los mismos POI del dominio original.

El conjunto de entrenamiento experimental pasó de:

**688 → 1.466 observaciones**

mientras el **TEST oficial de 173 reseñas permaneció congelado**.

Este experimento se realizó como análisis complementario y **no implicó una nueva selección del modelo oficial**.

Los resultados aportan evidencia favorable sobre la escalabilidad dentro del dominio analizado, pero no constituyen una garantía de rendimiento equivalente en territorios nuevos.

### Experimento B1 — Generalización territorial externa

Se evaluaron los modelos oficiales sobre un corpus independiente correspondiente a **Nariño**, compuesto por POI y territorio no observados durante el entrenamiento.

Este experimento permitió estudiar el comportamiento del sistema ante un cambio territorial y evidenció la necesidad de **validación local antes de desplegar PARCHAR Intelligence en nuevos destinos**.

Los experimentos complementarios no sustituyen los resultados oficiales del estudio principal y permiten caracterizar tanto oportunidades de escalabilidad como límites de generalización.

---

## 🧠 Interpretabilidad

El modelo final incorpora análisis de coeficientes para identificar variables y términos asociados predictivamente con cada clase.

Los coeficientes se interpretan como **asociaciones predictivas y no como relaciones causales**.

También se realizó un análisis post hoc de errores sobre TEST para caracterizar falsos positivos y falsos negativos **sin modificar el modelo final**.

---

## 🏗️ Arquitectura analítica

El flujo general de PARCHAR Intelligence puede resumirse como:

**Fuentes de datos → Data Engineering → QA → NLP → Feature Engineering → Machine Learning → Evaluación → Interpretabilidad → Business Intelligence**

El proyecto incorpora controles específicos para:

- prevención de **data leakage**;
- independencia **TRAIN / TEST**;
- deduplicación textual;
- trazabilidad de transformaciones;
- reproducibilidad;
- protección del conjunto TEST;
- separación entre el estudio principal y los experimentos complementarios.

---

## 📊 Business Intelligence

Los resultados analíticos se trasladan a una capa de **Business Intelligence desarrollada en Tableau**, orientada a perfiles no técnicos.

La solución permite explorar información relacionada con:

- destinos;
- puntos de interés;
- categorías turísticas;
- reseñas y satisfacción;
- periodos;
- indicadores contextuales, turísticos y macroeconómicos.

Esta capa permite complementar los resultados predictivos con una perspectiva descriptiva y territorial orientada al análisis y apoyo a la toma de decisiones.

La capa BI funciona como **herramienta de apoyo analítico y no como mecanismo de decisión automatizada**.

### 🌐 Tableau Public

La versión interactiva de **PARCHAR Intelligence** puede consultarse en:

**https://public.tableau.com/app/profile/leidy.johanna.moreno.posada/viz/TABLEAUPARCHARINTELLIGENCELJMP/Dashboard1?publish=yes**

---

## 🧳 Tourist Review Analyzer

Como prueba de concepto de productivización, PARCHAR Intelligence incorpora un **Tourist Review Analyzer**.

El prototipo permite introducir una nueva reseña y aplicar sobre ella el pipeline de Procesamiento del Lenguaje Natural y el modelo **M4 previamente entrenado** para obtener una clasificación:

**Positiva / No positiva**

El modelo no se reentrena durante la inferencia.

Las fotografías incorporadas en la experiencia demostrativa tienen únicamente una función visual y **no participan en la predicción**.

La implementación y demostración del prototipo se encuentran integradas en el **notebook final del proyecto**.

---

## 🛠️ Tecnologías

El proyecto fue desarrollado principalmente con:

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

## 📦 Artefactos del proyecto

El repositorio reúne los principales artefactos reproducibles y documentales de **PARCHAR Intelligence**:

- **Memoria final:** documento académico del TFM en formato PDF.
- **Notebook reproducible:** exportación HTML del pipeline analítico completo y sus resultados.
- **Tableau Packaged Workbook:** versión empaquetada de la capa de Business Intelligence.
- **Presentación:** síntesis visual desarrollada para la presentación audiovisual del proyecto.
- **Anexo documental:** confirmación de ANATO relacionada con el uso académico de la información empleada en el proyecto.

La versión interactiva de la capa de Business Intelligence se encuentra disponible mediante **Tableau Public**.

---

## 📁 Estructura del repositorio

```text
PARCHAR-Intelligence-TFM/
│
├── README.md
├── .gitignore
│
├── notebook/
│   └── Leidy_Johanna_Moreno_Posada_PARCHAR_Notebook.html
│
├── tableau/
│   └── Leidy_Johanna_Moreno_Posada_PARCHAR_Tableau.twbx
│
├── docs/
│   └── Leidy_Johanna_Moreno_Posada_TFM_PARCHAR_Intelligence.pdf
│
├── presentation/
│   └── Leidy_Johanna_Moreno_Posada_PARCHAR_Presentacion.pdf
│
└── anexos/
    └── Leidy_Johanna_Moreno_Posada_Confirmacion_ANATO.pdf
```

---

## 🔄 Reproducibilidad

El notebook final documenta el pipeline analítico desarrollado para PARCHAR Intelligence, incluyendo las principales etapas de:

1. adquisición e integración de fuentes;
2. controles de calidad y trazabilidad;
3. preparación y limpieza de datos;
4. Procesamiento del Lenguaje Natural;
5. construcción de variables;
6. separación TRAIN / TEST;
7. modelado y validación cruzada;
8. selección pre-test;
9. evaluación final;
10. interpretabilidad y análisis de errores;
11. análisis complementarios de robustez y generalización;
12. prueba de concepto de inferencia sobre nuevas reseñas.

La versión HTML permite consultar el desarrollo completo y los resultados obtenidos **sin necesidad de ejecutar el notebook**.

---

## 🗃️ Nota sobre los datos y fuentes externas

Los datos utilizados en el proyecto proceden de distintas fuentes públicas y documentales identificadas y citadas en la memoria académica.

El repositorio prioriza la **reproducibilidad metodológica, documentación y trazabilidad del proyecto** y no redistribuye de forma indiscriminada datasets o documentos originales procedentes de terceros.

Las fuentes, criterios de integración y transformaciones utilizadas se encuentran documentados en la memoria y en el notebook final.

El **Índice de Competitividad Turística Regional de Colombia (ICTRC) 2025**, utilizado como fuente documental del proyecto, se encuentra debidamente referenciado en la memoria académica y **no se redistribuye íntegramente en este repositorio**.

---

## ⚠️ Alcance y limitaciones

Los resultados corresponden al dominio y a los datos analizados en el Trabajo Fin de Máster.

La evidencia complementaria muestra que el desempeño puede variar cuando cambian las condiciones territoriales o el dominio de aplicación.

Por esta razón, **PARCHAR Intelligence debe entenderse como una herramienta de apoyo analítico y no como un sistema de decisión automatizada**.

La aplicación a nuevos territorios requiere validación adicional y, cuando corresponda, incorporación de nuevos datos y reentrenamiento.

---

## 👩‍💻 Autora

**Leidy Johanna Moreno Posada**

Trabajo Fin de Máster  
**Máster en Big Data, Data Science y Business Analytics**  
Universidad Complutense de Madrid  

2026

---

## 📌 Estado del proyecto

**TFM finalizado — 2026**

El pipeline analítico principal se encuentra cerrado y los resultados oficiales están congelados.

Los experimentos complementarios se presentan como análisis de robustez, escalabilidad y generalización y **no modifican retrospectivamente la selección del modelo oficial**.

**PARCHAR Intelligence — descubrir · analizar · decidir. 🇨🇴**
