# Progress

## 2026-05-26 — Bootstrap del proyecto
- Estructura inicial: `Dockerfile` (nginx 1.27-alpine), `nginx.conf` con URLs limpias y cabeceras de cache/seguridad, `public/` como root.
- `public/index.html`: índice del subdominio con tarjeta de Merge.
- `public/merge/index.html`: landing de Merge (qué es, características, contacto, link a privacy).
- `public/merge/privacy.html`: política de privacidad redactada al detalle reflejando que la app no recopila datos ni pide permisos.
- `public/css/style.css`: estilo dark coherente con `javimendoza.com`, acento azul `#3b82f6` para Merge.
- Docs: `README`, `DEPLOY` (DNS Hetzner + Coolify paso a paso), `ARCHITECTURE`, `DECISIONS`, `BACKLOG`.

**Verificado:** los HTML se renderizan en el panel de Launch preview tras cada Write.
**Pendiente verificación:** el deploy real (DNS + Coolify) lo tiene que hacer el usuario; no probado todavía contra `https://app.javimendoza.com`.
