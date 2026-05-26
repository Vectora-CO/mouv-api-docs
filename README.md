# Mouv Platform — Public API Documentation

Site público: **https://developer.mouvlatam.com** (próximamente, deploy vía Mintlify)

Este repo contiene la documentación de la [API pública de Mouv Platform](https://consola.mouvlatam.com) para integraciones cliente. Stack: **Mintlify** (Stripe-quality docs, OpenAPI native, MDX).

---

## Estructura del repo

```
mouv-api-docs/
├── mint.json                   # Mintlify config (nav, theme, branding)
├── introduction.mdx            # Landing page
├── authentication.mdx          # API key auth
├── quickstart.mdx              # 5-min tutorial
├── scopes-and-permissions.mdx  # READ vs WRITE
├── errors.mdx                  # Códigos de error
├── rate-limits.mdx             # Rate limits + retry strategy
├── dictionaries.mdx            # Bancos / accountTypeId / documentTypeId
├── sarlaft-compliance.mdx      # Obligaciones regulatorias colombianas
└── api-reference/
    ├── balance/get-balance.mdx
    ├── transfers/
    │   ├── resolve-key.mdx
    │   ├── quote-breb.mdx
    │   ├── quote-ach.mdx
    │   └── send.mdx
    ├── deposits/
    │   ├── create-pse.mdx
    │   ├── list.mdx
    │   └── share-email.mdx
    └── transactions/
        ├── list.mdx
        ├── get.mdx
        └── receipt.mdx
```

---

## Setup Mintlify (pasos pendientes CTO)

Mintlify free tier rinde Stripe-quality docs sin esfuerzo, auto-syncea con cada push al repo.

### Paso 1 — Crear cuenta Mintlify

1. Andá a https://mintlify.com
2. Click "Get Started"
3. Sign in con GitHub (cuenta `vectora-bkcap` o cuenta admin de la org `Vectora-CO`)
4. Authorize Mintlify para acceder al org `Vectora-CO`

### Paso 2 — Conectar este repo

1. En el dashboard Mintlify → "New project"
2. Seleccioná el repo `Vectora-CO/mouv-api-docs`
3. Mintlify detecta `mint.json` automáticamente
4. Click "Deploy"
5. Mintlify te da una URL temporal `mouv.mintlify.app` o similar

### Paso 3 — Custom domain `developer.mouvlatam.com`

1. En Mintlify dashboard → Settings → Custom domain
2. Ingresá `developer.mouvlatam.com`
3. Mintlify te da un CNAME target (algo como `cname.mintlify.app`)
4. En Route53 (cuenta AWS Mouv `mouvlatam.com` hosted zone):
   ```
   developer.mouvlatam.com CNAME cname.mintlify.app
   ```
5. Esperar ~5-15 min DNS propagation
6. Mintlify auto-provisiona SSL cert (Let's Encrypt)

### Paso 4 — Logo + favicon

Subí los assets en `logo/` directorio:
- `logo/light.svg` — versión para fondo claro
- `logo/dark.svg` — versión para fondo oscuro
- `favicon.svg`

Mouv logo está en `apps/admin-dashboard/public/` y `apps/client-dashboard/public/`.

### Paso 5 — Verificación

```bash
# Verificar SSL
curl -sI https://developer.mouvlatam.com | head -5

# Debe retornar 200 + HTTPS válido
```

---

## Editar la documentación

Local development:

```bash
# Install Mintlify CLI
npm i -g mintlify

# Preview localhost:3000
mintlify dev
```

Para cambios menores:

1. Crear branch `docs/<descripcion>`
2. Editar el .mdx pertinente
3. PR → merge a `main`
4. Mintlify auto-deploya en 1-2 min

Para cambios mayores (nueva sección, restructuring):

1. Actualizar `mint.json` `navigation` array
2. Crear los .mdx nuevos
3. Linkear desde otras páginas si aplica
4. Preview con `mintlify dev` ANTES de push

---

## Convenciones de escritura

- **Español Colombia** (no español neutro, no español de España)
- **Tone**: directo, técnico, sin marketing fluff
- **Code examples**: siempre incluí curl + Node.js (mínimo). Python opcional para endpoints high-traffic.
- **Sin emojis** en docs (S38 F4.2 + S62 reforzada)
- Mintlify components disponibles: `<Card>`, `<CardGroup>`, `<Steps>`, `<Step>`, `<Note>`, `<Warning>`, `<Tip>`, `<Expandable>`, `<ParamField>`, `<ResponseField>`, `<RequestExample>`, `<ResponseExample>`

---

## Versionado

Esta doc refleja el estado actual de la API. Cuando agreguemos endpoints o cambiemos shapes:

- **Additive change** (campo nuevo, endpoint nuevo): merge a main, Mintlify deploya, sin breaking change para clientes
- **Breaking change** (rename, eliminación de campo, cambio status code): se comunica con 30 días de aviso vía `developers@vectora.com.co`, agregamos sección "Migration guide" temporal

---

## Sync con el código fuente

La doc viva en este repo, pero los **shapes de request/response** vienen del código en `Vectora-CO/mouv-platform/apps/api-gateway/src/routes/*.ts`. Si actualizás un endpoint, asegurate de actualizar la doc en el mismo PR (o follow-up dentro del mismo sprint).

Convention: cualquier PR que cambia behavior de la API pública debe incluir un commit `docs(api):` con la actualización aquí. CI puede gate esto eventualmente (script que compara docs vs OpenAPI spec).

---

## OpenAPI 3 spec (próximo)

Mintlify soporta OpenAPI 3 native — cuando publiquemos un `openapi.yaml` en este repo, los endpoint pages se auto-generan con interactive playground. Roadmap S67+.

---

## Soporte para clientes

Los clientes externos:
- **Reportan bugs** o solicitudes en este repo (Issues) — habilitar Issues post-Mintlify setup
- **Contacto comercial**: hola@vectora.com.co
- **Soporte técnico API**: dev-support@vectora.com.co (configurar alias post-launch)

---

## License

Documentación © Vectora S.A.S. (Mouv Platform) — todos los derechos reservados. Los snippets de código en esta doc se publican bajo MIT para que los clientes puedan copiarlos a sus integraciones sin restricciones.
