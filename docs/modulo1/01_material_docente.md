# Módulo 1 — Material Docente: Fundamentos de Visión por Computador

---

## 1. La imagen digital

### 1.1 ¿Qué es una imagen digital?

Una imagen digital es una representación discreta de una escena visual, almacenada como una matriz de valores numéricos. Cada posición de la matriz corresponde a un **píxel** (*picture element*), la unidad mínima de información de la imagen.

En Python con NumPy, una imagen es simplemente un `ndarray`:

```python
import cv2
import numpy as np

img = cv2.imread("imagen.jpg")
print(type(img))    # <class 'numpy.ndarray'>
print(img.shape)    # (alto, ancho, canales) → p.ej. (480, 640, 3)
print(img.dtype)    # uint8
```

### 1.2 Píxel y resolución espacial

- **Píxel**: unidad mínima de la imagen. Contiene el valor de intensidad (o color) en esa posición.
- **Resolución espacial**: número de píxeles que forman la imagen, expresado como `ancho × alto` (ej. 1920×1080).

> Una mayor resolución implica más detalle pero también mayor tamaño en memoria y mayor coste computacional.

```python
alto, ancho, canales = img.shape
print(f"Resolución: {ancho}×{alto} píxeles")
print(f"Total de píxeles: {alto * ancho:,}")
print(f"Total de valores en memoria: {img.size:,}")  # alto × ancho × canales
```

### 1.3 Profundidad de bits y tipos de dato

La **profundidad de bits** determina el rango de valores que puede tomar cada píxel:

| Bits | Tipo NumPy | Rango de valores | Uso típico |
|------|-----------|-------------------|------------|
| 8 | `uint8` | 0–255 | Imágenes RGB estándar |
| 16 | `uint16` | 0–65535 | Sensores multiespectrales |
| 32 | `float32` | 0.0–1.0 (normalizado) | Procesamiento y cálculos |

```python
# Imagen estándar (uint8)
img_uint8 = cv2.imread("imagen.jpg")
print(img_uint8.dtype)   # uint8
print(img_uint8.max())   # 255

# Convertir a float32 normalizado [0, 1]
img_float = img_uint8.astype(np.float32) / 255.0
print(img_float.dtype)   # float32
print(img_float.max())   # 1.0
```

> **Nota para imágenes multiespectrales**: los sensores agrícolas suelen producir imágenes de 16 bits (`uint16`). Es importante normalizar antes de operar con ellas.

---

## 2. Canales de color RGB

### 2.1 El modelo RGB

Una imagen en color almacena tres valores por píxel, uno por cada canal del modelo aditivo RGB:

- **R (Red / Rojo)**: reflectancia en longitudes de onda ~620–750 nm
- **G (Green / Verde)**: reflectancia en ~495–570 nm
- **B (Blue / Azul)**: reflectancia en ~450–495 nm

La imagen completa es un array 3D de forma `(alto, ancho, 3)`.

> **Atención**: OpenCV carga las imágenes en orden **BGR** (azul, verde, rojo), no RGB. Esto es importante tenerlo en cuenta al visualizar con Matplotlib.

### 2.2 Separar y visualizar canales

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Cargar imagen (OpenCV → BGR)
img_bgr = cv2.imread("imagen.jpg")

# Convertir a RGB para visualización correcta
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)

# Separar canales
R = img_rgb[:, :, 0]
G = img_rgb[:, :, 1]
B = img_rgb[:, :, 2]

# Visualizar imagen original y canales
fig, axes = plt.subplots(1, 4, figsize=(16, 4))

axes[0].imshow(img_rgb)
axes[0].set_title("Original")

axes[1].imshow(R, cmap="Reds")
axes[1].set_title("Canal Rojo (R)")

axes[2].imshow(G, cmap="Greens")
axes[2].set_title("Canal Verde (G)")

axes[3].imshow(B, cmap="Blues")
axes[3].set_title("Canal Azul (B)")

for ax in axes:
    ax.axis("off")

plt.tight_layout()
plt.show()
```

### 2.3 Indexación y operaciones sobre canales

```python
# Acceder a un píxel específico (fila 100, columna 200)
pixel = img_rgb[100, 200]
print(f"R={pixel[0]}, G={pixel[1]}, B={pixel[2]}")

