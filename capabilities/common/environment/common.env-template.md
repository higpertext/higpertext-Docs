# Capability: `common.env-template`

**Nombre**: Environment Template
**Versión**: 1.0.0

## Propósito
Crea, valida o muestra templates locales de entornos Docker/Podman.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_template.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | validate, show o create. |
| `--template` | Sí | ID del template. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe crear templates bajo .higpertext/env/templates.
- Debe validar templates locales o globales.
- Debe soportar JSON para show/validate.
<!-- higpertext:generated-by=common.docs-sync -->
