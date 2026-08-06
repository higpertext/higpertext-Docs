# Capability: `common.env-stop`

**Nombre**: Environment Stop
**Versión**: 1.0.0

## Propósito
Detiene un entorno local iniciado por env-runner.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_stop.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--run_id` | Sí | Run a detener. |
| `--volumes` | No | Eliminar volúmenes. Default: false. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe ejecutar compose down para el run.
- Debe actualizar estado a stopped o failed.
<!-- higpertext:generated-by=common.docs-sync -->
