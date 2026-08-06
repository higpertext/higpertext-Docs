# Capability: `common.commit-report`

**Nombre**: Commit Report Explicativo
**Versión**: 1.0.0

## Propósito
Genera un reporte Markdown explicativo de un commit o rango de commits: resumen en lenguaje natural, archivos impactados por capa DDD, estadísticas, líneas modificadas y análisis de impacto.

**Entrypoint**: `capabilities/common/scripts/core/reports/commit_report.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--commit` | No | Hash del commit a analizar. Acepta HEAD, HEAD~N o hash corto. Default: HEAD. |
| `--range` | No | Rango de commits en formato git (e.g. HEAD~5..HEAD). Si se especifica, sobreescribe --commit. |
| `--no-diff` | No | Omite el diff completo línea por línea. Por defecto se incluye. |
| `--output` | No | Ruta de salida del reporte. Default: .higpertext/reports/commits/<hash>_report.<ext>. |

## Contrato Técnico (Reglas)

- Debe leer el historial git real del repositorio.
- Debe mostrar resumen ejecutivo en lenguaje natural.
- Debe agrupar archivos impactados por capa: domain, application, infrastructure, test, capability, other.
- Debe mostrar estadísticas: archivos cambiados, líneas añadidas, líneas eliminadas.
- Debe indicar si el commit incluye nuevos tests o nuevas capacidades.
- Debe incluir las líneas modificadas y el diff completo por defecto.
- Debe completar con [SUCCESS] o [ERROR] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
