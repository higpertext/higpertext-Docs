# Capability: `common.env-logs`

**Nombre**: Environment Logs
**Versión**: 1.0.0

## Propósito
Obtiene logs compactos de un run Docker/Podman, con filtros por servicio, tail y errores.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_logs.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--run_id` | Sí | Run a consultar. |
| `--service` | No | Servicio opcional. |
| `--tail` | No | Cantidad de líneas. Default: 200. |
| `--errors_only` | No | Filtra líneas con error/failed/exception. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe usar el backend del run persistido.
- Debe limitar logs con tail.
- Debe soportar errors_only.
<!-- higpertext:generated-by=common.docs-sync -->
