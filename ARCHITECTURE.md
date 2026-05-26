# Architecture

## Propósito
Subdominio único `app.javimendoza.com` que aloja las landings y páginas legales (privacy, etc.) de cada app que publico. Una carpeta por app.

## Stack
- **HTML + CSS planos**, sin JS más allá de un `new Date().getFullYear()` en el footer.
- **nginx 1.27-alpine** sirve `public/` como root.
- **Docker** para empaquetar; **Coolify** sobre Hetzner para el deploy + HTTPS con Let's Encrypt.

## Routing
nginx con `try_files $uri $uri/ $uri.html =404;` →
- `/` → `public/index.html`
- `/merge/` → `public/merge/index.html`
- `/merge/privacy` → `public/merge/privacy.html` (URL limpia, sin `.html`)

## Cache
- HTML: `Cache-Control: no-cache` para que los cambios se vean al instante tras el redeploy.
- CSS/imágenes: 30 días con `immutable`.

## Identidad visual
Mismo lenguaje que `javimendoza.com`: fondo `#0f0f0f`, texto `#f5f5f5`, tipografía system. Acento general rojo `#f56565` (heredado del sitio personal); acento de las páginas de Merge azul `#3b82f6` (mismo blue de la app).

## Convenciones
- Una app = una carpeta en `public/`.
- Cada app debe tener al menos `index.html`. Si va a Play Store, también `privacy.html`.
- Todos los enlaces internos son absolutos (`/merge/...`), nunca relativos.
