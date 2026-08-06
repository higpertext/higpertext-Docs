# Capability: `common.search-router`

**Nombre**: Search Router
**Versión**: 1.0.0

## Propósito
Recomienda el plan de capacidades adecuado para localizar contexto: error-context-locator, smart-read, graph-query o grep-search según intención y query.

**Entrypoint**: `capabilities/common/scripts/core/search/search_router.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--query` | Sí | Consulta, símbolo, path o error. |
| `--intent` | No | error, feature, refactor, docs, symbol o general. |
| `--scope` | No | Ruta base para grep-search. Default: . |
| `--preset` | No | Preset para grep-search. Default: code. |
| `--budget` | No | Presupuesto de tokens sugerido. Default: 4000. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe recomendar error-context-locator para intención error o trazas.
- Debe recomendar graph-query para feature/refactor/symbol.
- Debe incluir grep-search como fallback general con límites de contexto.
- Debe soportar salida JSON.
<!-- higpertext:generated-by=common.docs-sync -->
