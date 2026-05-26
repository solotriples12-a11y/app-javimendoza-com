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

## 2026-05-26 — Sokoban añadido
- `public/sokoban/index.html`: landing (30 niveles, sistema de estrellas, undo, sin red).
- `public/sokoban/privacy.html`: política gemela de la de Merge ajustada a lo que Sokoban guarda (solo estrellas por nivel).
- `public/index.html`: tarjeta de Sokoban añadida bajo la de Merge.
- Sin cambios en `Dockerfile` ni `nginx.conf` — la estructura ya soportaba apps adicionales.
