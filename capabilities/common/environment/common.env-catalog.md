# Capability: `common.env-catalog`

**Nombre**: Environment Catalog
**Versión**: 1.0.0

## Propósito
Lista templates locales/globales de entornos Docker/Podman y detecta motores disponibles.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_catalog.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--engine` | No | auto, docker o podman. Default: auto. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe listar templates de .higpertext/env/templates y templates del motor.
- Debe reportar disponibilidad Docker/Podman.
- Debe soportar JSON.
<!-- higpertext:generated-by=common.docs-sync -->
