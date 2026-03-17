# 📋 Informe de Trazabilidad
## Dashboard Previsión Precio Crudo Brent 2026

> **Proyecto:** `fuel-forecast`
> **Fecha de inicio:** 17 de marzo de 2026
> **Estado actual:** ✅ Producción — v1.3
> **Archivo principal:** `index.html`

---

## 1. Contexto y Objetivo

### 1.1 Necesidad detectada
Analizar y visualizar la previsión del precio del combustible (crudo Brent) a
corto plazo en el contexto del conflicto armado entre EE.UU./Israel e Irán
iniciado el 28 de febrero de 2026, que provocó una subida del ~40% en el precio
del crudo en menos de dos semanas.

### 1.2 Objetivo del proyecto
Crear un dashboard web público, accesible desde cualquier navegador, que muestre:
- El precio real del Brent actualizado automáticamente
- La previsión del mercado (consenso institucional)
- Dos escenarios de análisis propio (bajista y alcista)
- Visualización comparada en gráfica precio-tiempo de granularidad quincenal

---

## 2. Fuentes de Datos y Análisis

### 2.1 Fuentes consultadas para el análisis
| Fuente | Tipo | Uso |
|--------|------|-----|
| EIA Short-Term Energy Outlook | Institucional | Previsión base de mercado |
| Goldman Sachs Commodities | Banco de inversión | Precio objetivo Q2 2026 |
| IEA Oil Market Report | Institucional | Contexto oferta/demanda |
| Bloomberg (referencia) | Financiero | Datos spot históricos |
| stooq.com | API pública | Precio en tiempo real (BZ.F) |

### 2.2 Datos clave del análisis
| Fecha | Precio Brent | Evento |
|-------|-------------|--------|
| 1 Ene 2026 | $65/b | Inicio de año, mercado estable |
| 15 Ene 2026 | $67/b | Sin cambios relevantes |
| 1 Feb 2026 | $70/b | Tensiones geopolíticas latentes |
| 15 Feb 2026 | $72/b | Escalada diplomática |
| 1 Mar 2026 | $79/b | Días previos al conflicto |
| 28 Feb 2026 | ~$120/b | ⚡ **PICO** — Inicio ataque EE.UU./Israel a Irán |
| 15 Mar 2026 | ~$102/b | Retroceso parcial, mercado digiriendo el shock |

### 2.3 Escenarios de previsión
| Escenario | Color | Prob. estimada | Precio dic 2026 | Hipótesis |
|-----------|-------|---------------|-----------------|-----------|
| Mercado (EIA/GS/IEA) | Azul `#4a9eff` | ~45% | $64/b | OPEC+ sube producción, sin escalada |
| Bajista (propio) | Verde `#2ecc8e` | ~25% | $54/b | Paz negociada + recesión global |
| Alcista (propio) | Naranja `#ff7143` | ~30% | >$100/b | Cierre Ormuz / escalada a Arabia Saudí |

---

## 3. Historial de Versiones

### v1.0 — Borrador inicial (turn 1-3)
**Descripción:** Primeros intentos de crear el artefacto en formato React/JSX.

**Problema:** El artefacto React no se renderizó correctamente en la interfaz
de Claude.ai. Se intentaron múltiples variaciones del código sin resultado visible.

**Decisión:** Abandonar React y migrar a HTML puro con Chart.js vía CDN.

---

### v1.1 — Primera versión HTML funcional
**Archivo:** `fuel_forecast.html`
**Cambios implementados:**
- Migración completa de React/JSX a HTML5 + CSS3 + JS vanilla
- Integración de Chart.js 4.4.1 desde CDN de Cloudflare
- Gráfica con granularidad **mensual** (13 puntos: Ene–Dic + split de Mar)
- Labels: `["Ene","Feb","Mar-i","Mar-f","Abr","May","Jun","Jul","Ago","Sep","Oct","Nov","Dic"]`
- Plugin personalizado `warLinePlugin` para línea vertical roja en "Mar-i"
- Barra de precio en vivo con llamada directa a `stooq.com`
- 4 tarjetas resumen con colores temáticos
- Diseño oscuro con paleta navy/gold
- Botones de filtro para mostrar/ocultar escenarios

**Estado al terminar:** ✅ Funcional localmente

