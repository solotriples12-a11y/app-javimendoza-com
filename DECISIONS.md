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
