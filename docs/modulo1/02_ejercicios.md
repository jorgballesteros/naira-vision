# Módulo 1 — Ejercicios Prácticos

> **Nivel**: Intermedio
> **Librerías**: `numpy`, `opencv-python`, `matplotlib`
> **Tiempo estimado**: 1.5 h

---

## Ejercicio 1 — Explorar una imagen

**Objetivo**: Familiarizarse con la estructura de una imagen como array NumPy.

### Enunciado

Carga cualquier imagen RGB (`.jpg` o `.png`) y responde las siguientes preguntas con código Python:

1. ¿Cuáles son sus dimensiones (alto, ancho, canales)?
2. ¿Cuál es el tipo de dato de los píxeles?
3. ¿Cuál es el valor máximo y mínimo de toda la imagen?
4. ¿Cuántos megabytes ocupa en memoria?
5. ¿Cuál es el valor del píxel en la posición (fila=50, columna=80)?

### Pistas

- Usa `img.shape`, `img.dtype`, `img.min()`, `img.max()`, `img.nbytes`.
- `img.nbytes / 1024 / 1024` convierte bytes a megabytes.

<details>
<summary>Solución</summary>

```python
import cv2
import numpy as np

img = cv2.imread("imagen.jpg")

print(f"Dimensiones: {img.shape}")
print(f"Tipo de dato: {img.dtype}")
print(f"Valor mínimo: {img.min()}")
print(f"Valor máximo: {img.max()}")
print(f"Tamaño en memoria: {img.nbytes / 1024 / 1024:.2f} MB")
print(f"Píxel (50, 80): {img[50, 80]}")  # valor en BGR
```
</details>

---

## Ejercicio 2 — Separar y comparar canales RGB

**Objetivo**: Extraer y visualizar individualmente cada canal de color.

### Enunciado

1. Carga una imagen a color y conviértela a RGB (recuerda el orden BGR de OpenCV).
2. Separa los tres canales: R, G, B.
3. Crea una figura con 4 subplots:
   - Imagen original en color
   - Canal R (colormap `Reds`)
   - Canal G (colormap `Greens`)
   - Canal B (colormap `Blues`)
4. Añade título a cada subplot.
5. **Pregunta de análisis**: ¿En qué canal esperas ver mayor intensidad si la imagen muestra vegetación sana? ¿Por qué?

### Pistas

- `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` para convertir.
- `img[:, :, 0]` extrae el primer canal (R tras la conversión).
- `plt.subplots(1, 4)` crea 4 subplots en fila.

<details>
<summary>Solución</summary>

```python
import cv2
import matplotlib.pyplot as plt

img_bgr = cv2.imread("imagen.jpg")
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)

R, G, B = img_rgb[:, :, 0], img_rgb[:, :, 1], img_rgb[:, :, 2]

fig, axes = plt.subplots(1, 4, figsize=(16, 4))
axes[0].imshow(img_rgb);          axes[0].set_title("Original")
axes[1].imshow(R, cmap="Reds");   axes[1].set_title("Canal R")
axes[2].imshow(G, cmap="Greens"); axes[2].set_title("Canal G")
axes[3].imshow(B, cmap="Blues");  axes[3].set_title("Canal B")
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()

# Análisis: la vegetación sana refleja fuertemente en el verde (G),
# por lo que el canal G mostrará mayor intensidad en zonas con plantas.
```
</details>

---

## Ejercicio 3 — Estadísticas por canal

**Objetivo**: Calcular métricas de intensidad para comparar canales.

### Enunciado

Para una imagen a color, calcula para **cada canal RGB**:
- Media
- Desviación estándar
- Percentil 10 y percentil 90

Presenta los resultados en una tabla (puede ser `print` con formato).

**Pregunta**: Si el canal G tiene una media mucho mayor que R y B, ¿qué puede indicar esto sobre el contenido de la imagen?

### Pistas

- `np.mean()`, `np.std()`, `np.percentile()`.
- Accede a cada canal con `img_rgb[:, :, i]`.

<details>
<summary>Solución</summary>

```python
import cv2
import numpy as np

img_rgb = cv2.cvtColor(cv2.imread("imagen.jpg"), cv2.COLOR_BGR2RGB)

print(f"{'Canal':<8} {'Media':>8} {'Std':>8} {'P10':>6} {'P90':>6}")
print("-" * 40)
for i, nombre in enumerate(["Rojo", "Verde", "Azul"]):
    canal = img_rgb[:, :, i].astype(np.float32)
    print(f"{nombre:<8} {canal.mean():>8.1f} {canal.std():>8.1f} "
          f"{np.percentile(canal, 10):>6.1f} {np.percentile(canal, 90):>6.1f}")

# Si G >> R y G >> B, la imagen probablemente tiene mucha vegetación.
```
</details>

