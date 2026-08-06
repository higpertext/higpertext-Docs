# Capability: `common.semantic-search`

**Nombre**: Semantic Search
**Versión**: 1.0.0

## Propósito
Busca fragmentos de código y documentación semánticamente usando RAG.

**Entrypoint**: `capabilities/common/scripts/core/search/semantic_search.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--query` | Sí | Consulta o concepto semántico a buscar. |
| `--limit` | No | Número máximo de fragmentos a retornar. |
| `--root` | No | Ruta al directorio raíz del proyecto. |

## Contrato Técnico (Reglas)

- Debe calcular el embedding del query.
- Debe devolver una lista ordenada de fragmentos por similitud coseno en JSON.
- Usa el mismo proveedor y modelo configurados durante la indexación.
<!-- higpertext:generated-by=common.docs-sync -->
