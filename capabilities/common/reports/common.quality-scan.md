# Capability: `common.quality-scan`

**Nombre**: Quality Scan
**Versión**: 1.0.0

## Propósito
Genera .higpertext/reports/code_quality_report.md analizando archivos Python con quality gates básicos.

**Entrypoint**: `capabilities/common/scripts/core/reports/quality_scan.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--target` | No | Directorio o archivo Python objetivo. |
| `--output` | No | Ruta del reporte Markdown generado. |
| `--max_function_lines` | No | Máximo de líneas por función. |
| `--max_class_lines` | No | Máximo de líneas por clase. |

## Contrato Técnico (Reglas)

- Debe generar un reporte Markdown en .higpertext/reports/code_quality_report.md por defecto.
- Debe listar archivos analizados con puntuación /100.
- Debe incluir la sección 'Penalizaciones aplicadas' compatible con common.quality-resolver.
<!-- higpertext:generated-by=common.docs-sync -->