# Estadísticas por canal
for i, canal in enumerate(["R", "G", "B"]):
    banda = img_rgb[:, :, i]
    print(f"{canal}: media={banda.mean():.1f}, std={banda.std():.1f}, "
          f"min={banda.min()}, max={banda.max()}")
```

### 2.4 Imagen en escala de grises

Una imagen en escala de grises colapsa los tres canales en uno solo, ponderando la contribución de cada canal según la percepción humana:

```
Gris = 0.299·R + 0.587·G + 0.114·B
```

```python
# Convertir a escala de grises
gris = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2GRAY)
print(gris.shape)   # (alto, ancho) — sin canal de color

plt.imshow(gris, cmap="gray")
plt.title("Escala de grises")
plt.axis("off")
plt.show()
```

---

## 3. Histogramas e intensidad

### 3.1 ¿Qué es un histograma?

Un histograma de imagen muestra la **distribución de frecuencias de los valores de intensidad**. El eje X representa los valores posibles (0–255 en uint8) y el eje Y la cantidad de píxeles que tienen ese valor.

Permite responder preguntas como:
- ¿La imagen está sobreexpuesta (histograma desplazado a la derecha)?
- ¿Tiene bajo contraste (histograma estrecho y centrado)?
- ¿Existen zonas saturadas (píxeles en 0 o en 255)?

### 3.2 Calcular y visualizar histogramas

```python
import cv2
import matplotlib.pyplot as plt

img_bgr = cv2.imread("imagen.jpg")
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)

fig, ax = plt.subplots(figsize=(8, 4))

colores = ("red", "green", "blue")
etiquetas = ("Rojo", "Verde", "Azul")

for i, (color, etiqueta) in enumerate(zip(colores, etiquetas)):
    # cv2.calcHist(imágenes, canales, máscara, bins, rango)
    hist = cv2.calcHist([img_rgb], [i], None, [256], [0, 256])
    ax.plot(hist, color=color, label=etiqueta)

ax.set_xlabel("Valor de intensidad (0–255)")
ax.set_ylabel("Número de píxeles")
ax.set_title("Histograma de canales RGB")
ax.legend()
plt.tight_layout()
plt.show()
```

### 3.3 Histograma con NumPy

```python
import numpy as np

canal_rojo = img_rgb[:, :, 0].flatten()  # aplanar a 1D
conteos, bordes = np.histogram(canal_rojo, bins=256, range=(0, 256))

print(f"Valor más frecuente: {np.argmax(conteos)}")
print(f"Media de intensidad: {canal_rojo.mean():.1f}")
```

### 3.4 Interpretación del histograma en teledetección

| Patrón en el histograma | Posible causa agronómica |
|---|---|
| Pico alto en valores bajos de R | Suelo expuesto o vegetación estresada |
| Pico alto en valores medios de G | Vegetación sana con alta reflectancia verde |
| Histograma plano | Imagen de baja calidad o muy heterogénea |
| Cola larga a la derecha en NIR | Vegetación densa y vigorosa |

### 3.5 Equalización de histograma (mejora de contraste)

```python
# Ecualización en escala de grises
gris = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2GRAY)
gris_eq = cv2.equalizeHist(gris)

fig, axes = plt.subplots(2, 2, figsize=(10, 8))
axes[0, 0].imshow(gris, cmap="gray"); axes[0, 0].set_title("Original")
axes[0, 1].imshow(gris_eq, cmap="gray"); axes[0, 1].set_title("Ecualizada")

hist_orig = cv2.calcHist([gris], [0], None, [256], [0, 256])
hist_eq = cv2.calcHist([gris_eq], [0], None, [256], [0, 256])

axes[1, 0].plot(hist_orig, color="gray"); axes[1, 0].set_title("Histograma original")
axes[1, 1].plot(hist_eq, color="gray"); axes[1, 1].set_title("Histograma ecualizado")

for ax in axes.flat:
    ax.axis("off") if ax.images else None

plt.tight_layout()
plt.show()
```

---

## 4. Introducción a OpenCV y NumPy

### 4.1 NumPy: arrays multidimensionales

NumPy es la columna vertebral del procesamiento de imágenes en Python. Una imagen es un `ndarray` de 2 o 3 dimensiones.

```python
import numpy as np

