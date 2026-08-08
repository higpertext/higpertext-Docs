# Capability: `common.quality-resolver`

Actualiza un checklist de remediación a partir de un reporte de calidad existente. No ejecuta el análisis; transforma sus resultados en tareas de seguimiento.

```bash
htx task common.quality-resolver --report code_quality_report.md
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--report` | No | Reporte de calidad; predeterminado: `code_quality_report.md` |
| `--todo_file` | No | Checklist resultante; predeterminado: `remediation_todo.md` |
| `--mode` | No | `update` conserva resueltas; `create` lo recrea |

Falla si el reporte no existe.
