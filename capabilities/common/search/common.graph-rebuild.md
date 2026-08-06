# Capability: `common.graph-rebuild`

**Nombre**: Graph Rebuild
**Versión**: 1.0.0

## Propósito
Regenera el grafo semántico del proyecto parseando todos los archivos Python con AST. Persiste el resultado en .higpertext/state/semantic_graph.json y actualiza el resumen en .higpertext/state/semantic_graph.md.

**Entrypoint**: `capabilities/common/scripts/core/search/graph_rebuild.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--root` | No | Directorio raíz del proyecto a analizar. Por defecto el directorio actual. |
| `--god_threshold` | No | Umbral de in-degree para clasificar un nodo como god node. Default: 5. |

## Contrato Técnico (Reglas)

- Debe generar .higpertext/state/semantic_graph.json con campos symbols y relations.
- Debe imprimir el total de símbolos y relaciones indexados al finalizar.
- Debe indicar cuántos god nodes fueron detectados.
- Nunca debe exponer contenido de archivos .env o secrets.
- Debe completar con [SUCCESS] o [ERROR] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
