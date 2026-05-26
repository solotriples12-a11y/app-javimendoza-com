# app.javimendoza.com

Sitio estático con las landings y páginas legales de cada app que publico. Servido por nginx en un contenedor Docker, desplegado por Coolify en Hetzner.

## Estructura

```
public/
├── index.html              → app.javimendoza.com (índice de apps)
├── css/style.css           → estilos compartidos
└── merge/
    ├── index.html          → app.javimendoza.com/merge
    └── privacy.html        → app.javimendoza.com/merge/privacy
```

Cuando añada una app nueva, se crea otra carpeta hermana de `merge/` y se enlaza desde `public/index.html`.

## Desarrollo local

Cualquier servidor estático sirve. Con Python:

```bash
cd public
python3 -m http.server 8000
```

Abre <http://localhost:8000>.

O con Docker, igual que producción:

```bash
docker build -t app-javimendoza .
docker run --rm -p 8080:80 app-javimendoza
```

## Deploy

Consulta [DEPLOY.md](DEPLOY.md) para los pasos de DNS en Hetzner y configuración de Coolify la primera vez. Después, auto-deploy en cada push a `main`.
