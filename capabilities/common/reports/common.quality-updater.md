# Capability: `common.quality-updater`

**Nombre**: common.quality-updater
**Versión**: 1.0.0

## Propósito
Actualiza el archivo del plan de remediación progresiva (.higpertext/reportes/remediation_todo.md) marcando las tareas corregidas como [x] y añadiendo nuevas violaciones detectadas.

**Entrypoint**: `capabilities/common/scripts/core/quality_resolver.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--report` | No | Nombre o ruta del reporte de calidad en .higpertext/reportes/. |
| `--todo_file` | No | Nombre o ruta del archivo TODO checklist a actualizar. |
| `--mode` | No | Modo de actualización: 'update' para actualizar progresivamente, 'create' para sobreescribir. |

## Contrato Técnico (Reglas)

- Debe leer el checklist de remediación anterior si existe.
- Debe comparar las violaciones actuales con las anteriores.
- Debe marcar como completadas [x] las violaciones resueltas.
- Debe añadir como pendientes [ ] las nuevas violaciones detectadas.
<!-- higpertext:generated-by=common.docs-sync -->
