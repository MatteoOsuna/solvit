# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project: SolvIT

SolvIT is a Managed Marketplace for home technical services (plomería, electricidad, cerrajería, etc.) targeting CABA and GBA Zona Norte, Argentina. The repo contains two independent HTML apps and supporting assets — no build step, no package manager, no framework.

## Files

| File | Purpose |
|---|---|
| `SolvIT-Pitch.html` | Investor pitch deck (7 slides, self-contained) |
| `index.html` | MVP: Panel de Control for registering service providers (Supabase backend) |
| `SolvIT.pdf` | Full business plan — source of truth for brand, copy, and market data |
| `ossuna.png` | Official SolvIT logo image |
| `qr para el mvp.png` | QR code pointing to `solvitpanel.vercel.app` |
| `video de la app_final.mp4` | Demo video used in slide 3 phone mockup |
| `obi won waving STICKER.gif` | Pixel art character (Habbo-style) used in slide 4 as persona avatar |

---

## REGLA CRÍTICA — Ediciones quirúrgicas

**Cuando el usuario pide cambiar UNA slide, SOLO tocar el CSS y HTML de ESA slide.**
Nunca modificar componentes globales, otras slides, ni el bloque `<script>` salvo que se pida explícitamente.

Antes de editar, siempre leer el bloque exacto a modificar con `Read` usando `offset` y `limit`.
Nunca usar `replace_all: true` salvo que el string a reemplazar sea 100% exclusivo de la slide indicada.

---

## SolvIT-Pitch.html — Arquitectura

### Estructura del archivo (orden en el HTML)

```
<style>          ← TODO el CSS inline (líneas ~8–860)
</style>
<body>
  <!-- UI global: progress-bar, nav-dots, notes-panel, edit-banner -->
  <!-- SLIDE 1 -->  <section class="slide slide-hook"    id="slide-1">
  <!-- SLIDE 2 -->  <section class="slide slide-data"    id="slide-2">
  <!-- SLIDE 3 -->  <section class="slide slide-demo"    id="slide-3">
  <!-- SLIDE 4 -->  <section class="slide slide-company" id="slide-4">
  <!-- SLIDE 5 -->  <section class="slide slide-market"  id="slide-5">
  <!-- SLIDE 6 -->  <section class="slide slide-closing" id="slide-6">
  <!-- SLIDE 7 -->  <section class="slide slide-thanks"  id="slide-7">
<script>         ← Controlador JS (SlidePresentation, NotesController, EditController)
```

### CSS global (NO modificar salvo que se pida)

| Bloque | Qué hace |
|---|---|
| `:root` | Variables de color, tipografía y espaciado de toda la presentación |
| `.slide` | Base de cada diapositiva: `100vw × 100dvh`, `overflow: hidden`, scroll-snap |
| `.slide-content` | Contenedor flexible dentro de cada slide |
| `.accent-line` | Línea mint horizontal en el borde superior de cada slide |
| `.slide-header` | Logo + número de slide |
| `.logo-mark` / `.logo-icon` / `.logo-text` | Logo "S" + texto "SolvIT" en el header |
| `.reveal` / `.reveal-scale` | Animaciones de entrada (opacity + translateY). Activadas por `.visible` que añade el JS |
| `.nav-dots`, `.progress-bar`, `.notes-panel`, `.edit-*` | UI superpuesta — nunca tocar |
| Media queries `@media (max-height: 700/600/500px)` | Responsividad vertical — nunca tocar |

### Elemento decorativo compartido: `.pipes-scene`

Slides 1 y 7 tienen un `<div class="pipes-scene">` con un SVG de caños. La clase CSS `.pipes-scene` está definida globalmente. **Los IDs de los gradientes SVG deben ser únicos por slide:**
- Slide 1 usa prefijo `pk` (`pkV`, `pkH`, `pkJ`, `pkHiV`, `pkHiH`)
- Slide 7 usa prefijo `t7` (`t7V`, `t7H`, `t7J`, `t7HiV`, `t7HiH`, `t7StL`, `t7StR`)

En SVG, los elementos se pintan en orden de documento (el último queda encima). Los círculos de codo deben estar **al final** de cada `<g>` de caño para quedar encima de ambos tramos.

---

## Mapa de cada slide (estado actual)

### Slide 1 — Hook (`slide-hook`)
- **CSS exclusivo:** `.slide-hook`, `.hook-content`, `.hook-question`, `.hook-sub`, `.pipes-scene`
- **Contenido:** Pregunta retórica grande centrada + subtítulo
- **Decoración:** SVG de caños rotos con gotas animadas (`.pda`–`.pdf`, animación `wDrip`)
- **Footer:** "SolvIT — Pitch para inversores 2026" | "Tu hogar en orden, tu vida en paz."

### Slide 2 — Datos del Problema (`slide-data`)
- **CSS exclusivo:** `.slide-data`, `.s2-layout`, `.s2-left`, `.s2-headline`, `.s2-quotes`, `.s2-quote`, `.s2-right`, `.s2-stat`, `.s2-num`, `.s2-label`, `.s2-src`
- **Contenido:** Grid 2 columnas — izquierda: titular + 3 citas externas | derecha: 3 stats (75%, 60%, 58%)
- **Header:** Logo SolvIT + "02 / 07"
- **Footer:** "Datos verificables disponibles a solicitud"

