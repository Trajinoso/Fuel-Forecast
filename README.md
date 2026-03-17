# Fuel-Forecast
Actualización precio Barril Brent 


Es muy sencillo. Tienes que hacer **dos cosas** cada quincena:

---

### 1️⃣ Consultar el precio real del Brent
Ve a este enlace y apunta el precio de cierre:
👉 [finance.yahoo.com/quote/BZ=F](https://finance.yahoo.com/quote/BZ=F)

---

### 2️⃣ Editar el `index.html` en GitHub — 3 cambios

Entra en tu repositorio de GitHub, haz clic en `index.html` y luego en el **lápiz ✏️** para editar.

**Cambio A — Añade el precio real en la gráfica** (busca `realData`):
```js
// Ejemplo: si el 1 de abril el precio es $91
// Antes:
null, null,    // 1 Abr, 15 Abr

// Después:
91, null,      // 1 Abr = ya conocido, 15 Abr = aún null
```

**Cambio B — Actualiza el precio en la barra superior** (busca `livePrice`):
```html
<!-- Antes: -->
<div class="live-price" id="livePrice">$102.00</div>

<!-- Después: -->
<div class="live-price" id="livePrice">$91.00</div>
```

**Cambio C — Actualiza la fecha** (busca `liveMeta`):
```html
<!-- Antes: -->
<div class="live-meta" id="liveMeta">Actualizado: 15 Mar 2026</div>

<!-- Después: -->
<div class="live-meta" id="liveMeta">Actualizado: 1 Abr 2026</div>
```

Luego pulsa **"Commit changes"** y en 1 minuto la web se actualiza sola.

---

### 📅 Calendario de los 24 puntos del año

Para que sepas exactamente qué índice del array tocar cada vez:

| Fecha | Índice `realData` |
|-------|:-----------------:|
| 1 Ene | 0 ✅ |
| 15 Ene | 1 ✅ |
| 1 Feb | 2 ✅ |
| 15 Feb | 3 ✅ |
| 1 Mar | 4 ✅ |
| 15 Mar | 5 ✅ ← hoy |
| 1 Abr | 6 ⬅ próxima actualización |
| 15 Abr | 7 |
| 1 May | 8 |
| 15 May | 9 |
| 1 Jun | 10 |
| 15 Jun | 11 |
| 1 Jul | 12 |
| 15 Jul | 13 |
| 1 Ago | 14 |
| 15 Ago | 15 |
| 1 Sep | 16 |
| 15 Sep | 17 |
| 1 Oct | 18 |
| 15 Oct | 19 |
| 1 Nov | 20 |
| 15 Nov | 21 |
| 1 Dic | 22 |
| 15 Dic | 23 |

En total son **3 cambios en el archivo, una vez cada 15 días**, y todo lo demás se actualiza solo en la web.
