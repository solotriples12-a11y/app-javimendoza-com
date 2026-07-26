# Decisions

## 2026-05-26 — Sitio estático con nginx, no Flask
**Contexto:** necesito alojar landings + páginas legales para mis apps. El sitio `javimendoza.com` usa Flask porque tiene contadores de YouTube/Instagram dinámicos.

**Opciones consideradas:**
1. Reutilizar la app Flask de `javimendoza.com` y añadir rutas.
2. Flask nuevo en otro contenedor.
3. Estático con nginx en contenedor.

**Decisión:** opción 3.

**Por qué:** las páginas son 100 % estáticas (sin contadores, sin formularios, sin sesión). Flask sería complejidad sin beneficio. nginx es más rápido, más sencillo de cachear y de razonar. La opción 1 además mezcla dominios distintos en una sola app.

**Consecuencias:** si en el futuro alguna app necesita formulario de contacto o algo dinámico, se añadirá como servicio aparte; este proyecto se queda estático.

## 2026-05-26 — Subdominio `app.` en vez de `apps.`
**Contexto:** elegir nombre del subdominio.

**Decisión:** `app.javimendoza.com` (singular).

**Por qué:** lo pidió así el usuario. Más corto. Se siguen alojando múltiples apps debajo, pero el subdominio actúa como "el hub de apps", igual que `app.example.com` en muchos productos.

**Consecuencias:** ninguna; las URLs por app son `/<app>/...` y eso es independiente del nombre del subdominio.

## 2026-05-26 — Una carpeta por app, no un Docker por app
**Contexto:** ¿cada app un contenedor distinto con su propio subdominio (`merge.javimendoza.com`) o todas bajo `app.javimendoza.com/<app>`?

**Decisión:** todas bajo el mismo subdominio en carpetas.

**Por qué:** lo pidió así el usuario. Un único deploy, un único certificado, un único repo. Para sitios estáticos no hay ningún coste técnico en compartir.

**Consecuencias:** si una app específica creciera mucho o necesitara su propio backend, se puede migrar a su propio subdominio sin romper nada (manteniendo redirecciones).

## 2026-05-26 — Privacy policy literal, no plantilla
**Contexto:** Play Console exige URL de política de privacidad.

**Decisión:** redactarla reflejando exactamente lo que Merge hace: cero datos, cero permisos, cero terceros.

**Por qué:** una plantilla genérica que mencione cookies, analytics o publicidad es **falsa** para esta app y desalineada con la Data Safety form de Play Console → motivo común de rechazo. La precisión protege más que la cobertura genérica.

**Consecuencias:** si alguna versión futura de Merge añade analytics o ads, hay que actualizar la política **antes** de publicar esa versión.

## 2026-07-27 — Una sola política para Android e iOS, no una por plataforma
**Contexto:** MyPodcast se publica ya en las dos tiendas. Las páginas `mypodcast/privacy.html` y `mypodcast/data-deletion.html` estaban redactadas solo para Android, y Apple abre la URL de privacidad durante la revisión.

**Opciones consideradas:**
1. Páginas separadas (`/mypodcast/privacy-ios`) y dar a cada tienda la suya.
2. Una única página que acota por plataforma allí donde el comportamiento difiere.

**Decisión:** opción 2. La URL para App Store Connect y para Play Console es la misma: `https://app.javimendoza.com/mypodcast/privacy`.

**Por qué:** es una sola app con un solo modelo de datos; el 90 % del texto es idéntico. Dos páginas se desincronizan a la primera iteración, y una política desactualizada es peor que una con dos incisos por plataforma. La landing se hace agnóstica por el mismo motivo.

**Consecuencias:** cada afirmación que no valga para las dos plataformas debe llevar su acotación explícita («en Android…», «en iOS…»). Al revisar, `grep -i android` sobre las tres páginas no debe devolver ninguna frase sin acotar.

## 2026-07-27 — Tres huecos que la auditoría cross-plataforma destapó
**Contexto:** al verificar el texto contra el código de las dos apps (en vez de contra el encargo) aparecieron tres cosas que la política no cubría. Se documentan porque son caras de volver a descubrir.

1. **Play In-App Review + In-App Updates** (Android, añadido el 2026-07-12). La sección «Servicios de terceros» afirmaba una lista **cerrada** — iTunes, Google Sign-In/Drive y los servidores de podcast — y ya era falsa. Añadidas como bibliotecas de Google Play, aclarando que hablan con Play y no con el desarrollador.
2. **El scope de perfil de Google en iOS.** El texto decía «no se pide tu nombre ni tu foto», cierto en Android (`DriveSyncRepository` pide `email` + `drive.appdata` y **no** `profile`), pero falso en iOS: `GIDSignIn.signIn(withPresenting:hint:additionalScopes:)` incluye el perfil básico del SDK y no se puede reducir. La app solo lee `user.profile?.email`, así que la redacción ahora separa lo que se **pide** de lo que se **usa y guarda**.
3. **La base de datos de iOS sí entra en el backup de iCloud.** Solo los audios descargados llevan `isExcludedFromBackup`. El paralelismo con Android (donde el backup está desactivado del todo) no existe y no se podía afirmar lo mismo en ambas.

**Consecuencias:** la regla de «vigilar» del BACKLOG deja de ser «si Merge añade SDKs» y pasa a incluir MyPodcast en sus dos plataformas. Cualquier SDK nuevo, permiso nuevo o cambio de scope obliga a tocar la política **antes** del release, y a verificarlo contra el código de las dos apps.
