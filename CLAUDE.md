# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proyecto
Portal web estático educativo para visualizar el análisis estadístico del examen **PC4** de Ingeniería Industrial (UNI). Compara soluciones en **2 herramientas: Excel y Minitab**.

> El examen PC4 ya **no pide Python ni R** en la solución (solo **Excel y Minitab**). Se quitaron las columnas Python/R de las páginas y sus descargas de anexos. La antigua carpeta `codigos/` (scripts Python/R) se borró; ahora **`codigos/problemaN/` es un repositorio de los `.xlsx` de solución** que se descargan desde Anexos. El único uso restante de Python es **interno**: el flujo de `CONTROL PC4/` genera `formula_N.png` e `interpretacion_N.png` con un script; el profesor solo ve esas imágenes, nunca el código.

## Ejecución

No hay sistema de build ni gestor de paquetes. Es HTML estático puro.

- **Abrir directamente:** doble clic en `index.html`
- **Con servidor local (recomendado para que funcione `fetch()` en `anexos.html`):**
  ```powershell
  # Python
  python -m http.server 8080
  # Node (si está instalado)
  npx serve .
  ```

## Arquitectura

Sitio multipágina estático. Flujo de navegación (**6 problemas**):

```
index.html → problema1.html → … → problema6.html → anexos.html
```

La barra de navegación superior está duplicada en cada archivo HTML (no hay componente compartido), aunque su **estilo** sí está centralizado en `css/style.css` (ver «Sistema de diseño visual»). Tailwind CSS se carga vía CDN con configuración de tema extendida (Material Design 3) embebida en un `<script id="tailwind-config">` dentro de **cada** archivo HTML — si se cambia la paleta de colores, hay que actualizarla en todos los archivos.

> Las 6 páginas de problema (`problema1.html`–`problema6.html`) se generaron desde una **plantilla única** y son estructuralmente idénticas (solo cambia el número y la imagen). Si hay que tocar el layout, conviene editarlas de forma consistente o regenerarlas.

## Convenciones críticas

### Estructura de imágenes por problema

Cada problema sigue este esquema de nombres en `imagenes/problemaN/`:

```
enunciado.png        ← imagen del enunciado del problema (OBLIGATORIO)
formula_N.png        ← imagen de "Planteamiento" (objetivo/criterio) — OPCIONAL: si falta, su slot se auto-oculta
interpretacion_N.png ← imagen con interpretaciones y conclusiones (OBLIGATORIO)
excel_1.png – excel_4.png    ← capturas de Excel (máximo 4, pueden faltar algunas)
minitab_1.png – minitab_4.png ← capturas de Minitab (máximo 4, pueden faltar algunas)
datos.xlsx           ← archivo de datos para descargar en anexos.html (opcional)
```

**Importante:**
- `formula_N.png` e `interpretacion_N.png` llevan el número del problema (ej. `formula_1.png`, `interpretacion_1.png`).
- El HTML tiene **slots de hasta 4 imágenes por herramienta**, pero **no es obligatorio** que existan todas. `js/main.js` oculta silenciosamente las que faltan.
- Solo `enunciado.png` e `interpretacion_N.png` son obligatorios; **`formula_N.png` es opcional** — su sección "Planteamiento" (`data-optional-slot="formula"`) se auto-oculta si la imagen no existe, y el enunciado pasa a ancho completo.
- Cada carpeta `imagenes/problemaN/` tiene un `LEEME_capturas.txt` con el esquema de nombres.

### Layout de cada página de problema

Cada `problemaN.html` tiene tres bloques basados en imágenes:

1. **Encabezado en 2 columnas** (`grid lg:grid-cols-12`): a la izquierda **Enunciado del Problema** (`col-span-5`, `data-enunciado-slot`, `enunciado.png`), a la derecha **Planteamiento** (`col-span-7`, `data-optional-slot="formula"`, `formula_N.png`) — **opcional, se auto-oculta** si falta la imagen (y el enunciado se expande a ancho completo). El título "Planteamiento" se cambia por página según lo que pida el problema. En pantallas chicas se apilan.
2. **Grid de 2 herramientas** (Excel, Minitab) con las capturas `_1` a `_4` (`grid md:grid-cols-2`).
3. **Callout "Interpretaciones y Conclusiones"** (recuadro azul `#eef4fb` con borde `#2d7fd4`): una sola imagen `interpretacion_N.png`.

Todas las imágenes llevan la clase `img-lightbox` para ampliarse con clic.

### De dónde salen las imágenes y los archivos

Las capturas de Excel y Minitab se pegan manualmente; `formula_N.png` e `interpretacion_N.png` las genera el flujo de `CONTROL PC4/` (uso interno), concretamente el script `CONTROL PC4/<fase>/problema N/reporte_N.py`, que escribe **directamente en `imagenes/problemaN/`**. Los **`.xlsx` de solución** van en `codigos/problemaN/` para descargarse desde `anexos.html`. El sitio solo muestra las imágenes y ofrece los archivos.

> **Descarga en Anexos (convención de nombre fijo):** `anexos.html` enlaza cada Excel con `href="codigos/problemaN/solucion.xlsx"` (nombre **fijo** `solucion.xlsx`, más `download="PC4_ProblemaN_solucion.xlsx"` para el nombre amigable). Como es un sitio estático no puede listar carpetas: **el archivo debe llamarse exactamente `solucion.xlsx`** dentro de `codigos/problemaN/` o el enlace da 404. (Antes los `href` apuntaban por error a `imagenes/problemaN/datos.xlsx`; se corrigió el 2026-07-03.)

- **`formula_N.png` / `interpretacion_N.png` vienen SIN título dentro de la imagen:** el encabezado lo pone el propio HTML (el slot "Planteamiento" y el callout "Interpretaciones y Conclusiones"), que además **se cambia por problema** según lo que pida. No dupliques el título dentro de la imagen. El contenido va justificado.
- **Las capturas de Excel (`excel_*.png`) deben incluir la gráfica de control / histograma**, no solo la tabla: en este curso el Excel siempre lleva la misma gráfica que daría Minitab.

## Entregable oficial (guía del profe)

La guía oficial pide entregar un ZIP `PC4_Apellido_Nombre.zip`. Archivos ya creados en la raíz de la web:
- `README.md` — descripción, cómo verlo, estructura, herramientas (alumno: Renato Alex Leon Saenz).
- `prompts/declaracionIA.md` — herramientas + % de apoyo de IA (**el % lo pone el alumno**) + "interpretación propia". (El usuario lo movió a `prompts/`; el enlace de `anexos.html` apunta ahí. La guía lo pedía en la raíz — si se prefiere, moverlo y ajustar el enlace.)
- `prompts/prompts.md` — tabla de prompts (PROMPT · Objetivo · Respuesta IA · ¿Qué modifiqué? · Reflexión); la **Reflexión personal la escribe el alumno**.
- `minitab/` — carpeta con `LEEME.txt`; el **`proyecto.mpx` lo genera el usuario** en Minitab.
- Sección **"Bibliografía e IA utilizada"** al final de `anexos.html` (enlaza a `declaracionIA.md` y `prompts/prompts.md`).

Pendiente al empaquetar: renombrar al formato del profe (`imagenes/`→`img/`, `css/style.css`→`estilos.css`, `codigos/`→`excel/solucion.xlsx`). Ver memoria [[pc4-guia-oficial-entregable]].

## Cómo agregar / completar un problema (1 a 6)

1. Agregar las imágenes en `imagenes/problemaN/` con los nombres exactos del esquema anterior (`enunciado.png`, `formula_N.png`, `interpretacion_N.png` + capturas `excel_*` / `minitab_*`).
2. Opcionalmente `datos.xlsx` para la descarga en `anexos.html`.
3. El HTML del problema **ya existe** (`problemaN.html`) — solo hay que colocar las imágenes con los nombres correctos.