---

## Ejercicio 4 — Histogramas RGB

**Objetivo**: Calcular y visualizar histogramas de los tres canales en una sola figura.

### Enunciado

1. Carga una imagen y calcula el histograma de los canales R, G y B.
2. Dibuja los tres histogramas superpuestos en una sola figura con transparencia (`alpha=0.5`).
3. Añade leyenda, etiquetas de ejes y título.
4. Repite con dos imágenes diferentes (una con mucha vegetación, otra de suelo seco) y compara los histogramas. ¿Qué diferencias observas?

### Pistas

- `cv2.calcHist([img_rgb], [i], None, [256], [0, 256])` calcula el histograma del canal `i`.
- `ax.plot(hist, color="green", alpha=0.5, label="Verde")` dibuja con transparencia.

<details>
<summary>Solución</summary>

```python
import cv2
import matplotlib.pyplot as plt

img_rgb = cv2.cvtColor(cv2.imread("imagen.jpg"), cv2.COLOR_BGR2RGB)

fig, ax = plt.subplots(figsize=(9, 4))

for i, (color, nombre) in enumerate(zip(["red", "green", "blue"], ["Rojo", "Verde", "Azul"])):
    hist = cv2.calcHist([img_rgb], [i], None, [256], [0, 256])
    ax.plot(hist, color=color, alpha=0.6, label=nombre)

ax.set_xlabel("Intensidad (0–255)")
ax.set_ylabel("Número de píxeles")
ax.set_title("Histograma RGB")
ax.legend()
plt.tight_layout()
plt.show()
```
</details>

---

## Ejercicio 5 — Recorte y manipulación de regiones

**Objetivo**: Trabajar con regiones de interés (ROI) usando indexación NumPy.

### Enunciado

1. Carga una imagen y recorta una región cuadrada de 200×200 píxeles centrada en la imagen.
2. Muestra la imagen original y el recorte en la misma figura.
3. **Bonus**: Crea una copia de la imagen original y pinta un rectángulo rojo (grosor 3 px) alrededor de la región recortada, usando `cv2.rectangle`.

### Pistas

- Para centrar: `centro_y = alto // 2`, `roi = img[centro_y-100:centro_y+100, ...]`.
- `cv2.rectangle(img_copia, (x1, y1), (x2, y2), (0, 0, 255), 3)` dibuja el rectángulo (BGR).

<details>
<summary>Solución</summary>

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

img_bgr = cv2.imread("imagen.jpg")
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)

alto, ancho = img_rgb.shape[:2]
cy, cx = alto // 2, ancho // 2

roi = img_rgb[cy-100:cy+100, cx-100:cx+100]

# Dibujar rectángulo en copia
img_marcada = img_rgb.copy()
cv2.rectangle(img_marcada, (cx-100, cy-100), (cx+100, cy+100), (255, 0, 0), 3)

fig, axes = plt.subplots(1, 3, figsize=(14, 4))
axes[0].imshow(img_rgb);     axes[0].set_title("Original")
axes[1].imshow(img_marcada); axes[1].set_title("Con ROI marcada")
axes[2].imshow(roi);         axes[2].set_title("ROI recortada")
for ax in axes:
    ax.axis("off")
plt.tight_layout()
plt.show()
```
</details>

---

## Ejercicio 6 — Normalización y conversión de tipos

**Objetivo**: Convertir entre tipos de dato y rangos de valores.

### Enunciado

1. Carga una imagen `uint8` (rango 0–255).
2. Conviértela a `float32` normalizada en rango [0.0, 1.0].
3. Verifica que el máximo sea 1.0 y el mínimo sea 0.0.
4. Aplica una corrección de brillo: suma 0.2 a todos los píxeles y recorta con `np.clip` para que no supere 1.0.
5. Vuelve a convertir a `uint8` (multiplica por 255) y guarda el resultado.
6. **Reflexión**: ¿Por qué es importante normalizar antes de operar en lugar de operar directamente sobre `uint8`?

<details>
<summary>Solución</summary>

```python
import cv2
import numpy as np

img = cv2.imread("imagen.jpg")

# Normalizar
img_float = img.astype(np.float32) / 255.0
print(f"Min: {img_float.min():.3f}, Max: {img_float.max():.3f}")

# Corrección de brillo
img_bright = np.clip(img_float + 0.2, 0.0, 1.0)

