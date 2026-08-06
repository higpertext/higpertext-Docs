# Capability: `common.report-viewer`

**Nombre**: Visualizador de Reportes Premium
**Versión**: 1.0.0

## Propósito
Compila la telemetría, el diario de decisiones de diseño y el análisis de desviación en un dashboard HTML autoportante interactivo.

**Entrypoint**: `capabilities/common/scripts/core/reports/report_viewer.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--output` | No | Ruta de destino del dashboard HTML resultante. |

## Contrato Técnico (Reglas)

- Debe consolidar todos los reportes Markdown generados por higpertext en una sola estructura unificada.
- Usa el motor common.html-presentation para compilar el HTML final.
- Debe copiar los assets necesarios (JS/CSS) de manera portable al directorio del reporte.
<!-- higpertext:generated-by=common.docs-sync -->
