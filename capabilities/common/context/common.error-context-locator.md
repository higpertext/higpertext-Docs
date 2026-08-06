# Capability: `common.error-context-locator`

**Nombre**: Error Context Locator
**Versión**: 1.0.0

## Propósito
Extrae file:line desde trazas, errores o logs y devuelve contexto mínimo con sugerencias de smart-read focalizado.

**Entrypoint**: `capabilities/common/scripts/core/context/error_context_locator.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--error` | No | Texto del error o traza. |
| `--error_file` | No | Archivo con error/log a analizar. |
| `--max_context` | No | Líneas alrededor de cada ubicación. Default: 5. |
| `--include_tests` | No | Incluye ubicaciones de tests. Default: true. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe detectar patrones Python File path, line N y file:line genéricos.
- Debe devolver contexto mínimo por ubicación detectada.
- Debe sugerir common.smart-read con around_line para cada ubicación.
- Debe soportar error inline, error_file, include_tests y JSON.
<!-- higpertext:generated-by=common.docs-sync -->