## Sistema de diseño visual (efectos estilo Cult UI)

Los efectos visuales reutilizables están centralizados en `css/style.css` (no duplicar en el HTML). Están inspirados en componentes de [Cult UI](https://cult-ui.com) y recreados en CSS/JS puro para que funcionen en el sitio estático (sin React). Cada bloque lleva un comentario `/* ... */` que lo identifica.

### Barra de navegación (dock)
`nav.fixed` recibe cristal esmerilado (`backdrop-filter: blur`) y cada enlace se eleva al pasar el mouse. Se aplica **automáticamente vía CSS** a todas las páginas: la nav sigue duplicada en cada HTML, pero su estilo no. Usa `!important` para ganarle a las clases de Tailwind. Con 6 problemas + Anexo la barra tiene scroll horizontal (`overflow-x-auto`) en pantallas chicas.

### Tarjetas de herramientas
Para que una caja de herramienta tenga barra de color + brillo al pasar el mouse, añadir al contenedor exterior:
```html
<div class="tool-card ..." data-tool="excel">   <!-- excel | minitab -->
```
- El **color** va en una barra superior (`.tool-card::before` + variable CSS `--accent`), **no** en `border-top-color` (Tailwind CDN lo pisa por especificidad de cascada).
- El brillo diagonal en hover es `.tool-card::after`; la elevación es `transform` en `:hover`.
- Colores de acento: excel `#16a34a`, minitab `#7c3aed`.

### Títulos con degradado
Cualquier encabezado `<h1>`–`<h4>` con la clase `text-[#1a2a4a]` recibe **automáticamente** el degradado azul animado (regla en `css/style.css`). La clase explícita `.gradient-title` produce el mismo efecto.

### Portada (`index.html`)
Es autocontenida: su CSS va en un `<style>` propio (no usa el tema Tailwind de las demás páginas). Efectos: título con degradado animado, contadores animados (`.stat-num` con atributo `data-target`: 2 herramientas, 6 problemas, 12 análisis), botón con barrido de brillo, borde luminoso giratorio (`@property --beam-angle`) y orbes de luz flotantes.

### Reglas a tener en cuenta
- **Tailwind se inyecta en runtime** (Play CDN), por eso los estilos custom de `style.css` necesitan `!important` o mayor especificidad para ganar la cascada.
- Todo respeta `prefers-reduced-motion` (desactiva animaciones para quien lo prefiera).
- Tras editar `css/style.css`, forzar recarga del navegador con **Ctrl + Shift + R** (el navegador cachea el CSS).

## Dependencias implícitas

No hay `requirements.txt` ni `package.json`. Las dependencias son:

| Herramienta | Para qué |
|-------------|----------|
| Browser     | Tailwind CSS, Google Fonts, Material Symbols (todos via CDN) |
| Python 3    | Solo en `CONTROL PC4/` (flujo interno): `openpyxl` (genera los `.xlsx`), `matplotlib` + `numpy` (generan `formula_N.png` / `interpretacion_N.png`) |

## Estado actual del proyecto

- **Páginas:** `index.html`, `problema1.html`–`problema6.html`, `anexos.html` listas (2 herramientas, 6 problemas).
- **✅ Fase de prueba CERRADA (2026-07-03):** Ej1–3 resueltos y **verificados contra Minitab** (coinciden). Ya tienen sus `formula_N.png` e `interpretacion_N.png` en la web. **Faltan las capturas manuales** que pega el usuario: `enunciado.png`, `excel_*.png`, `minitab_*.png` (ya hay `minitab_1.png` en problema1). El `main.js` oculta las que falten.
- **Problemas 4–6:** NO se hicieron (la prueba se cerró con 3). Sus carpetas `imagenes/problema4..6/` están vacías (solo `LEEME_capturas.txt`) → esas páginas saldrán casi vacías hasta que se resuelvan en el examen real.
