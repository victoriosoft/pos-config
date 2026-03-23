# pos-config

Configuración estática para **pos-desktop** (Flutter) servida via GitHub Pages.

## Arquitectura

```
pos-desktop (Flutter)
       │
       │ GET https://config.victoriosoft.online/pos-desktop/staging.json
       ▼
GitHub Pages (este repo)
       │
       │ Retorna JSON con URLs de servicios
       ▼
pos-desktop usa URLs para conectarse
```

## ¿Por qué existe este repo?

pos-desktop necesita saber las URLs de los servicios **ANTES** de poder autenticarse.
Este repo proporciona esa configuración inicial ("bootstrap config").

```
PROBLEMA:
  pos-desktop necesita URL de auth-service para hacer login
  Pero ¿de dónde obtiene esa URL?

SOLUCIÓN:
  1. pos-desktop hace GET a config.victoriosoft.online
  2. Obtiene JSON con authServiceUrl, posServiceUrl, etc.
  3. Ahora puede hacer login y sincronizar
```

## Estructura

```
pos-config/
├── README.md
├── CNAME                         # config.victoriosoft.online
└── pos-desktop/
    ├── staging.json              # Config para ambiente staging
    └── production.json           # Config para ambiente production
```

## URLs

| Ambiente | URL |
|----------|-----|
| Staging | https://config.victoriosoft.online/pos-desktop/staging.json |
| Production | https://config.victoriosoft.online/pos-desktop/production.json |

## Contenido del JSON

```json
{
  "environment": "staging",
  "version": "1.0.0",
  "lastUpdated": "2026-03-22",
  "services": {
    "authServiceUrl": "https://auth.chillsell.online",
    "posServiceUrl": "https://pos.chillsell.online",
    "customerServiceUrl": "https://customer.chillsell.online",
    "billingServiceUrl": "https://billing.chillsell.online",
    "productsServiceUrl": "https://product.chillsell.online",
    "lookupServiceUrl": "https://lookup.victoriosoft.online",
    "verificationUrl": "https://verificar.chillsell.online"
  }
}
```

## Diferencia con Config Server (Spring Cloud)

| Aspecto | Este repo (pos-config) | Config Server (8888) |
|---------|------------------------|----------------------|
| **Usuario** | pos-desktop (Flutter) | Microservicios Java |
| **Formato** | JSON simple | YAML (Spring properties) |
| **Cuándo** | Al iniciar app (antes de login) | Al iniciar microservicio |
| **Hosting** | GitHub Pages (estático) | Contenedor Docker |

## Cómo actualizar

1. Editar el JSON correspondiente (staging.json o production.json)
2. Commit y push a main
3. GitHub Pages actualiza automáticamente (~1 minuto)

```bash
# Ejemplo
git add .
git commit -m "update: cambiar URL de productsServiceUrl"
git push origin main
```

## Notas importantes

- **productsServiceUrl**: Es `product.chillsell.online` (SIN 's'), NO `products.chillsell.online`
- Los cambios en este repo afectan a TODOS los pos-desktop que se inicien después
- Si hay error en el JSON, pos-desktop caerá a defaults (localhost)

---

*Mantenido por el equipo ChillSell*
