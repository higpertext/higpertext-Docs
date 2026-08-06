# Capability: `common.semantic-diff`

**Nombre**: common.semantic-diff
**Versión**: 1.0.0

## Propósito
Detecta qué funciones, clases y métodos cambiaron entre dos commits (o entre HEAD y el working tree), usando AST parsing. Útil para identificar qué tests reejecutar tras un cambio.

**Entrypoint**: `capabilities/common/scripts/core/search/semantic_diff.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--base` | No | Commit/branch de referencia base (default: HEAD~1). |
| `--head` | No | Commit/branch destino a comparar (default: HEAD). |
| `--files` | No | Lista de archivos específicos separados por comas (opcional). |
| `--format` | No | Formato de salida: 'text' o 'json' (default: text). |

## Contrato Técnico (Reglas)

- Listar funciones y clases modificadas, añadidas o eliminadas entre los dos commits.
- Agrupar los cambios por archivo.
- Indicar el tipo de cambio: added, removed, modified.
- Si format=json, devolver un objeto JSON estructurado con la lista de símbolos cambiados.
<!-- higpertext:generated-by=common.docs-sync -->
