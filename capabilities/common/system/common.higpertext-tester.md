# Capability: `common.higpertext-tester`

**Nombre**: common.higpertext-tester
**Versión**: 1.0.0

## Propósito
Suite de validación y testing para asegurar la integridad de capacidades y el cumplimiento de contratos técnicos.

**Entrypoint**: `capabilities/common/scripts/core/system/higpertext_tester.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--level` | No | Nivel de prueba a ejecutar: integrity, smoke o full. |
| `--target` | No | Capacidad específica a testear (opcional, por defecto todas). |

## Contrato Técnico (Reglas)

- Generar un reporte de salud del motor en formato Markdown.
- Listar fallos de integridad (archivos faltantes) con prioridad crítica.
- Validar cumplimiento de reglas de contrato (Regex/Heurística).
<!-- higpertext:generated-by=common.docs-sync -->
