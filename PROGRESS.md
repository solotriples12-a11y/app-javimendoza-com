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

## 2026-07-27 — MyPodcast: las tres páginas pasan a cubrir Android e iOS
Motivo: la app iOS entra en revisión de App Store y Apple abre la URL de privacidad. Una política que se declara «para Android» no cubre la app que se envía.

- `mypodcast/privacy.html`: subtítulo y `meta description` a «Android e iOS»; credenciales privadas (EncryptedSharedPreferences en Android / Llavero en iOS); tecnología de almacenamiento (Room + DataStore / SQLite + preferencias del sistema); secciones de **Permisos**, **Servicios de terceros**, **Copias de seguridad** y **Cómo borrar tus datos** reescritas por plataforma; matiz del scope de perfil de Google en iOS; fecha al 27/07/2026.
- `mypodcast/data-deletion.html`: subtítulo, ruta in-app con el rótulo real de cada plataforma, borrado local en Android y en iOS, fecha.
- `mypodcast/index.html`: landing agnóstica (hero, `meta description`, salto de silencios marcado como Android-only, controles de coche/bloqueo en vez de solo Android Auto).

**Verificado contra el código de las dos apps, no supuesto:**
- Rótulos: `account_delete_me` = «Borrar datos sincronizados» (Android, `strings.xml:379`, ruta Perfil → Mi cuenta → Zona de peligro); «Borrar datos de la nube» (iOS, `ProfileView.swift:30`, directo en la sección de Drive, sin submenú).
- Android: permisos del `AndroidManifest.xml`, `allowBackup=false` + `data_extraction_rules.xml` excluyendo todos los dominios en cloud-backup y device-transfer.
- iOS: Llavero en `CredentialsStore.swift`, GRDB en `Application Support/mypodcast.db`, `UIBackgroundModes = audio + fetch`, y **solo** las descargas excluidas del backup (`DownloadManager.swift:131`) — la base de datos sí entra en la copia de iCloud.
- Sin Firebase ni analítica en ninguna de las dos; los únicos hosts son `itunes.apple.com`, `googleapis.com` (`drive.appdata`) y los servidores de cada podcast.

**Tres huecos encontrados fuera del encargo** (ver DECISIONS de la misma fecha): Play In-App Review/Updates, el scope de perfil del SDK de Google en iOS, y la landing sin adaptar.

**Verificado:** las tres páginas renderizan en el panel; `grep -i android` no deja ninguna afirmación sin acotar a su plataforma.

## 2026-09-16 — Sotto: página y política de privacidad para App Store
Motivo: la 1.0.0 de Sotto (iPhone) va a revisión de App Store, que exige URL de privacidad y de soporte.

- `public/sotto/index.html`: qué hace, características, requisitos (iOS 26 y chip A15 o posterior), privacidad, soporte y un resumen en inglés (la ficha de App Store tiene también `en-US`).
- `public/sotto/privacy.html`: política en español y en inglés (`#en`) en la misma página.
- `public/icons/sotto.png`: icono de la app a 144 px, sin alfa. `public/index.html`: tarjeta de Sotto.

**Verificado contra el código de Sotto (`sotto-mac`), no supuesto:** el único host es `huggingface.co` (WhisperKit, FluidAudio y `NemotronModelDownloader`); las notificaciones son locales (`ProcessNotifier`); los rastros de diagnóstico guardan fase, hora y memoria, sin audio ni texto; nada de la app se excluye de la copia de seguridad salvo diagnósticos en `Caches`; los rótulos en inglés de Ajustes (`Settings → Saved voices`, `Settings → Models`) salen de `en.lproj`. Las dos páginas y la tarjeta renderizadas en local antes del push.
