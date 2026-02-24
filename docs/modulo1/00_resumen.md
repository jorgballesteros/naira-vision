# Módulo 1 — Fundamentos de Visión por Computador

## Descripción general

Este módulo introduce los conceptos fundamentales de la imagen digital y las herramientas Python que se utilizarán a lo largo del curso. Es el punto de partida para entender cómo el computador representa, almacena y procesa información visual antes de aplicarla al análisis de cultivos.

---

## Objetivos de aprendizaje

Al finalizar este módulo, el alumno será capaz de:

- Explicar qué es una imagen digital y cómo se representa en memoria.
- Identificar píxeles, resolución espacial y profundidad de bits.
- Separar y manipular los canales RGB de una imagen.
- Calcular e interpretar histogramas de intensidad.
- Leer, visualizar y modificar imágenes con OpenCV y NumPy.

---

## Competencias que se trabajan

| Competencia | Nivel |
|---|---|
| Representación digital de imágenes | Comprensión conceptual |
| Manejo de arrays multidimensionales con NumPy | Aplicación básica |
| Lectura y visualización de imágenes con OpenCV | Aplicación básica |
| Análisis de histogramas | Comprensión e interpretación |

---

## Contenidos

| Tema | Descripción |
|---|---|
| 1. La imagen digital | Píxel, resolución, profundidad de bits, tipos de datos |
| 2. Canales de color RGB | Descomposición en bandas, visualización por canal |
| 3. Histogramas e intensidad | Distribución de valores, contraste, saturación |
| 4. OpenCV y NumPy | Carga de imágenes, operaciones básicas sobre arrays |

---

## Duración estimada

| Actividad | Tiempo |
|---|---|
| Lectura del material docente | 1.5 h |
| Ejercicios guiados | 1.5 h |
| **Total** | **3 h** |

---

## Recursos y prerequisitos

**Conocimientos previos recomendados:**
- Python básico (listas, bucles, funciones)
- Álgebra matricial elemental (filas, columnas, indexación)

**Librerías necesarias:**
```bash
pip install numpy opencv-python matplotlib
```

**Archivos de este módulo:**
- `01_material_docente.md` — teoría completa con ejemplos de código
- `02_ejercicios.md` — ejercicios prácticos

---

## Conexión con módulos posteriores

Los conceptos de este módulo son la base de todos los demás:
- **Módulo 2** usará estas herramientas para trabajar con imágenes aéreas reales.
- **Módulo 3** extenderá el modelo RGB al espectro infrarrojo (NIR).
- **Módulo 4** aplicará correcciones radiométricas sobre las bandas aquí aprendidas.
