# Deploy

Pasos para publicar `app.javimendoza.com` la primera vez. Una vez configurado, los cambios se despliegan solos al hacer push a `main`.

## 1. DNS en Hetzner

En el panel de Hetzner DNS (donde gestionas `javimendoza.com`):

1. Abre la zona DNS de `javimendoza.com`.
2. Añade un registro nuevo:
   - **Tipo:** `A`
   - **Nombre:** `app`
   - **Valor:** la **IP pública** del servidor de Hetzner donde corre Coolify (la misma a la que apunta `javimendoza.com`; lo puedes ver con `dig +short javimendoza.com`).
   - **TTL:** el por defecto (3600 s) está bien.
3. Si tienes IPv6 en ese servidor, añade también un `AAAA` para `app` con la IPv6.
4. Guarda.

Verifica con `dig +short app.javimendoza.com` que ya resuelve a tu IP antes de pasar al siguiente paso (puede tardar unos minutos).

## 2. Repositorio en GitHub

```bash
cd /Users/javier/Documents/Claude/Projects/web-javimendoza/app-javimendoza-com
git init
git add .
git commit -m "Initial commit"
git branch -M main
# Crear el repo vacío en GitHub primero (gh repo create ... o web)
git remote add origin git@github.com:<tu-usuario>/app-javimendoza-com.git
git push -u origin main
```

## 3. Nueva aplicación en Coolify

En el panel de Coolify, mismo servidor donde está `javimendoza.com`:

1. **+ New Resource → Application → Public Repository** (o privado conectando GitHub).
2. **Repository URL:** la del repo recién creado. **Branch:** `main`.
3. **Build Pack:** `Dockerfile`.
4. **Domains:** `https://app.javimendoza.com`.
5. **Port:** `80` (el `EXPOSE 80` del Dockerfile).
6. Activa **HTTPS** → certificado automático Let's Encrypt.
7. Activa **Auto-deploy on push** y configura el webhook de GitHub si Coolify lo pide.
8. **Deploy**.

Tras un par de minutos, `https://app.javimendoza.com` debería responder con el índice de apps, y `https://app.javimendoza.com/merge/privacy` con la política.

## 4. Conectar con Play Console

En Google Play Console, ficha de Merge:

- **Política de privacidad:** `https://app.javimendoza.com/merge/privacy`
- **Sitio web (opcional):** `https://app.javimendoza.com/merge`
- **Correo de soporte:** `solotriples12@gmail.com`

## Cambios futuros

Cualquier edición en `public/**` → push a `main` → Coolify reconstruye y redepliega automáticamente.

## Añadir otra app

1. Crear `public/<nombre-app>/index.html` y, si Play Console lo pide, `privacy.html`.
2. Añadir su `<a class="app-card">` en `public/index.html`.
3. Push.