---

### v1.2 — Corrección CORS + sistema de proxies
**Archivo:** `index.html` (renombrado para compatibilidad con GitHub Pages)

**Problema detectado:** Al abrir el archivo en el navegador, el fetch a `stooq.com`
fallaba con el error:
```
Warning: Live price fetch failed: Error: Failed to fetch
```
**Causa raíz:** Política CORS (Cross-Origin Resource Sharing). El navegador bloquea
peticiones desde una página web estática a dominios externos que no incluyen
la cabecera `Access-Control-Allow-Origin: *` en su respuesta. stooq.com no la incluye.

**Solución implementada:** Cascada de 3 intentos en orden de preferencia:

```
Intento 1 → corsproxy.io (proxy gratuito, sin API key, CORS permitido)
              URL: https://corsproxy.io/?url=<stooq_url_encoded>

Intento 2 → allorigins.win (proxy de respaldo, también gratuito y sin registro)
              URL: https://api.allorigins.win/raw?url=<stooq_url_encoded>

Intento 3 → Dato estático de referencia ($102, 16 Mar 2026)
              Muestra aviso visual "⚠ Datos en vivo no disponibles ahora"
```

**Funciones JavaScript añadidas:**
- `updateUI(price, open, dateStr, source)` — centraliza la actualización del DOM y la gráfica
- `parseStooqCSV(text)` — parsea el CSV de stooq (columnas: Symbol, Date, Time, Open, High, Low, Close, Volume)
- `tryCorsproxy()` — intento con proxy 1
- `tryAllOrigins()` — intento con proxy 2
- `fetchLivePrice()` — orquesta la cascada de intentos con try/catch anidados

**Refresco automático:** `setInterval(fetchLivePrice, 5 * 60 * 1000)` (cada 5 min)

**Estado al terminar:** ✅ Funcional con proxy CORS activo

---

### v1.3 — Granularidad quincenal (versión actual)
**Archivo:** `index.html`

**Cambio solicitado:** Aumentar la granularidad de mensual a quincenal (cada 15 días).

**Cambios técnicos:**
- Labels: 24 puntos en lugar de 13 (de "1 Ene" a "15 Dic")
- Arrays de datos actualizados a 24 elementos cada uno
- Índice del punto "hoy" en `realData`: cambia de `[3]` a `[5]` (= "15 Mar")
- Plugin `warLinePlugin`: búsqueda cambia de `'Mar-i'` → `'15 Mar'`
- Inyección del precio en vivo: `myChart.data.datasets[0].data[5]` (antes `[3]`)

**Nuevos arrays de datos:**
```js
// 24 puntos, granularidad quincenal
labels = ["1 Ene","15 Ene","1 Feb","15 Feb","1 Mar","15 Mar",
          "1 Abr","15 Abr","1 May","15 May","1 Jun","15 Jun",
          "1 Jul","15 Jul","1 Ago","15 Ago","1 Sep","15 Sep",
          "1 Oct","15 Oct","1 Nov","15 Nov","1 Dic","15 Dic"]
```

**Estado al terminar:** ✅ Versión actual en producción

---

## 4. Arquitectura Técnica

### 4.1 Estructura del archivo
```
index.html
├── <head>
│   ├── Chart.js 4.4.1 (CDN Cloudflare)
│   └── Google Fonts: DM Mono + Syne
├── <body>
│   ├── .header          — Título y subtítulo
│   ├── .live-bar        — Precio en tiempo real
│   ├── .filters         — Botones de filtro
│   ├── .chart-box       — Canvas Chart.js
│   ├── .cards           — 4 tarjetas resumen
│   └── .footer          — Fuentes + disclaimer
└── <script>
    ├── Datos (labels + 4 arrays)
    ├── warLinePlugin (plugin Chart.js personalizado)
    ├── myChart (instancia Chart.js)
    ├── setScenario() (lógica de filtros)
    └── fetchLivePrice() + helpers CORS
```

### 4.2 Dependencias externas
| Librería | Versión | CDN | Uso |
|----------|---------|-----|-----|
| Chart.js | 4.4.1 | cdnjs.cloudflare.com | Gráfica de líneas |
| DM Mono | 400, 500 | fonts.googleapis.com | Fuente monoespaciada (cuerpo) |
| Syne | 700, 800 | fonts.googleapis.com | Fuente display (títulos) |

