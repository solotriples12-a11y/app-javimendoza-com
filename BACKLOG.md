# Backlog

## Próximo
- Capturas/mockup en la landing de Merge cuando esté la app publicada (sustituir el bloque "Características" por algo más visual).
- Añadir favicon (`public/favicon.ico` o SVG).

## Cuando se publique la siguiente app
- Crear `public/<app>/index.html` (+ `privacy.html` si va a Play Store).
- Añadirla en la lista de `public/index.html`.

## Vigilar
- Cualquier cambio en Merge que añada permisos, red, analytics o SDKs de terceros obliga a actualizar `merge/privacy.html` **antes** del release.
- Lo mismo para MyPodcast, pero en **las dos** apps (Android e iOS): permisos, SDKs, scopes de Google o exclusiones de backup nuevos obligan a revisar `mypodcast/privacy.html` y `mypodcast/data-deletion.html` antes del release, verificando contra el código de cada plataforma. Ambas páginas son agnósticas: toda afirmación que no valga para las dos debe acotarse («en Android…», «en iOS…»).