### Slide 3 — Demo MVP (`slide-demo`)
- **CSS exclusivo:** `.slide-demo`, `.demo-layout`, `.demo-info`, `.demo-title`, `.demo-steps`, `.demo-step`, `.step-num`, `.step-text`, `.phone-wrap`, `.phone-frame`, `.phone-video`, `.phone-ph`
- **Contenido:** Grid 2 columnas — izquierda: 3 pasos del flujo | derecha: phone mockup con video
- **Video:** `src="video de la app_final.mp4"` (autoplay, muted, loop)
- **Logo watermark:** `ossuna.png` posicionado absolute en fondo inferior izquierdo
- **Header:** Logo + "03 / 07"

### Slide 4 — Cliente Ideal (`slide-company` clase CSS, contenido `.s4-*`)
- **CSS exclusivo:** `.s4-layout`, `.s4-persona`, `.s4-avatar-wrap`, `.s4-glow`, `.s4-avatar`, `.s4-avatar-label`, `.s4-chips`, `.s4-chip`, `.s4-quote`, `.s4-right`, `.s4-eyebrow`, `.s4-title`, `.s4-rule`, `.s4-list`, `.s4-item`, `.s4-num`, `.s4-item-body`, `.s4-item-label`, `.s4-item-val`, `.s4-item-sub`
- **Contenido:** Grid asimétrico 36%/64% — izquierda: avatar pixel art + chips demográficos + cita | derecha: 4 atributos en lista editorial con números fantasma (01–04)
- **Avatar:** `src="obi won waving STICKER.gif"` con `image-rendering: pixelated`
- **Nota:** Usa la clase `slide-company` en la sección para heredar el fondo, pero todo el layout real usa clases `.s4-*`
- **Header:** Sin logo (solo "04 / 07")

### Slide 5 — TAM SAM SOM (`slide-market`)
- **CSS exclusivo:** `.slide-market`, `.market-layout`, `.market-img-area`, `.concentric-circles`, `.cc-ring`, `.cc-tam/sam/som`, `.cc-label`, `.cc-center`, `.market-data`, `.market-tiers`, `.market-tier`, `.tier-header`, `.tier-acro`, `.tier-value`, `.tier-bullets`, `.market-footnote`
- **Contenido:** Grid 2 columnas — izquierda: círculos concéntricos CSS (TAM/SAM/SOM) | derecha: datos en 3 tiers
- **Números:** TAM 180M hogares · SAM ~15.7M hogares · SOM ~500K hogares
- **Ajuste visual:** `.market-data` tiene `margin-left` negativo para acercar el texto al gráfico
- **Header:** Logo + "05 / 07"

### Slide 6 — Cierre (`slide-closing`)
- **CSS exclusivo:** `.slide-closing`, `.closing-layout`, `.closing-text`, `.closing-quote`, `.closing-sub`, `.contact-row`, `.contact-item`, `.contact-sep`, `.qr-area`, `.qr-box`, `.qr-label`, `.qr-url`
- **Contenido:** Grid 2 columnas — izquierda: cita de cierre + datos de contacto | derecha: QR al MVP
- **QR:** `src="qr para el mvp.png"` → `solvitpanel.vercel.app`. El `.qr-box` tiene `box-shadow` multicapa para efecto flotante.
- **Header:** Logo + "06 / 07"

### Slide 7 — Muchas Gracias (`slide-thanks`)
- **CSS exclusivo:** `.slide-thanks`, `.thanks-content`, `.thanks-word`, `.thanks-tagline`
- **Contenido:** "Muchas Gracias." centrado + slogan en italic
- **Decoración:** SVG de caños arreglados con chorro continuo animado (`.t7ba/bb`, `.t7f1`–`.t7f6`, animaciones `t7Pulse` y `t7Flow`) — misma posición que slide 1
- **Background:** Gradiente radial + grilla CSS sutil
- **Header:** Logo + "07 / 07"

---

## Runtime controls

- `→` / `Space` — siguiente slide
- `N` — panel de notas del presentador (cada slide tiene `data-notes` en el `<section>`)
- `E` — modo edición inline (edits guardados en `localStorage`)
- `Ctrl+S` — exportar HTML editado a archivo

---

## Brand & Copy

Siempre consultar `SolvIT.pdf` para copy autoritativo. Datos clave:
- **Slogan:** "Tu hogar en orden, tu vida en paz."
- **Elevator pitch:** "Usamos IA para que dejes de sufrir con los arreglos de tu casa. Diagnosticás con una foto, pagás precio cerrado y te enviamos un experto verificado. Cero estrés."
- **Tipografía:** Sora (700/800 display) — única fuente, sin fallbacks
- **Colores:** `--accent-mint: #99DDC8` · `--accent-green: #659B5E` · `--bg-deep: #0a141e` · `--bg-primary: #0d1a24`
- **Equipo:** CEO Matteo · UX Colo & Anna · Dev Var · Mkt Rocio
- **Cliente ideal:** 26–33 años, CABA + Zona Norte, NSE Medio–Medio Alto, pago digital
- **Mercado:** TAM 180M hogares LATAM · SAM ~15.7M hogares Argentina · SOM ~500K hogares CABA+GBA Norte ABC1/C2

---

## index.html — MVP Panel

Single-file Supabase client app. Gestión de registro de prestadores de servicios.

**Backend:** Supabase (PostgreSQL). Credenciales hardcodeadas cerca de líneas ~430-431 — `SUPABASE_URL` y `SUPABASE_KEY` (anon public key).

**Tabla:** `personas` — perfiles de proveedores (nombre, edad, profesión, disponibilidad horaria, teléfono, verificación).

**Deployed at:** `solvitpanel.vercel.app`