# Volver a uint8
img_uint8 = (img_bright * 255).astype(np.uint8)
cv2.imwrite("imagen_brillo.jpg", img_uint8)
print("Guardada imagen_brillo.jpg")

# Reflexión: operar sobre uint8 puede causar desbordamiento silencioso
# (255 + 10 → 9 en uint8). float32 permite operaciones seguras.
```
</details>

---

## Ejercicio integrador — Pipeline básico de análisis

**Objetivo**: Combinar todos los conceptos del módulo en un flujo de trabajo completo.

### Enunciado

Escribe un script `analisis_imagen.py` que:

1. Reciba la ruta de una imagen como argumento de línea de comandos (`sys.argv[1]`).
2. Cargue la imagen y la convierta a RGB.
3. Imprima un resumen de sus propiedades (dimensiones, tipo, media por canal).
4. Genere una figura con 6 subplots:
   - Imagen original
   - Escala de grises
   - Canal R, G, B por separado
   - Histograma de los 3 canales superpuestos
5. Guarde la figura como `resumen_<nombre_archivo>.png`.

### Criterios de evaluación

- El script debe ejecutarse sin errores con cualquier imagen `.jpg` o `.png`.
- La figura debe ser clara, con títulos y etiquetas.
- El código debe estar organizado en funciones (`cargar_imagen`, `resumen_propiedades`, `generar_figura`).

<details>
<summary>Solución de referencia</summary>

```python
import sys
import cv2
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path


def cargar_imagen(ruta):
    img_bgr = cv2.imread(ruta)
    if img_bgr is None:
        raise FileNotFoundError(f"No se pudo cargar: {ruta}")
    return cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)


def resumen_propiedades(img_rgb, ruta):
    alto, ancho, canales = img_rgb.shape
    print(f"\n=== Resumen: {ruta} ===")
    print(f"Dimensiones : {ancho}×{alto} ({canales} canales)")
    print(f"Tipo de dato: {img_rgb.dtype}")
    print(f"Memoria     : {img_rgb.nbytes / 1024 / 1024:.2f} MB")
    for i, nombre in enumerate(["Rojo", "Verde", "Azul"]):
        media = img_rgb[:, :, i].mean()
        print(f"Media {nombre:<5}: {media:.1f}")


def generar_figura(img_rgb, ruta_salida):
    fig = plt.figure(figsize=(16, 8))

    # Imagen original
    ax1 = fig.add_subplot(2, 3, 1)
    ax1.imshow(img_rgb); ax1.set_title("Original"); ax1.axis("off")

    # Escala de grises
    gris = cv2.cvtColor(img_rgb, cv2.COLOR_RGB2GRAY)
    ax2 = fig.add_subplot(2, 3, 2)
    ax2.imshow(gris, cmap="gray"); ax2.set_title("Grises"); ax2.axis("off")

    # Canales individuales
    colormaps = ["Reds", "Greens", "Blues"]
    nombres = ["Canal R", "Canal G", "Canal B"]
    for i in range(3):
        ax = fig.add_subplot(2, 3, 3 + i)
        ax.imshow(img_rgb[:, :, i], cmap=colormaps[i])
        ax.set_title(nombres[i]); ax.axis("off")

    # Histograma (en lugar de ax6, superpuesto)
    ax_hist = fig.add_subplot(2, 3, 3)
    ax_hist.set_visible(False)  # reservado para histograma si se desea expandir

    # Añadir histograma en la misma posición que "Canal B" rediseñando el layout
    # Versión simplificada: histograma como sexto subplot
    fig2, ax_hist2 = plt.subplots(figsize=(6, 3))
    for i, (color, nombre) in enumerate(zip(["red", "green", "blue"], ["R", "G", "B"])):
        hist = cv2.calcHist([img_rgb], [i], None, [256], [0, 256])
        ax_hist2.plot(hist, color=color, alpha=0.6, label=nombre)
    ax_hist2.set_title("Histograma RGB")
    ax_hist2.legend()
    plt.tight_layout()
    fig2.savefig(ruta_salida.replace(".png", "_hist.png"))

    fig.tight_layout()
    fig.savefig(ruta_salida)
    print(f"\nFigura guardada en: {ruta_salida}")


if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Uso: python analisis_imagen.py <ruta_imagen>")
        sys.exit(1)

    ruta = sys.argv[1]
    nombre = Path(ruta).stem
    img = cargar_imagen(ruta)
    resumen_propiedades(img, ruta)
    generar_figura(img, f"resumen_{nombre}.png")
```
</details>
