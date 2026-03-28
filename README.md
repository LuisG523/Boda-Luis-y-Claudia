# 💍 Web de Boda — Guía completa

Diseño minimalista inspirado en Apple. Bilingüe (ES/EN). Listo para GitHub Pages.

---

## 📁 Estructura

```
boda/
├── index.html      ← Todo en un archivo (HTML + CSS + JS)
├── images/         ← Sube aquí tus fotos
│   ├── hero.jpg
│   ├── historia1.jpg … historia4.jpg
│   ├── foto1.jpg  … foto9.jpg   (galería)
└── README.md
```

---

## ✏️ Cómo personalizar (paso a paso)

### 1 — Nombres y fecha

Abre `index.html` y busca los comentarios `<!-- ✏️ EDIT -->`.

| Qué cambiar | Dónde buscarlo |
|---|---|
| Nombres en el hero | `<h1 class="hero-names">Elena <em>&amp;</em> Marcos</h1>` |
| Fecha en el hero | `<p class="hero-date">12 · VII · 2025</p>` |
| Ciudad | `<p class="hero-loc">Sevilla, España</p>` |
| Iniciales nav | `<a href="#inicio" class="nav-logo">E &amp; M</a>` |
| Footer | Sección `<footer>` al final |

### 2 — Fecha del countdown (JS)

En el bloque `CFG` al final del `<script>`:

```js
// Meses van de 0 a 11: enero=0, julio=6, diciembre=11
weddingDate: new Date(2025, 6, 12, 12, 0, 0),
//                    AÑO  MES DÍA  HH  MM
```

### 3 — Detalles de la boda

Busca la sección `<!-- DETALLES -->` y edita:
- Hora y lugar de la ceremonia
- Hora y lugar de la celebración
- Dress code
- Horario
- Email de contacto y teléfono

### 4 — Traducciones (ES/EN)

Al final del `<script>`, en el objeto `T`, edita las frases en ambos idiomas (`es` y `en`). Cada clave es la misma, solo cambia el texto.

### 5 — Mapa de Google

1. Abre [maps.google.com](https://maps.google.com) y busca tu venue
2. Click en **Compartir → Insertar un mapa → HTML**
3. Copia el valor del atributo `src="..."` del `<iframe>`
4. Pégalo en el `<iframe>` de la sección `<!-- Mapa -->`

### 6 — Fotos

**Hero (portada):**
1. Pon tu foto como `images/hero.jpg`
2. En el CSS, busca `.hero-bg` y descomenta:
   ```css
   background-image: url('images/hero.jpg');
   background-size: cover; background-position: center;
   ```

**Galería:**
1. Sube tus fotos como `images/foto1.jpg`, `images/foto2.jpg`, …
2. En `CFG.photos` del script:
   ```js
   photos: [
     'images/foto1.jpg',
     'images/foto2.jpg',
     // ... hasta 9
   ],
   ```

**Historia / Timeline:**
1. Sube fotos como `images/historia1.jpg`, …
2. En cada `.tl-img-inner`, reemplaza con:
   ```html
   <img src="images/historia1.jpg" alt="Descripción"
        style="width:100%;height:100%;object-fit:cover"/>
   ```

---

## 📋 Configurar el RSVP (recibir respuestas)

### Opción A — Google Sheets (gratis, recomendado)

**1.** Crea un Google Sheet con estas cabeceras en la fila 1:

```
Timestamp | Name | Email | Attending | Guests | GuestNames | Dietary | BusTo | BusFrom | BusTime
```

**2.** Ve a **Extensiones → Apps Script** y pega este código:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data  = JSON.parse(e.postData.contents);
    sheet.appendRow([
      data.timestamp, data.name, data.email,
      data.attending, data.guests, data.guestNames,
      data.dietary, data.busTo, data.busFrom, data.busTime
    ]);
    return ContentService
      .createTextOutput(JSON.stringify({result:'ok'}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({result:'error',err:err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

**3.** Click **Implementar → Nueva implementación**
   - Tipo: **Aplicación web**
   - Ejecutar como: **Yo**
   - Acceso: **Cualquier persona**

**4.** Copia la URL que genera y pégala en `CFG.sheetsURL`:

```js
sheetsURL: 'https://script.google.com/macros/s/TU_ID/exec',
```

### Opción B — Formspree (más fácil, gratis hasta 50 env/mes)

1. Crea cuenta en [formspree.io](https://formspree.io)
2. Crea un Form y copia el endpoint
3. En `CFG`:
   ```js
   formspreeURL: 'https://formspree.io/f/TU_ID',
   ```

---

## 🚀 Publicar en GitHub Pages (gratis)

### Opción A — Interface web (sin instalar nada)

1. Crea cuenta en [github.com](https://github.com)
2. Haz click en **+** → **New repository**
3. Nombre: `mi-boda` (o el que quieras) → **Create**
4. En la página del repo, click **Add file → Upload files**
5. Arrastra todos los archivos de la carpeta `boda/`
6. Click **Commit changes**
7. Ve a **Settings → Pages → Source: main / (root)** → **Save**
8. En ~2 minutos tu web estará en:
   `https://TU_USUARIO.github.io/mi-boda`

### Opción B — Terminal (si sabes usar git)

```bash
cd boda/
git init
git add .
git commit -m "Web de boda lista 💍"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/mi-boda.git
git push -u origin main
```

Luego activa GitHub Pages en Settings → Pages.

### Dominio personalizado (opcional)

Si quieres `elenaymarcos.com` en vez de `*.github.io`:
1. Compra el dominio (namecheap, porkbun, etc. ~10€/año)
2. Settings → Pages → Custom domain → escribe tu dominio
3. En el DNS de tu dominio, añade un registro CNAME apuntando a `TU_USUARIO.github.io`

---

## 🎨 Cambiar colores

Al inicio del `<style>`, edita las variables:

```css
:root {
  --gold:  #a8834a;   /* Dorado — color principal */
  --gold2: #c9a96e;   /* Dorado claro — detalles */
  --ivory: #f9f6f0;   /* Fondo principal */
  --ink:   #1c1916;   /* Texto oscuro */
}
```

---

## 📱 Compatibilidad

✅ Chrome, Safari, Firefox, Edge  
✅ iPhone / Android  
✅ GitHub Pages, Netlify, Vercel  
✅ Accesible (ARIA, alt text, contraste, reduced-motion)

---

*Hecho con ❤️ — ¡que la boda sea preciosa!*
