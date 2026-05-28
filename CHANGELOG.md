# Changelog

Todos los cambios notables de este proyecto están documentados aquí.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es/1.0.0/),
y este proyecto adhiere a [Versionado Semántico](https://semver.org/lang/es/).

---

## [1.4.0] — 2025-05-27 — Versión pública inicial

Primer lanzamiento público bajo licencia MIT. La herramienta fue generalizada
para ser reutilizable en cualquier contexto, eliminando toda referencia específica
de dominio y refactorizando el código para seguir buenas prácticas de publicación
en GitHub.

### Añadido
- Licencia MIT completa en cabecera del HTML y en footer visible
- Ícono SVG generado en código puro (sin imágenes externas ni base64)
- Catálogo de documentos genérico y demostrativos (5 fijos, 2 variables, 2 condicionales)
- Documentación JSDoc completa en español para todas las funciones
- Comentarios de bloque explicando el propósito de cada sección
- Documentación del objeto `CATALOG` y del estado `S` con todas sus propiedades
- Comentarios inline que explican decisiones de diseño no obvias
- Footer con enlace a repositorio GitHub: `github.com/ElMemoRexx`
- README con instrucciones de uso y guía de personalización del catálogo
- CHANGELOG en formato Keep a Changelog

### Cambiado
- Nombre de la app: `"PDF Splitter — Cortador de PDF por Páginas"`
- Campos del formulario: `numExpediente` y `nombreTitular` (nombres genéricos)
- Nombre del ZIP descargado: `PDF_Splitter[_id].zip`
- Sugerencias de error en `updatePagesStatus()` generadas dinámicamente desde el CATÁLOGO
- CSS `.logo-mejoravit` renombrado a `.header-logo-svg`

### Eliminado
- Imagen base64 embebida (logo institucional, ~94 KB)
- Toda referencia a nombres de instituciones, programas o sistemas internos en código,
  comentarios, textos visibles y nombres de variables

---

## [1.3.0] — Tooltips, bloqueo de cards e identificador opcional

### Añadido
- **Tooltips CSS puro** con `data-tip` y `::after` pseudo-elemento en documentos condicionales,
  sin JavaScript adicional
- **Bloqueo progresivo de cards**: card3 se desbloquea al cargar el PDF; card4, al verificar
  páginas correctas. Previene interacciones fuera de orden
- **Identificador opcional de expediente**: folio + nombre del titular se añaden como sufijo
  `_folio_nombre` en todos los archivos PDF y en el nombre del ZIP
- Función `buildId()` que sanitiza el identificador con regex antes de usarlo en nombres
  de archivo (elimina caracteres no válidos en sistemas de archivos)
- Mensajes de estado dinámicos en el botón de corte según el progreso actual

### Cambiado
- El auto-scroll ahora lleva al paso siguiente correcto cuando las páginas coinciden

---

## [1.2.0] — Animaciones, ripple, confetti y auto-scroll

### Añadido
- **9 animaciones CSS** con `@keyframes`: `fadeUp`, `fadeIn`, `scaleIn`, `slideDown`,
  `pulse`, `spin`, `bounce`, `ripple`, `confettiFall` y `checkDraw`
- **Efecto ripple** al hacer clic en el botón principal, generado dinámicamente con JS
- **Animación confetti** de 28 partículas al completar el corte exitosamente
- **Auto-scroll suave** entre pasos: al cargar el PDF se desplaza a la verificación;
  al coincidir páginas, se desplaza al paso de generación
- **Animación checkDraw** en el ícono SVG de la caja de éxito (trazo animado)
- Indicador de progreso visual (rail) con puntos y líneas animados entre los 4 pasos
- Cards con animación `fadeUp` escalonada al cargar la página (delay por `nth-child`)
- Spinner de carga durante el parseo del PDF y el corte

### Cambiado
- Zona de carga muestra estado visual diferenciado: normal / arrastrando / cargado / error

---

## [1.1.0] — Caché de cómputo, corte paralelo y PDF cacheado

### Añadido
- **Caché de cómputo** con `_cache`, `invalidate()` y `computed()`: los rangos de páginas
  se recalculan una sola vez por ciclo de update, no en cada función de render
- **Corte paralelo con `Promise.all`**: todos los documentos PDF se generan simultáneamente,
  aprovechando las operaciones asíncronas de pdf-lib
- **`PDFDocument` cacheado en estado**: el PDF se parsea una vez en `loadPDF()` y se
  reutiliza en `cutPDF()` sin re-parsear, eliminando una operación costosa
- Barra de progreso que avanza en tiempo real conforme cada PDF individual se completa
- Función `buildSuccessNode()` que construye la caja de éxito por DOM (sin innerHTML de usuario)
- `resetDropZone()` para recuperar la zona de carga tras un error, sin handlers inline
- Helpers `escapeHtml()` y `debounce()` como utilidades de seguridad y rendimiento

### Cambiado
- Todos los handlers de eventos migrados de atributos `onX=""` en HTML a `addEventListener`
  en la función `init()`, desacoplando HTML de lógica
- `onUpdate()` deja de leer checkboxes del DOM; `S.conds` es la única fuente de verdad
- Inputs de texto con debounce de 150ms para evitar recálculos en cada pulsación

### Seguridad
- `file.name` y `e.message` ya no se inyectan en `innerHTML`; se usan nodos DOM
  con `textContent` para prevenir XSS
- Sufijo de identificador de usuario pasa por `escapeHtml()` antes de usarse en templates

---

## [1.0.0] — Versión inicial funcional

Primera versión funcional de la herramienta.

### Añadido
- Catálogo de documentos (`CATALOG`) con tipos `fijo`, `variable` y `condicional`
- Estado global `S` como fuente única de verdad de la aplicación
- Carga de PDF con `file.arrayBuffer()` y conteo de páginas con pdf-lib
- Comparación de páginas cargadas vs. esperadas con indicadores visual (ok / warn / err)
- Tabla de vista previa con la asignación documento → rango de páginas
- Corte básico y descarga de archivos PDF individuales
- Empaquetado en ZIP con JSZip
- Steppers ± para documentos con páginas variables
- Checkboxes para documentos condicionales
- Soporte drag & drop sobre la zona de carga
- Diseño responsivo con CSS Grid y Flexbox
- Variables CSS (`--primary`, `--ok`, `--warn`, `--err`, etc.) para theming centralizado

---

[1.4.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.4.0
[1.3.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.3.0
[1.2.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.2.0
[1.1.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.1.0
[1.0.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.0.0
