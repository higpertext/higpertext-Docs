# Capability: `common.context-assembler`

**Nombre**: Context Assembler
**Versión**: 1.0.0

## Propósito
Ensambla un 'context pack' curado para una tarea concreta. Dado un objetivo y tipo, extrae keywords, selecciona del semantic graph solo los símbolos relevantes bajo un presupuesto de tokens, y genera un artefacto Markdown en .higpertext/state/context_packs/. Provee al agente el contexto mínimo-suficiente en vez de explorar el repo a ciegas.

**Entrypoint**: `capabilities/common/scripts/core/context/context_assembler.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--goal` | Sí | Objetivo de la tarea en lenguaje natural, ej: 'refactorizar el sistema de hooks de sesión'. |
| `--type` | No | Tipo de tarea: refactor|feature|bugfix|review. Default: feature. |
| `--budget` | No | Presupuesto máximo de tokens del pack. Default: 8000. |

## Contrato Técnico (Reglas)

- Debe extraer keywords del objetivo filtrando stopwords (ES/EN).
- Debe seleccionar símbolos del semantic graph relevantes a los keywords.
- El pack generado nunca debe exceder el presupuesto de tokens indicado.
- Debe persistir el artefacto Markdown en .higpertext/state/context_packs/.
- Debe reportar el número de símbolos seleccionados y los tokens estimados.
- Si el semantic graph no existe, debe generar un pack vacío sin fallar.
<!-- higpertext:generated-by=common.docs-sync -->
