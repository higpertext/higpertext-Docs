# Capability: `common.roadmap-report`

**Nombre**: Roadmap Report
**Versión**: 1.0.0

## Propósito
Genera un reporte Markdown explicativo del roadmap: progreso por fase, lógica del trabajo, skills y subagents usados, commits relacionados y cambios de código. Al listar roadmaps completados se actualiza automáticamente.

**Entrypoint**: `capabilities/common/scripts/core/reports/roadmap_report.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--roadmap` | No | Path al archivo roadmap JSON. Default: roadmap activo en .higpertext/config/roadmaps/. |
| `--commit-range` | No | Rango Git exacto a documentar, por ejemplo `HEAD~10..HEAD`. |
| `--output` | No | Ruta de salida del reporte. Default: .higpertext/reports/roadmap/<roadmap-id>_report.<ext>. |

## Contrato Técnico (Reglas)

- Debe leer el roadmap indicado o el roadmap activo en .higpertext/config/roadmaps/.
- Debe mostrar resumen ejecutivo en lenguaje natural con % de completitud.
- Debe mostrar todas las fases con su status (done/active/pending) e iconos visuales.
- Debe listar skills y subagents por fase y totales.
- Debe listar commits relacionados con hash, fecha, mensaje y diff completo.
- Debe generar únicamente Markdown.
- Debe completar con [SUCCESS] o [ERROR] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
