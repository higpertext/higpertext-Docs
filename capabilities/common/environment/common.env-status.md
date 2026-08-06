# Capability: `common.env-status`

**Nombre**: Environment Status
**Versión**: 1.0.0

## Propósito
Consulta runs de entornos locales ejecutados por env-runner.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_status.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--run_id` | No | Run específico; si se omite lista todos. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe leer .higpertext/env/runs.
- Debe soportar consulta por run_id y listado general.
<!-- higpertext:generated-by=common.docs-sync -->
