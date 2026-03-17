# 🛢️ Master Prompt — Dashboard Previsión Precio Combustible 2026

> Copia y pega este prompt completo en cualquier IA (GPT-4, Gemini, Claude, etc.)
> para recrear el dashboard desde cero con todas las especificaciones exactas.

---

## PROMPT COMPLETO

```
Necesito que crees un dashboard web completo en un único archivo HTML autocontenido
(sin dependencias externas salvo librerías CDN) con las siguientes características:

─────────────────────────────────────────
PROPÓSITO
─────────────────────────────────────────
Dashboard de previsión del precio del crudo Brent para 2026, con:
- Gráfica precio vs tiempo con granularidad quincenal (cada 15 días)
- Precio en tiempo real obtenido automáticamente de una API pública
- Tres escenarios de previsión comparados visualmente
- Tarjetas resumen con datos clave
- Diseño oscuro, profesional, apto para publicarse en GitHub Pages

─────────────────────────────────────────
STACK TÉCNICO
─────────────────────────────────────────
- HTML5 + CSS3 + JavaScript vanilla (ES2020+)
- Chart.js 4.4.1 desde CDN: https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js
- Fuentes Google Fonts CDN: 'DM Mono' (400, 500) y 'Syne' (700, 800)
- Sin frameworks JS (no React, no Vue)
- Todo en un solo archivo index.html

─────────────────────────────────────────
DISEÑO VISUAL
─────────────────────────────────────────
Paleta de colores (variables CSS):
  --bg:      #060c18   (fondo principal, navy muy oscuro)
  --surface: #0d1625   (tarjetas y contenedores)
  --border:  #1a2740   (bordes sutiles)
  --gold:    #f5c842   (color primario, precio real y acentos)
  --blue:    #4a9eff   (previsión de mercado)
  --green:   #2ecc8e   (escenario bajista)
  --orange:  #ff7143   (escenario alcista)
  --red:     #ef4444   (alertas y línea de guerra)
  --text:    #c8d8f0   (texto principal)
  --muted:   #4a637a   (texto secundario)

Fondo del body: gradientes radiales sutiles en azul y dorado sobre --bg.
Animaciones de entrada: fadeUp (opacity 0→1 + translateY 14px→0, 0.5s) con
  delays escalonados (0s, 0.1s, 0.15s, 0.2s, 0.3s, 0.4s) para cada sección.
Animación pulse en el punto vivo del header tag (dot parpadeante).
Animación spin para el spinner de carga.
Max-width del contenido: 940px, centrado.

─────────────────────────────────────────
ESTRUCTURA HTML (orden de secciones)
─────────────────────────────────────────
1. HEADER
   - Tag pill con punto parpadeante: "Datos en tiempo real · Actualización automática"
   - H1 con fuente Syne 800: "Previsión Precio <span>Crudo Brent</span> 2026"
     (el span con color --gold)
   - Subtítulo: "Consenso de mercado (EIA · Goldman Sachs · IEA) vs. escenarios
     de análisis propio · USD/barril"

2. BARRA DE PRECIO EN VIVO (.live-bar)
   - Etiqueta: "Brent Crude · Precio en vivo"
   - Precio grande (id="livePrice"): se rellena con JS
   - Variación diaria (id="liveChange"): verde si sube, roja si baja
   - Spinner CSS animado (id="spinner"): visible solo durante la carga
   - Metadatos fuente (id="liveMeta"): fecha y fuente del dato

3. BOTONES FILTRO (.filters)
   Cuatro botones que muestran/ocultan líneas de la gráfica:
   - "Todos los escenarios" → muestra las 4 líneas (activo por defecto, fondo --gold)
   - "📊 Mercado (EIA/GS)" → solo precio real + previsión mercado (fondo azul semitransparente)
   - "🟢 Bajista" → precio real + escenario bajista (fondo verde semitransparente)
   - "🔴 Alcista" → precio real + escenario alcista (fondo naranja semitransparente)

4. GRÁFICA (.chart-box, altura 380px)
   Chart.js tipo 'line' con plugin personalizado que dibuja una línea
   roja discontinua vertical en el punto "15 Mar" con texto
   "⚡ Guerra Irán 28 Feb" (font: 10px DM Mono).

5. TARJETAS RESUMEN (.cards, grid auto-fit minmax 200px)
   4 tarjetas con borde izquierdo de color:
   - Dorada: "Precio actual Brent" — valor dinámico (id="cardPrice")
   - Azul: "Previsión mercado" — "$64–88" — badge "Escenario base institucional"
   - Verde: "Escenario bajista" — "$54/b dic" — badge "Probabilidad estimada: ~25%"
   - Naranja: "Escenario alcista" — ">$100/b" — badge "Probabilidad estimada: ~30%"

6. FOOTER + DISCLAIMER
   - Fuentes citadas: EIA, Goldman Sachs, IEA, stooq.com
   - Aviso: actualización al abrir y cada 5 minutos
   - Disclaimer legal en caja roja semitransparente

─────────────────────────────────────────
DATOS DE LA GRÁFICA
─────────────────────────────────────────
Granularidad: quincenal. 24 puntos = año completo 2026.

Labels (eje X):
["1 Ene","15 Ene","1 Feb","15 Feb","1 Mar","15 Mar","1 Abr","15 Abr",
 "1 May","15 May","1 Jun","15 Jun","1 Jul","15 Jul","1 Ago","15 Ago",
 "1 Sep","15 Sep","1 Oct","15 Oct","1 Nov","15 Nov","1 Dic","15 Dic"]

realData (precio real conocido hasta 15 Mar, el resto null):
[65,67, 70,72, 79,102, null,null, null,null, null,null, null,null,
 null,null, null,null, null,null, null,null, null,null]

mktData (previsión consenso EIA/Goldman Sachs/IEA):
[65,67, 70,68, 76,95, 91,88, 85,82, 79,77, 76,74, 73,72,
 71,70, 69,68, 67,66, 65,64]

bearData (escenario bajista — paz rápida + recesión global):
[65,67, 70,70, 80,88, 78,70, 67,65, 63,62, 61,60, 59,58,
 57,57, 56,56, 55,55, 54,54]

bullData (escenario alcista — conflicto prolongado / cierre Ormuz):
[65,67, 70,75, 90,118, 115,112, 108,104, 99,95, 90,86, 83,80,
 78,76, 74,72, 70,68, 66,64]

Estilos de líneas:
- realData:  color #f5c842, strokeWidth 3, puntos radio 5, sin guiones
- mktData:   color #4a9eff, strokeWidth 2, guiones [8,4]
- bearData:  color #2ecc8e, strokeWidth 2, guiones [5,4]
- bullData:  color #ff7143, strokeWidth 2, guiones [5,4]

Eje Y: min 50, max 130, ticks con prefijo "$"
Tooltip: modo 'index', fondo --surface, fuente DM Mono

─────────────────────────────────────────
PRECIO EN TIEMPO REAL (JS)
─────────────────────────────────────────
Fuente de datos: stooq.com, símbolo BZ.F (Brent Crude Futures)
URL: https://stooq.com/q/l/?s=bz.f&f=sd2t2ohlcv&e=csv
Respuesta CSV: Symbol, Date, Time, Open, High, Low, Close, Volume
Precio = cols[6] (Close), Open = cols[3]

El fetch directo falla por CORS. Usar cascada de 3 intentos:

  1. Proxy 1 — corsproxy.io:
     fetch(`https://corsproxy.io/?url=${encodeURIComponent(stooqURL)}`)

  2. Proxy 2 (fallback) — allorigins.win:
     fetch(`https://api.allorigins.win/raw?url=${encodeURIComponent(stooqURL)}`)

  3. Fallback estático: mostrar "~$102" con aviso "⚠ Datos en vivo no disponibles ahora"

