# Changelog

Todos los cambios notables de este proyecto están documentados aquí.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es/1.0.0/),
y este proyecto adhiere a [Versionado Semántico](https://semver.org/lang/es/).

---

## [1.8.7] — 2026-06-17 — Rediseño UX, vista previa con miniaturas y catálogo extendido

Versión de producción con arquitectura interna refactorizada. Se eliminaron los módulos
de OCR y LLM para simplificar la herramienta; se agregó vista previa visual con PDF.js,
estado derivado con caché, soporte de múltiples tipos de identificación y catálogo
de documentos expandido.

### Añadido
- **Vista previa con miniaturas** — `PreviewService` renderiza thumbnails de cada sección
  usando PDF.js (escala 0.65×) con skeleton loader durante la carga
- **Modo tabla detallada** — segunda pestaña en la vista previa con rango, tipo y páginas
  por documento en formato tabular
- **ComputedState con caché invalidable** — docs activos, rangos y total de páginas se
  calculan una vez por ciclo de estado; `invalidate()` fuerza recómputo solo cuando cambia
- **Múltiples tipos de identificación oficial** — INE/IFE, Pasaporte, Cédula Profesional,
  Cartilla Militar, FM2; el nombre del archivo del documento se actualiza dinámicamente
- **Validación INE automática** — al cambiar a un tipo de ID distinto de INE/IFE, el
  condicional de validación se desmarca automáticamente
- **Toggle documento bancario** — Estado de Cuenta (1–4 pp.) o Contrato de Apertura
  Bancaria (1–8 pp.) resueltos dinámicamente desde `AppState.tipoDocBancario`
- **Acuse de Notificación de Inicio de Trámite** — nuevo documento condicional previo
  al Formato SIC en el orden físico del catálogo
- **Stepper para Aviso de Retención de Descuentos** — visible solo cuando el condicional
  está activo; permite ajustar entre 1 (con sello) y 2 hojas (sin sello + carta adjunta)
- **Barra de progreso animada** — avanza en fases: parseo, renderizado, empaquetado ZIP
- **Animación confetti** al completar el corte exitosamente (28 partículas, 6 colores)
- **Rail de pasos** en la cabecera con indicador de paso activo y animación pulse
- **Badge de versión** en el header
- **Timeout de protección** en `PreviewService.renderGrid()` — no bloquea si PDF.js no
  responde; muestra placeholder en lugar del thumbnail
- `smoothScroll` automático a la sección correcta tras cargar el PDF
- `escapeHtml()` aplicado a todos los valores de usuario inyectados en el DOM
- `addRipple()` para feedback táctil en botones de acción

### Cambiado
- Arquitectura migrada a tres módulos de estado: `AppState` (fuente de verdad),
  `ComputedState` (derivado con caché), `UI` (efectos y renderizado)
- Nombre del ZIP: `Expediente[_sufijo].zip`
- `CONFETTI_COLORS` movido al objeto `CONFIG` para personalización centralizada

### Eliminado
- Integración con Ollama (LLM local) y su panel de configuración
- Módulo OCR con Tesseract.js y su diccionario `OCR_KEYWORDS`
- Auto-detección de configuración por conteo de páginas

---

## [1.7.5] — 2026 — Integración Ollama y reconocimiento de documentos por IA

### Añadido
- **Integración con Ollama** (LLM local, sin costo) para identificación automática
  de documentos a partir del contenido OCR de cada sección
- Panel de configuración de URL y modelo Ollama desde la propia interfaz
- Diccionario `OCR_KEYWORDS` con palabras clave por documento para reconocimiento semántico
- Modo de identificación asistida: sugiere la asignación de secciones al catálogo

---

## [1.7.4] — 2026 — OCR, auto-detección y configuración por archivo

### Añadido
- **OCR con Tesseract.js** — extrae texto por sección para verificar el contenido
  contra el catálogo antes del corte
- **Auto-detección de configuración** — propone la configuración de páginas variables
  basándose en el total del PDF cargado
- **Configuración individual en modo lote** — cada archivo PDF puede tener su propia
  configuración de condicionales y páginas variables en una misma sesión

---

## [1.5.0] — 2025 — Estabilidad y documentos condicionales

### Añadido
- Soporte inicial para documentos condicionales con checkbox en el formulario
- Ajustes de layout y tipografía responsiva

### Cambiado
- Mejoras de estabilidad en la lectura de PDFs con páginas no estándar

---

## [1.4.0] — 2025-05-27 — Versión pública inicial

Primer lanzamiento público bajo licencia MIT. La herramienta fue generalizada
para ser reutilizable en cualquier contexto, con catálogo genérico y documentación
completa para desarrolladores.

### Añadido
- Licencia MIT completa en cabecera del HTML y en footer visible
- Catálogo de documentos genérico (5 fijos, 2 variables, 2 condicionales)
- Documentación JSDoc completa en español para todas las funciones
- Footer con enlace a repositorio GitHub
- README con instrucciones de uso y guía de personalización del catálogo
- CHANGELOG en formato Keep a Changelog

### Cambiado
- Nombre de la app: `"PDF Splitter — Cortador de PDF por Páginas"`
- Nombre del ZIP: `PDF_Splitter[_id].zip`

---

## [1.3.0] — Tooltips, bloqueo de cards e identificador opcional

### Añadido
- **Tooltips CSS puro** con `data-tip` y `::after` en documentos condicionales
- **Bloqueo progresivo de cards**: card3 se desbloquea al cargar el PDF;
  card4, al verificar páginas correctas
- **Identificador opcional de expediente**: folio + nombre del titular como
  sufijo `_folio_nombre` en todos los archivos PDF y en el ZIP
- Función `buildId()` que sanitiza el identificador con regex

### Cambiado
- Auto-scroll lleva al paso siguiente correcto cuando las páginas coinciden

---

## [1.2.0] — Animaciones, ripple, confetti y auto-scroll

### Añadido
- **9 animaciones CSS** con `@keyframes`: `fadeUp`, `fadeIn`, `scaleIn`, `slideDown`,
  `pulse`, `spin`, `bounce`, `ripple`, `confettiFall` y `checkDraw`
- **Efecto ripple** al hacer clic en el botón principal
- **Animación confetti** de 28 partículas al completar el corte
- **Auto-scroll suave** entre pasos
- Indicador de progreso visual (rail) con puntos animados entre los 4 pasos
- Spinner de carga durante el parseo del PDF y el corte

### Cambiado
- Zona de carga con estados visuales diferenciados: normal / arrastrando / cargado / error

---

## [1.1.0] — Caché de cómputo, corte paralelo y PDF cacheado

### Añadido
- **Caché de cómputo** con `_cache`, `invalidate()` y `computed()`: rangos calculados
  una vez por ciclo de update
- **Corte paralelo con `Promise.all`**: todos los PDFs se generan simultáneamente
- **`PDFDocument` cacheado**: el PDF se parsea una vez y se reutiliza sin re-parsear
- Barra de progreso en tiempo real durante el corte
- Helpers `escapeHtml()` y `debounce()` para seguridad y rendimiento

### Cambiado
- Todos los handlers migrados de atributos `onX=""` a `addEventListener` en `init()`
- `onUpdate()` lee solo `S.conds`; los checkboxes del DOM ya no son fuente de verdad
- Inputs de texto con debounce de 150ms

### Seguridad
- `file.name` y `e.message` ya no se inyectan en `innerHTML`; se usan nodos DOM con `textContent`

---

## [1.0.0] — Versión inicial funcional

### Añadido
- Catálogo de documentos con tipos `fijo`, `variable` y `condicional`
- Estado global como fuente única de verdad
- Carga de PDF y conteo de páginas con pdf-lib
- Comparación de páginas cargadas vs. esperadas
- Tabla de vista previa con asignación documento → rango de páginas
- Corte y descarga de PDFs individuales
- Empaquetado en ZIP con JSZip
- Steppers ± para documentos variables
- Checkboxes para documentos condicionales
- Soporte drag & drop en la zona de carga
- Diseño responsivo con CSS Grid y Flexbox
- Variables CSS para theming centralizado

---

[1.8.7]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.8.7
[1.7.5]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.7.5
[1.7.4]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.7.4
[1.5.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.5.0
[1.4.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.4.0
[1.3.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.3.0
[1.2.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.2.0
[1.1.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.1.0
[1.0.0]: https://github.com/ElMemoRexx/pdf-splitter/releases/tag/v1.0.0
