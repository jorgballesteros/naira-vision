# NAIRA Vision

### Análisis de Imágenes Multiespectrales para la Monitorización de Cultivos

Este repositorio contiene los materiales, código y recursos del curso:

> **Análisis de Imágenes Multiespectrales para la Monitorización Inteligente de Cultivos**

Desarrollado en el marco del proyecto **NAIRA – Sistema de inteligencia aplicada para la transformación circular del sector agroalimentario**.

---

## 🎯 Objetivo del Curso

Capacitar al alumnado para analizar imágenes RGB y multiespectrales mediante Python con el fin de evaluar el estado de salud de cultivos agrícolas.

El curso permite identificar:

* Estrés hídrico
* Deficiencias nutricionales
* Zonas de bajo vigor
* Posibles afecciones por plagas o enfermedades

A través del cálculo de índices espectrales y técnicas básicas de segmentación y clasificación, el alumnado desarrollará competencias en visión por computador aplicada a agricultura de precisión.

---

## 🧠 Competencias que se desarrollan

Al finalizar el curso, el alumnado será capaz de:

* Comprender el comportamiento espectral de la vegetación.
* Interpretar imágenes multiespectrales (RGB + NIR).
* Aplicar técnicas de preprocesamiento y corrección.
* Calcular índices de vegetación como NDVI, GNDVI, SAVI y EVI.
* Generar mapas de vigor y mapas de falso color.
* Aplicar segmentación y clasificación básica.
* Detectar patrones y anomalías en cultivos.

---

## ⏱ Duración

**24 horas totales**

* 18 horas de formación teórico-práctica
* 6 horas de trabajo autónomo (proyecto final)

---

## 📚 Contenidos del Curso

### 1️⃣ Fundamentos de Visión por Computador

* Imagen digital: píxel, resolución y profundidad de bits
* Canales RGB
* Histogramas y análisis de intensidad
* Introducción a OpenCV y NumPy

### 2️⃣ Visión Artificial en Agricultura

* Agricultura de precisión
* Resolución espacial vs espectral
* Casos de uso reales
* Introducción a imágenes aéreas y satelitales

### 3️⃣ Fundamentos de Imágenes Multiespectrales

* Espectro electromagnético
* Visible vs infrarrojo cercano (NIR)
* Firma espectral de la vegetación
* RGB vs multiespectral vs hiperespectral

### 4️⃣ Preprocesamiento de Imágenes

* Corrección radiométrica
* Normalización
* Reducción de ruido
* Alineación de bandas
* Formatos TIFF y GeoTIFF

### 5️⃣ Índices de Vegetación

* NDVI
* GNDVI
* SAVI
* EVI
* Interpretación agronómica de resultados

### 6️⃣ Segmentación y Machine Learning

* Thresholding
* K-means
* Clasificación supervisada básica
* Detección de anomalías

---

## 📂 Estructura del Repositorio

```
naira-vision/
├─ docs/                # Material teórico organizado por módulos
├─ assets/              # Diagramas e imágenes explicativas
├─ data/
│   ├─ raw/             # Imágenes originales
│   └─ processed/       # Dataset preprocesado
├─ notebooks/           # Exploración y prácticas guiadas
└─ src/
    ├─ preprocessing/   # Correcciones y limpieza
    ├─ indices/         # Cálculo de índices espectrales
    ├─ visualization/   # Generación de mapas y gráficos
    ├─ segmentation/    # Segmentación de zonas
    └─ ml/              # Modelos de clasificación
```

---

## 🖥 Requisitos Técnicos

* Python 3.10+
* 8 GB RAM mínimo recomendado
* Entorno virtual recomendado (`venv` o `conda`)

### Librerías principales

```bash
pip install numpy opencv-python matplotlib rasterio scikit-learn pandas
```

Opcional:

```bash
pip install geopandas scikit-image
```

---

## 📜 Licencia

Este repositorio forma parte del ecosistema formativo del proyecto NAIRA.
Consultar el archivo `LICENSE` para más información.

---

## 📬 Contacto

Proyecto NAIRA
Jorge Ballesteros


