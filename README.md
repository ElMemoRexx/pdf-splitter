# PDF Splitter — Cortador de PDF por Páginas

[![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-green.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-Single--File-orange.svg)](pdf-splitter.html)
[![Versión](https://img.shields.io/badge/Versi%C3%B3n-1.8.7-blue.svg)](CHANGELOG.md)
[![Sin instalación](https://img.shields.io/badge/Instalaci%C3%B3n-Ninguna-lightgrey.svg)](#cómo-usar)

Herramienta web **autocontenida en un solo archivo HTML** para dividir un PDF compuesto en múltiples documentos individuales, según un catálogo de documentos totalmente configurable. Funciona **100% en el navegador** — ningún archivo se envía a ningún servidor.

---

## Interfaz visual

La aplicación sigue un flujo de 4 pasos guiado con indicador de progreso:

```
┌──────────────────────────────────────────────────────────────┐
│  ✂️  Cortador de Expediente Documental              v1.8.7   │
│      Procesamiento 100% local · ningún archivo sale del equipo│
└──────────────────────────────────────────────────────────────┘

  ① Configurar ──── ② Cargar PDF ──── ③ Verificar ──── ④ Generar

┌──────────────────────────────────────────────────────────────┐
│ Paso 1 · Configuración                                       │
│  Número de Folio:   [___________]                            │
│  Nombre del Titular:[___________]                            │
│                                                              │
│  Tipo de ID:  ● INE/IFE  ○ Pasaporte  ○ Cédula  ○ Cartilla  │
│                                                              │
│  ☐ Documento Condicional A   ☐ Documento Condicional B       │
│                                                              │
│  Doc. Variable [−] 17 [+]   Anexo [−] 1 [+]                 │
│  Total esperado: 42 páginas                                  │
└──────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────┐
│ Paso 2 · Cargar PDF                                          │
│  ┌──────────────────────────────────┐                        │
│  │  📄  Haz clic o arrastra el PDF  │                        │
│  └──────────────────────────────────┘                        │
└──────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────┐
│ Paso 3 · Vista Previa                    🖼 Miniaturas  📋 Tabla│
│                                                              │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐                     │
│  │  01  │  │  02  │  │  03  │  │  04  │  …                   │
│  │ [img]│  │ [img]│  │ [img]│  │ [img]│                      │
│  │Doc A │  │Doc B │  │Doc C │  │Doc D │                      │
│  │Págs 1│  │Págs 2│  │Págs 3│  │Págs 4│                      │
│  └──────┘  └──────┘  └──────┘  └──────┘                     │
└──────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────┐
│ Paso 4 · Generar y Descargar                                 │
│  [ ✂️  Cortar y Descargar ZIP ]                              │
│  ████████████████████░░░░  Empaquetando ZIP...               │
└──────────────────────────────────────────────────────────────┘
```

---

## Características principales

- **Sin instalación** — archivo HTML único, ábrelo en cualquier navegador moderno
- **100% local** — ningún dato sale del dispositivo; todo el procesamiento ocurre en el navegador
- **Vista previa con miniaturas** — renderiza thumbnails de cada sección con PDF.js antes de cortar
- **Catálogo configurable** — agrega, quita o modifica documentos editando el array `CATALOG`
- **Tres tipos de documentos:**
  - `fijo` — siempre se incluye con un número de páginas determinado
  - `variable` — el usuario ajusta el conteo mediante steppers ±
  - `condicional` — se incluye solo si el usuario marca el checkbox correspondiente
- **Múltiples tipos de identificación** — INE/IFE, Pasaporte, Cédula Profesional, Cartilla Militar, FM2
- **Toggle de documento bancario** — Estado de Cuenta (1–4 pp.) o Contrato de Apertura Bancaria (1–8 pp.)
- **ComputedState con caché** — rangos y totales se recalculan solo cuando cambia el estado
- **Corte paralelo** — usa `Promise.all` para dividir todos los PDFs simultáneamente
- **Identificador opcional** — folio + nombre del titular se añaden como sufijo a los nombres de archivo
- **Descarga en ZIP** — todos los PDFs nombrados y ordenados en un solo archivo comprimido
- **Verificación automática** — compara páginas cargadas contra páginas esperadas
- **UX animada** — fadeUp, ripple, confetti, skeleton loader, auto-scroll entre pasos
- **Sin dependencias de servidor** — solo pdf-lib, JSZip y PDF.js vía CDN

---

## Cómo usar

No requiere instalación, servidor ni conexión a internet después de cargar la página por primera vez.

1. **Abre** `pdf-splitter.html` en cualquier navegador moderno (Chrome, Firefox, Edge, Safari)
2. **Paso 1 — Configura:**
   - Ingresa el folio y nombre del titular *(opcional — se usan en los nombres de archivo)*
   - Selecciona el tipo de identificación oficial del titular
   - Marca los documentos condicionales que apliquen al caso
   - Ajusta el número de páginas de los documentos variables con los botones ±
3. **Paso 2 — Carga el PDF:**
   - Haz clic en la zona de carga o arrastra el archivo
   - El PDF debe contener todos los documentos escaneados en el mismo orden que el catálogo
4. **Paso 3 — Verifica:**
   - Revisa las miniaturas o la tabla detallada para confirmar la asignación de páginas
   - Si el conteo no coincide, ajusta la configuración hasta que el indicador esté en verde
5. **Paso 4 — Genera:**
   - Haz clic en **"Cortar y Descargar ZIP"**
   - La barra de progreso indicará el avance; al terminar se descargará el ZIP automáticamente

---

## Cómo personalizar el catálogo de documentos

Edita el array `CATALOG` dentro del bloque `CONFIG` en `pdf-splitter.html`.

### Documento fijo

```javascript
{ id: "mi_doc", nombre: "Documento_Mi_Nombre", label: "Mi Documento", pg: 2, tipo: "fijo" }
```

### Documento variable

```javascript
{
  id: "informe", nombre: "Documento_Informe", label: "Informe Técnico",
  pg: 3, tipo: "variable", varKey: "informe", min: 3, max: 5
}
```

Agrega la clave `informe` en `AppState.varPg` y su stepper en el HTML:

```html
<div class="var-card">
  <div class="vc-label">Informe Técnico</div>
  <div class="vc-desc">Varía entre 3 y 5 páginas</div>
  <div class="stepper">
    <button class="step-btn" id="btn_informe_minus" onclick="UI.changePage('informe',-1)" disabled>−</button>
    <div class="step-val" id="val_informe">3</div>
    <button class="step-btn" id="btn_informe_plus"  onclick="UI.changePage('informe',+1)">+</button>
    <span class="step-range">mín 3 / máx 5</span>
  </div>
</div>
```

### Documento condicional

```javascript
{
  id: "poder", nombre: "Documento_Poder_Notarial",
  label: "Poder Notarial", pg: 1, tipo: "condicional", cond: "poderNotarial"
}
```

Agrega `poderNotarial: false` en `AppState.conds` y su checkbox en el HTML:

```html
<label class="cond-item" id="lbl_poderNotarial">
  <input type="checkbox" id="cond_poderNotarial" onchange="UI.toggleCond('poderNotarial',this.checked)"/>
  <div>
    <div class="cond-name">Poder Notarial</div>
    <div class="cond-desc">Incluir si el titular actúa mediante representante legal</div>
  </div>
</label>
```

---

## Dependencias externas y licencias

| Librería | Versión | Licencia | Uso |
|----------|---------|----------|-----|
| [pdf-lib](https://pdf-lib.js.org/) | 1.17.1 | MIT | Manipulación y extracción de páginas PDF |
| [JSZip](https://stuk.github.io/jszip/) | 3.10.1 | MIT | Empaquetado ZIP con compresión DEFLATE |
| [PDF.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | Apache 2.0 | Renderizado de miniaturas en canvas |

Todas las dependencias se cargan desde CDN. Para uso completamente offline, descárgalas y actualiza las etiquetas `<script src="...">` en el `<head>` del archivo HTML.

---

## Estructura del proyecto

```
pdf-splitter/
├── pdf-splitter.html   # Aplicación v1.8.7 (HTML + CSS + JS en un solo archivo)
├── index.html          # Versión v1.4.0 — base pública inicial
├── README.md           # Este archivo
├── CHANGELOG.md        # Historial de versiones
└── LICENSE             # Texto completo de la licencia MIT
```

---

## Contribuciones

¡Los Pull Requests son bienvenidos!

Para contribuir:

1. Haz **fork** del repositorio
2. Crea una rama descriptiva: `git checkout -b feature/nombre-de-la-mejora`
3. Realiza tus cambios y haz commit con un mensaje claro
4. Abre un **Pull Request** describiendo qué cambiaste y por qué

Para cambios mayores (nueva funcionalidad, cambios de arquitectura), abre primero un **Issue** para discutir la propuesta antes de implementarla.

---

## Licencia

Distribuido bajo la [Licencia MIT](LICENSE).  
Copyright (c) 2025 [ElMemoRexx](https://github.com/ElMemoRexx)
