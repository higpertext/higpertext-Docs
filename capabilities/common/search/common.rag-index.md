# Capability: `common.rag-index`

**Nombre**: RAG Indexer
**Versión**: 1.0.0

## Propósito
Indexa el código fuente y documentación del proyecto para habilitar la búsqueda semántica.

**Entrypoint**: `capabilities/common/scripts/core/search/rag_index.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--root` | No | Ruta al directorio raíz del proyecto. |

## Contrato Técnico (Reglas)

- Debe generar fragmentos semánticos usando AST.
- Debe generar embeddings y guardarlos en .higpertext/state/vector_store.json.
- Usa el proveedor local por defecto (`paraphrase-multilingual-MiniLM-L12-v2`).
- Respeta `HIGPERTEXT_EMBEDDING_PROVIDER=gemini` para usar Gemini.
- Genera embeddings por lote cuando el proveedor lo soporta.
<!-- higpertext:generated-by=common.docs-sync -->