### 4.3 APIs y proxies
| Servicio | Rol | Autenticación | Límites conocidos |
|----------|-----|--------------|-------------------|
| stooq.com (BZ.F) | Precio Brent en tiempo real | Sin API key | Sin documentación oficial de límites |
| corsproxy.io | Proxy CORS primario | Sin API key | Posible rate limiting en uso intensivo |
| allorigins.win | Proxy CORS secundario | Sin API key | Posible latencia variable |

---

## 5. Instrucciones de Mantenimiento

### 5.1 Actualización quincenal de datos reales
Cada ~15 días, editar el array `realData` en `index.html`:

```js
// Ejemplo: al llegar el 1 de abril de 2026
// Antes:
null, null,   // índices 6-7 (1 Abr, 15 Abr)

// Después (si el precio el 1 Abr es $91):
91, null,     // 1 Abr = dato real, 15 Abr = pendiente
```

Subir el archivo actualizado al repositorio de GitHub. GitHub Pages se actualiza
automáticamente en 1-2 minutos.

### 5.2 Ajuste de previsiones
Si el consenso de mercado cambia significativamente, editar `mktData`, `bearData`
y/o `bullData` con los nuevos valores. Actualizar también las tarjetas resumen en el HTML.

### 5.3 Monitorización de los proxies CORS
Si el precio en vivo deja de cargarse, verificar en la consola del navegador
(F12 → Console) qué proxy está fallando. En ese caso:
- Comprobar disponibilidad de corsproxy.io y allorigins.win
- Si ambos fallan permanentemente, sustituir por alternativas como:
  - `https://api.codetabs.com/v1/proxy?quest=<url>`
  - `https://thingproxy.freeboard.io/fetch/<url>`

---

## 6. Publicación en GitHub Pages

### 6.1 Pasos de publicación inicial
1. Crear repositorio en [github.com/new](https://github.com/new)
   - Nombre sugerido: `fuel-forecast`
   - Visibilidad: Public (necesario para GitHub Pages gratuito)
   - Marcar "Add a README file"
2. Subir `index.html`: botón "Add file" → "Upload files" → Commit
3. Activar Pages: Settings → Pages → Deploy from branch → main → / (root) → Save
4. Esperar 1-2 minutos. La URL pública será:
   `https://TU-USUARIO.github.io/fuel-forecast/`

### 6.2 Actualización del sitio
Para cada actualización futura:
- Ir al repositorio en GitHub
- Clicar sobre `index.html` → botón editar (lápiz) o "Upload files"
- Subir la nueva versión → Commit changes
- El sitio se actualiza automáticamente en ~60 segundos

---

## 7. Decisiones de Diseño Documentadas

| Decisión | Alternativa descartada | Motivo |
|----------|----------------------|--------|
| HTML puro + Chart.js | React/JSX | React no se renderizó correctamente en el entorno |
| Archivo único `index.html` | Múltiples ficheros | Simplicidad de despliegue en GitHub Pages |
| stooq.com como fuente | Yahoo Finance, Alpha Vantage | Sin API key requerida |
| Proxy CORS en cascada | API con key propia | Evitar registro y coste |
| Granularidad quincenal | Mensual / semanal | Mejor resolución que mensual, menos ruido que semanal |
| Fondo oscuro navy | Fondo claro | Legibilidad de datos financieros, aspecto profesional |
| DM Mono + Syne | Inter / Roboto | Carácter diferenciado, aspecto de terminal financiero |

---

## 8. Registro de Problemas y Soluciones

| # | Problema | Causa | Solución | Versión |
|---|---------|-------|----------|---------|
| 1 | Artefacto React invisible | Fallo de renderizado en Claude.ai | Migrar a HTML puro | v1.1 |
| 2 | `Failed to fetch` en stooq | Política CORS del navegador | Proxies CORS en cascada | v1.2 |
| 3 | Punto de precio en vivo incorrecto | Cambio de índice al aumentar granularidad | Actualizar índice de `[3]` a `[5]` | v1.3 |

---

*Documento generado el 17 de marzo de 2026.*
*Actualizar este informe con cada cambio significativo en el proyecto.*
