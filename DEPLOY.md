# 🚀 Deploy FUSE Demo a Vercel — 5 minutos

Esta carpeta es un **paquete self-contained** listo para subir a Vercel.
El frontend está aquí. El backend LLM sigue corriendo en Railway (Rodmat), y CORS ya está abierto para cualquier origen.

---

## Opción A — Vercel drag & drop (más rápido, 2 min)

1. Ir a https://vercel.com/new
2. Login con GitHub (o crear cuenta si no tienes)
3. Click **"Deploy a project"** → **"Browse all templates"** → esquina inferior "Skip templates and deploy manually"
4. Arrastra ESTA CARPETA COMPLETA (`FUSE_DEMO_VERCEL/`) al área de drop
5. Nombre del proyecto: `fuse-demo` (o el que quieras)
6. Deploy → en ~30 segundos tienes URL tipo `https://fuse-demo-xxxx.vercel.app`

Listo. Compártela al inversor.

---

## Opción B — Vercel via GitHub (más profesional, 5 min)

Recomendado si quieres iterar el demo con git.

1. Crear repo GitHub nuevo `fuse-demo-standalone` (público o privado)
2. Subir el contenido de esta carpeta:
   ```bash
   cd C:\Users\horac\Desktop\ProjectRodmat\FUSE_DEMO_VERCEL
   git init
   git add .
   git commit -m "initial FUSE demo"
   git remote add origin https://github.com/TU-USER/fuse-demo-standalone.git
   git push -u origin main
   ```
3. Ir a https://vercel.com/new → **"Import Git Repository"** → seleccionar `fuse-demo-standalone`
4. Framework preset: **"Other"** (es static)
5. Root Directory: **`./`** (raíz)
6. Deploy → URL en ~30s

Cada `git push` re-despliega automáticamente.

---

## Opción C — Dominio custom (fuse.commerce/demo o similar)

Una vez desplegado en Vercel:

1. Proyecto → Settings → Domains
2. Add domain: `demo.fusecommerce.io` (o el que compres)
3. Vercel te da 2 opciones DNS (CNAME o A record) — configúralas en tu registrar
4. Certificado SSL automático en 1-2 min

Alternativa: si tienes ya el dominio FUSE apuntando a un Vercel personal, puedes usar `subdomain rewrite` para servir esta demo bajo `/demo/`.

---

## 🔗 Integrar dentro de la landing FUSE existente

Si tu landing FUSE actual está en Vercel personal y quieres AÑADIR la demo como sección `/demo/`:

### Método A: subdirectorio en el mismo proyecto

1. Copia el contenido de `FUSE_DEMO_VERCEL/` a una carpeta `public/demo/` dentro del proyecto de la landing FUSE
2. Actualiza rutas relativas si es necesario (los assets ya son relativos: `assets/style.css`)
3. Añade un botón en la landing:
   ```html
   <a href="/demo/" class="btn-primary">🚀 Probar Demo en Vivo</a>
   ```
4. `git push` → deploy → nueva URL: `https://tulandingfuse.com/demo/`

### Método B: subdominio separado (mejor SEO + tracking)

1. Deploy este paquete a `demo.fusecommerce.io` (Opción B/C arriba)
2. Añade link en la landing: `<a href="https://demo.fusecommerce.io">Ver Demo →</a>`

---

## ⚙️ ¿Cambiar algo antes de desplegar?

### Cambiar el texto/branding del hub
Edita `index.html` — el HTML es simple, todo el copy está inline.

### Cambiar los ejemplos de Sofía
Edita `sofia.html` línea con `CALENDAR.services` y `CALENDAR.slots` (calendario dummy). Cambia también los `<button onclick="tryExample(...)">` para los mensajes sugeridos.

### Cambiar el sample invoice de Ana
Edita `ana.html` → variable `SAMPLES` (objeto JS). Cada sample tiene `name` y `text`. Añade/modifica libremente.

### Cambiar la URL del backend LLM
Los HTML llaman por defecto a `https://rodmatdashboard-production.up.railway.app`. Si más adelante quieres backend propio FUSE (en Railway separado), edita `assets/fuse-api.js` línea `baseUrl:`.

---

## 🛡 Seguridad post-deploy

Cuando esta demo esté en un dominio público (fuse.commerce/demo), considera:

1. **Rotar el `demoToken`** — está hardcoded en `fuse-api.js` como `fuse-demo-2026`. Cámbialo cada 2-3 meses y actualiza también la env var `FUSE_DEMO_TOKEN` en el backend Railway.
2. **Rate limit más estricto** si vas a promocionar la demo (actualmente 100 req/hora/IP). Cambiar en `backend/app/api/fuse_demo.py` línea `_RATE_LIMIT = 100`.
3. **Analytics** — añadir Plausible/PostHog snippet en los HTML para medir uso real.
4. **Anti-abuse** — reCAPTCHA v3 antes de la primera llamada al API si detectas abuso.

Nada de esto bloquea el lanzamiento inicial.

---

## 💰 Coste Vercel

- **Hobby (gratis)**: 100GB bandwidth/mes, 100 deploys/día. Suficiente para demo público con miles de visitas.
- **Pro ($20/mes)**: solo si necesitas custom domains con SSL avanzado, edge functions, analytics premium. NO necesario para arrancar.

---

Cuando tengas la URL de Vercel, mándamela y actualizo:
- El deck (slide 4 y 15 mencionan URL)
- El GUION_REUNION (misma referencia)
- El README

Para que todo apunte a la nueva URL FUSE-branded y no a la de Rodmat.
