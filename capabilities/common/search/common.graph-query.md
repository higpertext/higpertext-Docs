# Capability: `common.graph-query`

**Nombre**: Graph Query
**Versión**: 1.0.0

## Propósito
Consulta el grafo semántico para encontrar símbolos relacionados con un nombre o keyword, expandiendo vecinos hasta una profundidad configurable y permitiendo filtrar, limitar o emitir resultados compactos.

**Entrypoint**: `capabilities/common/scripts/core/search/graph_query.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--symbol` | Sí | Nombre o keyword del símbolo a buscar (búsqueda parcial, case-insensitive). |
| `--depth` | No | Profundidad de expansión de vecinos en el grafo. Default: 2. |
| `--budget` | No | Presupuesto de tokens para el resultado. Default: 8000. |
| `--type` | No | Filtra por tipo de símbolo: class, function, method, variable o module. |
| `--limit` | No | Máximo de símbolos a mostrar. Default: 50. |
| `--files_only` | No | Si es true, muestra solo archivos únicos relacionados. |
| `--json` | No | Si es true, emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe leer .higpertext/state/semantic_graph.json. Si no existe, indicar que hay que ejecutar common.graph-rebuild primero.
- Debe mostrar cada símbolo con su archivo, línea y tipo.
- Debe soportar filtro por tipo, límite de resultados, modo files_only y salida JSON.
- Debe indicar el total de símbolos retornados y tokens estimados.
- Debe completar con [SUCCESS] o [NOT FOUND] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