# Crear una imagen negra de 200×300 píxeles (uint8)
negro = np.zeros((200, 300, 3), dtype=np.uint8)

# Crear una imagen blanca
blanco = np.ones((200, 300, 3), dtype=np.uint8) * 255

# Crear una imagen de ruido aleatorio
ruido = np.random.randint(0, 256, (200, 300, 3), dtype=np.uint8)

# Operaciones aritméticas (se aplican elemento a elemento)
brillo = np.clip(negro.astype(np.int16) + 50, 0, 255).astype(np.uint8)
```

#### Indexación y slicing

```python
img = cv2.imread("imagen.jpg")

# Recortar una región de interés (ROI)
roi = img[50:150, 100:300]   # filas 50-150, columnas 100-300

# Modificar una región (pintar un rectángulo azul)
img[10:50, 10:100] = [255, 0, 0]  # formato BGR en OpenCV
```

### 4.2 OpenCV: operaciones esenciales

#### Leer, mostrar y guardar imágenes

```python
import cv2

# Leer
img = cv2.imread("imagen.jpg")              # uint8, BGR
img_color = cv2.imread("imagen.jpg", cv2.IMREAD_COLOR)    # igual
img_gris = cv2.imread("imagen.jpg", cv2.IMREAD_GRAYSCALE) # 1 canal
img_16bit = cv2.imread("imagen.tif", cv2.IMREAD_UNCHANGED) # preserva bits

# Mostrar (en entorno de escritorio)
cv2.imshow("Imagen", img)
cv2.waitKey(0)
cv2.destroyAllWindows()

# Guardar
cv2.imwrite("salida.jpg", img)
cv2.imwrite("salida.png", img)   # sin pérdida
```

#### Redimensionar y rotar

```python
# Redimensionar a 640×480
img_resized = cv2.resize(img, (640, 480))

# Redimensionar manteniendo proporciones (50% del tamaño original)
img_half = cv2.resize(img, None, fx=0.5, fy=0.5, interpolation=cv2.INTER_LINEAR)

# Rotar 90° en sentido horario
img_rotada = cv2.rotate(img, cv2.ROTATE_90_CLOCKWISE)
```

#### Conversiones de espacio de color

```python
# BGR → RGB (para Matplotlib)
rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# BGR → Escala de grises
gris = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# BGR → HSV (matiz, saturación, valor)
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

### 4.3 Visualización con Matplotlib

```python
import matplotlib.pyplot as plt
import cv2

img_bgr = cv2.imread("imagen.jpg")
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)

# Imagen simple
plt.figure(figsize=(6, 4))
plt.imshow(img_rgb)
plt.title("Mi primera imagen")
plt.axis("off")
plt.show()

# Múltiples imágenes
fig, axes = plt.subplots(1, 2, figsize=(10, 4))
axes[0].imshow(img_rgb)
axes[0].set_title("Original")
axes[1].imshow(cv2.cvtColor(img_bgr, cv2.COLOR_BGR2GRAY), cmap="gray")
axes[1].set_title("Escala de grises")
for ax in axes:
    ax.axis("off")
plt.show()
```

### 4.4 Flujo de trabajo básico

```
Imagen en disco
      │
      ▼
cv2.imread()          → ndarray (uint8, BGR)
      │
      ▼
Conversión de color   → cv2.cvtColor()
      │
      ▼
Operaciones NumPy     → recorte, aritmética, estadísticas
      │
      ▼
Visualización         → plt.imshow()
      │
      ▼
Guardado              → cv2.imwrite()
```

---

## Resumen del módulo

| Concepto | Clave |
|---|---|
| Imagen digital | Array NumPy `(alto, ancho, canales)` |
| Profundidad de bits | `uint8` (0–255) en RGB; `uint16` en multiespectrales |
| Canales RGB | Tres matrices 2D: R, G, B (o B, G, R en OpenCV) |
| Histograma | Distribución de frecuencias de intensidad por canal |
| OpenCV | `imread`, `cvtColor`, `calcHist`, `resize` |
| NumPy | Indexación, slicing, operaciones aritméticas, `clip` |
