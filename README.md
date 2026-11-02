# Puntos de Lagrange en el sistema Sol–Tierra

Notebook de Jupyter/Colab (`puntos_de_lagrange.ipynb`) que estudia el **problema restringido de tres cuerpos circular** en el sistema Sol–Tierra. Se grafica el potencial efectivo en el sistema de referencia rotante, donde se identifican visualmente los puntos de equilibrio de Lagrange (L1–L5).

## Descripción

El Sol y la Tierra giran en órbita circular alrededor de su baricentro, y una partícula de masa despreciable se mueve bajo su influencia gravitacional. En el sistema rotante, el potencial efectivo es:

$$\Phi_{ef}(x,y) = -\frac{G\,m_\odot}{r_S} - \frac{G\,m_\oplus}{r_T} - \frac{1}{2}\,n^2\,(x^2+y^2)$$

con

$$r_S=\sqrt{(x+\mu)^2+y^2}, \qquad r_T=\sqrt{(x-(1-\mu))^2+y^2}, \qquad n=\sqrt{\frac{G(m_\odot+m_\oplus)}{a^3}}, \qquad \mu=\frac{m_\oplus}{m_\odot+m_\oplus}$$

Los puntos de Lagrange son los puntos donde $\nabla \Phi_{ef} = 0$: tres sillas de potencial colineales (L1, L2, L3) y dos máximos triangulares (L4, L5).

## Contenido del notebook

1. **Librerías**: instalación de `pymcel` e importación de dependencias.
2. **Constantes y unidades**: sistema adimensional ($G = 1$, $a = 1$, $m_\oplus = 1$).
3. **Posición de los cuerpos**: Sol en $(-\mu, 0)$ y Tierra en $(1-\mu, 0)$.
4. **Movimiento medio** $n$ (tercera ley de Kepler).
5. **Malla polar** ($r$ de 0 a 1.5, 629 ángulos) convertida a coordenadas cartesianas.
6. **Evaluación del potencial** $\Phi_{ef}$ en cada punto y reorganización en matrices.
7. **Normalización** $U = \Phi_{ef}/M$ y estadísticas del potencial (mínimo, máximo, percentiles).
8. **Curvas de nivel** del potencial efectivo, con malla refinada cerca de $r = 1$ y niveles concentrados alrededor de $U \approx -1.5$, donde se aprecian las sillas (L1, L2, L3) y los máximos (L4, L5).

## Parámetros usados

| Parámetro | Valor |
|---|---|
| $G$ | 1 |
| Distancia Sol–Tierra $a$ | 1 |
| Masa de la Tierra | 1 |
| Masa del Sol | 30 (valor ficticio) |
| $\mu$ | ≈ 0.0323 |

> **Nota:** la masa del Sol (30) **no es la real** (≈ 3×10⁵ masas terrestres). Se usa una razón de masas más moderada para que los puntos de Lagrange se distingan bien en la gráfica. Para el caso real basta cambiar `masa_sol` (aunque la visualización de las curvas de nivel requerirá ajustar los niveles).

## Requisitos

- Python 3
- `numpy`
- `scipy`
- `matplotlib`
- `celluloid`
- `plotly`
- [`pymcel`](https://pypi.org/project/pymcel/)

Instalación:

```bash
pip install -U pymcel numpy scipy matplotlib celluloid plotly
```

## Uso

**En Google Colab:** subir el notebook, abrirlo y ejecutar las celdas en orden (`Entorno de ejecución → Ejecutar todo`). La primera celda instala `pymcel` automáticamente.

**En local:**

```bash
jupyter notebook puntos_de_lagrange.ipynb
```

## Resultado

La última celda genera una figura de 9×9 con las curvas de nivel del potencial efectivo en el plano $xy$, con el Sol y la Tierra marcados en rojo sobre el eje $x$.

## Notas y estado actual

- Los imports de `solve_ivp`, `Camera` (celluloid), `plotly` y `pymcel` están disponibles pero **aún no se usan** en las celdas actuales; quedan previstos para integrar trayectorias, animaciones o gráficos interactivos.
- El notebook por ahora **grafica** el potencial e identifica visualmente los puntos de equilibrio; no calcula numéricamente sus coordenadas (por ejemplo, resolviendo $\nabla\Phi_{ef}=0$).
- Las celdas de construcción de la malla y evaluación del potencial usan bucles de Python; pueden vectorizarse con `numpy` para mayor velocidad.
- La malla polar se define dos veces (una inicial y otra refinada en la celda del gráfico); solo se usa realmente la primera (`X`, `Y`, `U`) en `contour`.

## Posibles extensiones

- Calcular numéricamente L1–L5 y compararlos con las aproximaciones analíticas (radio de Hill, $\pm 60^\circ$ para L4/L5).
- Integrar órbitas de la partícula de prueba con `solve_ivp` y animarlas con `celluloid`.
- Analizar la estabilidad de los puntos (criterio de Routh, $\mu < 0.0385$) y la constante de Jacobi.
- Usar la razón de masas real Sol–Tierra.