Al obtener precio exitosamente:
  - Actualizar id="livePrice" con `$${price.toFixed(2)}`
  - Actualizar id="liveChange" con variación vs open (color verde/rojo según signo)
  - Actualizar id="liveMeta" con fuente y fecha
  - Actualizar id="cardPrice"
  - Inyectar precio en myChart.data.datasets[0].data[5] (índice 5 = "15 Mar")
  - Llamar myChart.update()

Ejecutar fetchLivePrice() al cargar y con setInterval cada 5 minutos (300000ms).

─────────────────────────────────────────
COMPORTAMIENTO DE LOS BOTONES FILTRO
─────────────────────────────────────────
Función setScenario(key, el):
  - Quitar todas las clases activas de los botones
  - Añadir clase activa al botón pulsado (a-all / a-mkt / a-bear / a-bull)
  - Ocultar/mostrar datasets según mapa:
      all:  [true,  true,  true,  true ]
      mkt:  [true,  true,  false, false]
      bear: [true,  false, true,  false]
      bull: [true,  false, false, true ]
  - myChart.update()

─────────────────────────────────────────
CONTEXTO DE LOS DATOS (para el análisis)
─────────────────────────────────────────
Fecha de referencia: 16 de marzo de 2026.
Evento clave: conflicto armado EE.UU./Israel sobre Irán iniciado el 28 de febrero
de 2026, que disparó el crudo Brent desde ~$72 hasta un pico de ~$120/barril,
retrocediendo a ~$102 al 16 de marzo.

Escenario bajista (prob. ~25%): acuerdo diplomático rápido + indicios de recesión
global → caída acelerada hasta $54/b en diciembre 2026.

Escenario alcista (prob. ~30%): cierre parcial del Estrecho de Ormuz o escalada
hacia Arabia Saudí → precios sostenidos por encima de $100/b durante el año.

Escenario base mercado (prob. ~45%): normalización progresiva con OPEC+ aumentando
producción, llegando a $64–88/b en la segunda mitad del año.

─────────────────────────────────────────
REQUISITOS FINALES
─────────────────────────────────────────
- El archivo debe llamarse index.html
- Debe funcionar abriéndolo directamente en el navegador (doble clic)
- Debe funcionar publicado en GitHub Pages sin configuración adicional
- No usar localStorage ni sessionStorage
- No usar frameworks JS externos (solo Chart.js vía CDN)
- Responsive: funcionar en móvil y escritorio
- El disclaimer legal debe indicar que no es asesoramiento financiero
```

---

## NOTAS DE USO

**Para actualizar los datos reales** (cada quincena):
Editar el array `realData` en el HTML añadiendo el nuevo precio en el índice
correspondiente y poniendo `null` en los futuros. Por ejemplo, al llegar el 1 de abril:
```js
// Cambiar:
null, null,   // Abr
// Por:
95, null,     // 1 Abr = dato real, 15 Abr = aún null
```

**Para ajustar las previsiones**: editar los arrays `mktData`, `bearData`, `bullData`
según evolucione el consenso de mercado.

**Para publicar en GitHub Pages**:
1. Crear repositorio en github.com
2. Subir `index.html`
3. Settings → Pages → Deploy from branch → main → / (root) → Save
4. URL resultante: `https://TU-USUARIO.github.io/NOMBRE-REPO/`
