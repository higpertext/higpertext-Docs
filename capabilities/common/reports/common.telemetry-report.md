# Capability: `common.telemetry-report`

**Nombre**: Telemetry Report
**Versión**: 1.0.0

## Propósito
Muestra dashboard de telemetría higpertext en terminal: tokens estimados, costo, sesiones, commits y correlaciones.

**Entrypoint**: `capabilities/common/scripts/core/reports/telemetry_report.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--days` | No | Período en días a analizar (default: 7). |

## Contrato Técnico (Reglas)

- Lee únicamente .higpertext/telemetry.jsonl — nunca expone tokens de API ni variables de entorno.
- Si no hay datos, muestra mensaje informativo sin error.
- Tokens y costo son estimados (chars/4) — no son valores exactos del proveedor.
<!-- higpertext:generated-by=common.docs-sync -->
