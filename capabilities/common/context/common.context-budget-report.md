# Capability: `common.context-budget-report`

**Nombre**: Context Budget Report
**Versión**: 1.0.0

## Propósito
Estima cuánto contexto consume una lectura, búsqueda o skeleton antes de ejecutarla y recomienda read range, skeleton, summary o grep.

**Entrypoint**: `capabilities/common/scripts/core/context/context_budget_report.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--path` | Sí | Archivo a estimar. |
| `--operation` | No | read, search o skeleton. Default: read. |
| `--budget` | No | Presupuesto de tokens. Default: 4000. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe estimar tokens por tamaño de archivo.
- Debe recomendar una estrategia de contexto antes de leer blobs grandes.
- Debe soportar JSON.
<!-- higpertext:generated-by=common.docs-sync -->
