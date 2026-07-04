# PC4 — Control Estadístico de Procesos (CEP)

**Curso:** Control Estadístico de la Calidad — Universidad Nacional de Ingeniería (FIIS)
**Evaluación:** PC4 · *Assessment Industrial Analytics Challenge*
**Alumno:** Renato Alex Leon Saenz

---

## Descripción

Portal web estático que presenta la solución de los **6 problemas** de la PC4
(Control Estadístico de Procesos). Cada problema se resuelve con **dos herramientas
principales — Excel y Minitab —** y se documenta el razonamiento y la interpretación.
El apoyo de **IA** está declarado en `declaracionIA.md` y registrado en
`prompts/prompts.md`.

Para cada problema se muestra: identificación de la carta o índice adecuado,
desarrollo en Excel (con su gráfica de control o de capacidad), desarrollo en
Minitab, interpretación y conclusiones.

## Cómo verlo

- **Rápido:** abrir `index.html` con doble clic.
- **Recomendado** (para que funcione la carga de datos en `anexos.html`): levantar un
  servidor local desde esta carpeta y abrir la dirección que indique:

  ```bash
  python -m http.server 8080
  ```
  → http://localhost:8080

> Todas las rutas son **relativas**, así que el sitio funciona igual dentro del ZIP,
> abierto localmente o publicado en GitHub Pages.

## Estructura

```
index.html                     Portada
problema1.html … problema6.html   Un problema por página
anexos.html                    Descarga de los .xlsx de solución
css/                           Estilos del sitio
imagenes/                      Enunciados y capturas (Excel / Minitab) + formula_N / interpretacion_N
codigos/problemaN/             Archivos .xlsx de solución (descargables desde Anexos)
js/                            Interacciones (lightbox, ocultado de imágenes faltantes)
prompts/prompts.md             Registro de prompts de IA
declaracionIA.md               Declaración de uso de IA
```

> Nota: la guía oficial pide el entregable como `PC4_Apellido_Nombre.zip`. Al empaquetar
> se ordena según esa guía (p. ej. `css/`, `img/`, `excel/`, `minitab/`, `prompts/`).

## Herramientas utilizadas

- **Excel** — cálculos con fórmulas reales y **gráficas nativas** (cartas de control y
  gráficas de capacidad).
- **Minitab (Assistant)** — cartas de control y análisis de capacidad; proyecto en
  `minitab/proyecto.mpx`.
- **IA** — apoyo declarado en `declaracionIA.md` y registrado en `prompts/prompts.md`.
  La interpretación final es **propia** (revisada, verificada y sostenida por el alumno).

## Contenido de cada página de problema

Portada · Descripción del problema · Identificación de la carta · Desarrollo en Excel ·
Desarrollo en Minitab · Interpretación · Conclusiones · Bibliografía e IA utilizada.
