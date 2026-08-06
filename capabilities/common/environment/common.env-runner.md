# Capability: `common.env-runner`

**Nombre**: Environment Runner
**Versión**: 1.0.0

## Propósito
Levanta un entorno local Docker/Podman desde template, ejecuta una tarea/comando y persiste estado/logs por run_id.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_runner.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--template` | Sí | ID del template a ejecutar. |
| `--engine` | No | auto, docker o podman. |
| `--task` | No | Tarea definida por el template. |
| `--command` | No | Comando override a ejecutar dentro del servicio. |
| `--service` | No | Servicio donde ejecutar el comando. |
| `--detach` | No | Si true deja el entorno corriendo. |
| `--parallel` | No | Reservado; cada run ya usa project_name único. |
| `--timeout` | No | Timeout en segundos. Default: 300. |
| `--keep_alive` | No | No ejecutar down al terminar. |
| `--cleanup` | No | Ejecutar down al terminar si no keep_alive. Default: true. |
| `--env` | No | Variables KEY=value separadas por coma. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe seleccionar Docker o Podman mediante --engine.
- Debe crear run_id y estado bajo .higpertext/env/runs.
- Debe capturar logs de up/command/down.
- Debe aplicar timeout y cleanup por defecto.
<!-- higpertext:generated-by=common.docs-sync -->
