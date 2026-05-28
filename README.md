# PDF Splitter — Cortador de PDF por Páginas

[![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-green.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-Single--File-orange.svg)](index.html)
[![Sin instalación](https://img.shields.io/badge/Instalaci%C3%B3n-Ninguna-blue.svg)](#cómo-usar)

Herramienta web **autocontenida en un solo archivo HTML** para dividir un PDF compuesto en múltiples documentos individuales, según un catálogo de documentos totalmente configurable. Funciona **100% en el navegador** — ningún archivo se envía a ningún servidor.

---

## Interfaz visual

La aplicación sigue un flujo de 4 pasos guiado:

```
┌─────────────────────────────────────────────────────────┐
│  ✂️ PDF Splitter — Cortador de PDF por Páginas    v1.4.0 │
└─────────────────────────────────────────────────────────┘

  ① Configurar ──── ② Cargar PDF ──── ③ Verificar ──── ④ Generar

┌──────────────────────────────────────────────────────────┐
│ Paso 1 · Configuración                                   │
│  Folio / Número de Caso: [___________]                   │
│  Nombre del Titular:     [___________]                   │
│                                                          │
│  ☐ Documento Adicional A    ☐ Documento Especial B       │
│                                                          │
│  Contrato Principal [−] 10 [+]   Anexo Técnico [−] 1 [+]│
│  Total esperado: 17 páginas                              │
└──────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────┐
│ Paso 2 · Cargar PDF                                      │
│  ┌────────────────────────────────┐                      │
│  │  📄 Haz clic o arrastra el PDF │                      │
│  └────────────────────────────────┘                      │
└──────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────┐
│ Paso 3 · Vista Previa de Asignación de Páginas           │
│  # │ Documento               │ Tipo │ Págs │ Rango       │
│  01│ Solicitud Principal     │ fijo │  2   │ 1 – 2       │
│  02│ Identificación Oficial  │ fijo │  1   │ 3 – 3       │
│  …                                                       │
└──────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────┐
│ Paso 4 · Generar y Descargar                             │
│  [ ✂️ Cortar y Descargar ZIP ]                           │
└──────────────────────────────────────────────────────────┘
```

---

## Características principales

- **Sin instalación** — archivo HTML único, ábrelo en cualquier navegador moderno
- **100% local** — ningún dato sale del dispositivo del usuario
- **Catálogo configurable** — agrega, quita o modifica documentos editando el array `CATALOG`
- **Tres tipos de documentos:**
  - `fijo` — siempre se incluye con un número de páginas determinado
  - `variable` — el usuario ajusta el conteo mediante ± (stepper)
  - `condicional` — se incluye solo si el usuario marca el checkbox correspondiente
- **Verificación automática** — compara las páginas del PDF cargado con las esperadas
- **Corte paralelo** — usa `Promise.all` para dividir todos los PDFs simultáneamente
- **Identificador opcional** — folio + nombre del titular se añaden a los nombres de archivo
- **Descarga en ZIP** — todos los PDFs nombrados y ordenados en un solo archivo
- **Sin dependencias de servidor** — solo pdf-lib y JSZip vía CDN
- **Animaciones y UX** — fadeUp, ripple, confetti, auto-scroll entre pasos, tooltips

---

## Cómo usar

No requiere instalación, servidor ni conexión a internet (después de cargar la página por primera vez).

1. **Abre** `index.html` en cualquier navegador moderno (Chrome, Firefox, Edge, Safari)
2. **Paso 1 — Configura:**
   - Ingresa el folio y nombre del titular *(opcional, se usan en los nombres de archivo)*
   - Marca los documentos condicionales que apliquen al caso
   - Ajusta el número de páginas de los documentos variables con los botones ± 
3. **Paso 2 — Carga el PDF:**
   - Haz clic en la zona de carga o arrastra el archivo
   - El PDF debe contener todos los documentos escaneados en el mismo orden que el catálogo
4. **Paso 3 — Verifica:**
   - Confirma que el número de páginas del PDF coincida con las esperadas
   - Si hay diferencia, ajusta la configuración hasta que coincida
5. **Paso 4 — Genera:**
   - Haz clic en **"Cortar y Descargar ZIP"**
   - Los archivos PDF individuales se descargarán en un solo ZIP

---

## Cómo personalizar el catálogo de documentos

Edita el array `CATALOG` al inicio de la sección `<script>` en `index.html`.

### Agregar un documento fijo

```javascript
// Un documento que siempre ocupa exactamente 2 páginas
{ id: "mi_doc", nombre: "Documento_Mi_Nombre", label: "Mi Documento", pg: 2, tipo: "fijo" }
```

### Agregar un documento variable

```javascript
// Un documento cuyo número de páginas puede variar entre 3 y 5
{ id: "informe", nombre: "Documento_Informe", label: "Informe Técnico", pg: 3, tipo: "variable", varKey: "informe", min: 3, max: 5 }
```

Luego agrega `informe` al estado inicial y al stepper en HTML:

```javascript
// En el objeto S:
varPg: { contrato: 10, anexo: 1, informe: 3 }
```

```html
<!-- En la sección var-grid del HTML: -->
<div class="var-card">
  <div class="vc-label">Informe Técnico</div>
  <div class="vc-desc">Varía entre 3 y 5 páginas</div>
  <div class="stepper">
    <button type="button" class="step-btn" id="btn_informe_minus">−</button>
    <div class="step-val" id="val_informe">3</div>
    <button type="button" class="step-btn" id="btn_informe_plus">+</button>
    <span class="step-range">mín 3 / máx 5</span>
  </div>
</div>
```

```javascript
// En init(), añadir los listeners:
el("btn_informe_minus").addEventListener("click", () => changePage("informe", -1));
el("btn_informe_plus").addEventListener("click",  () => changePage("informe", +1));
```

### Agregar un documento condicional

```javascript
// Un documento que solo se incluye si el usuario lo marca
{ id: "poder", nombre: "Documento_Poder_Notarial", label: "Poder Notarial", pg: 1, tipo: "condicional", cond: "poderNotarial" }
```

Luego agrega `poderNotarial` al estado, al HTML y a `init()`:

```javascript
// En el objeto S:
conds: { docAdicionalA: false, docEspecialB: false, poderNotarial: false }
```

```html
<!-- En la sección cond-grid del HTML: -->
<div class="cond-item" id="lbl_poderNotarial">
  <input type="checkbox" id="cond_poderNotarial"/>
  <div>
    <div class="cond-name">Poder Notarial</div>
    <div class="cond-desc">Incluir si el titular actúa mediante representante</div>
  </div>
</div>
```

```javascript
// En init(), añadir "poderNotarial" al array de condicionales:
["docAdicionalA", "docEspecialB", "poderNotarial"].forEach(key => { /* ... */ });
```

---

## Dependencias externas y licencias

| Librería | Versión | Licencia | CDN |
|----------|---------|----------|-----|
| [pdf-lib](https://pdf-lib.js.org/) | 1.17.1 | MIT | cdnjs.cloudflare.com |
| [JSZip](https://stuk.github.io/jszip/) | 3.10.1 | MIT | cdnjs.cloudflare.com |

Ambas dependencias se cargan desde CDN. Para uso completamente offline, descárgalas y actualiza las etiquetas `<script src="...">` en el `<head>` del HTML.

---

## Estructura del proyecto

```
pdf-splitter/
├── index.html      # Aplicación completa (HTML + CSS + JS en un solo archivo)
├── README.md       # Este archivo
├── CHANGELOG.md    # Historial de versiones
└── LICENSE         # Texto completo de la licencia MIT
```

---

## Contribuciones

¡Los Pull Requests son bienvenidos!

Para contribuir:

1. Haz **fork** del repositorio
2. Crea una rama descriptiva: `git checkout -b feature/nombre-de-la-mejora`
3. Realiza tus cambios y haz commit con un mensaje claro
4. Abre un **Pull Request** describiendo qué cambiaste y por qué

Para cambios mayores (nueva funcionalidad, cambios en la arquitectura), abre primero un **Issue** para discutir la propuesta antes de implementarla.

---

## Licencia

Distribuido bajo la [Licencia MIT](LICENSE).  
Copyright (c) 2025 [ElMemoRexx](https://github.com/ElMemoRexx)
