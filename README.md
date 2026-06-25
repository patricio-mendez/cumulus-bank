# Banco Cumulus — Demo Web con Agentforce (MIAW)

Sitio web ficticio de Banco Cumulus, preparado para demo de **Agentforce** integrado vía **Messaging for In-App and Web (MIAW)**. El widget de chat en la esquina inferior derecha ("Pregúnteme lo que quiera") lanza una conversación con el agente de disputas desplegado en la org `sdo-disputes`.

## Stack

- HTML/CSS/JS estático — sin build, sin dependencias.
- Embedded Messaging for Web (MIAW) — snippet `bootstrap.min.js` cargado desde el Experience Site de Salesforce.
- ESW Deployment: **`Disputas_Chat_MIAW`**.

## Estructura

```
.
├── index.html      # Página única
├── .nojekyll       # Evita procesamiento Jekyll en GitHub Pages
├── .gitignore
└── README.md
```

## Publicación en GitHub Pages

1. Crear el repo en GitHub (ejemplo: `cumulus-bank-demo`).
2. Desde esta carpeta:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Cumulus Bank demo site with MIAW"
   git branch -M main
   git remote add origin https://github.com/patricio-mendez/cumulus-bank-demo.git
   git push -u origin main
   ```
3. En GitHub → **Settings → Pages**:
   - Source: `Deploy from a branch`
   - Branch: `main` / root (`/`)
4. URL final: `https://patricio-mendez.github.io/cumulus-bank-demo/`

## Configuración requerida en Salesforce

Para que el widget MIAW funcione desde el dominio público de GitHub Pages, el dominio debe estar autorizado en la org `sdo-disputes`:

### 1. CORS Allowed Origins

`Setup → Security → CORS → Allowed Origins List` → **New** → agregar:
```
https://patricio-mendez.github.io
```

### 2. Trusted URLs (CSP)

`Setup → Security → Trusted URLs` → **New Trusted URL**:
- **URL:** `https://patricio-mendez.github.io`
- **CSP Context:** `All`
- **CSP Directives a marcar:**
  - `connect-src`
  - `frame-src`
  - `script-src`
  - `img-src`
  - `style-src`
  - `font-src`

> Sin estos dos pasos el bootstrap MIAW se cargará pero la conversación fallará con error de CORS.

> **Nota:** MIAW no tiene un "Site Endpoint Allowlist" dentro del Embedded Service Deployment — el control de qué dominios pueden invocar al bootstrap se hace 100% vía CORS + Trusted URLs a nivel org.

## Snippet MIAW (referencia)

El snippet ya está embebido al final de `index.html`:

```html
<script type="text/javascript">
  function initEmbeddedMessaging() {
    embeddedservice_bootstrap.settings.language = 'es';
    embeddedservice_bootstrap.init(
      '00Dg7000005Wltm',
      'Disputas_Chat_MIAW',
      'https://storm-0da1adeb707833.my.site.com/ESWDisputasChatMIAW1778781369476',
      { scrt2URL: 'https://storm-0da1adeb707833.my.salesforce-scrt.com' }
    );
  }
</script>
<script type="text/javascript"
  src="https://storm-0da1adeb707833.my.site.com/ESWDisputasChatMIAW1778781369476/assets/js/bootstrap.min.js"
  onload="initEmbeddedMessaging()"></script>
```

## Disclaimer

Banco Cumulus es una marca ficticia creada exclusivamente para demos de Salesforce. Todos los datos, logos, métricas, testimonios y productos son inventados con fines ilustrativos.
