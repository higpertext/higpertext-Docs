# Capability: `common.graph-visualize`

**Nombre**: Graph Visualize
**Versión**: 1.0.0

## Propósito
Genera un grafo HTML interactivo con D3.js a partir del semantic_graph.json. El archivo se guarda en .higpertext/reports/semantic_graph.html y se puede abrir en el navegador.

**Entrypoint**: `capabilities/common/scripts/core/search/graph_visualize.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--output` | No | Ruta de salida del archivo HTML. Default: .higpertext/reports/semantic_graph.html. |
| `--max_nodes` | No | Límite de nodos a renderizar para evitar grafos ilegibles. Default: 200. |
| `--god_threshold` | No | Umbral de in-degree para resaltar god nodes en rojo. Default: 5. |

## Contrato Técnico (Reglas)

- Debe leer .higpertext/state/semantic_graph.json. Si no existe, indicar que hay que ejecutar common.graph-rebuild primero.
- Debe generar un archivo HTML self-contained (sin dependencias externas — D3 embebido o CDN).
- Debe resaltar god nodes visualmente (color distinto).
- Debe indicar la ruta del archivo generado al finalizar.
- Debe completar con [SUCCESS] o [ERROR] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
