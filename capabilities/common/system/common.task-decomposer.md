# Capability: `common.task-decomposer`

**Nombre**: Task Decomposer
**Versión**: 1.0.0

## Propósito
Descompone un objetivo de ingeniería en un task-graph determinístico (DAG). Produce fases con dependencias, skills y subagentes por nodo, compatible con roadmap.json de higpertext. Motor heurístico (NO LLM): plantillas por tipo de tarea (refactor/feature/bugfix/review). El resultado es un punto de partida para iterar.

**Entrypoint**: `capabilities/common/scripts/core/system/task_decomposer.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--goal` | Sí | Objetivo en lenguaje natural, ej: 'implementar el Efficiency Meter'. |
| `--type` | No | Tipo de tarea: refactor|feature|bugfix|review. Default: feature. |
| `--save` | No | Si 'true', escribe .higpertext/config/roadmap.json. Default: true. |

## Contrato Técnico (Reglas)

- El grafo generado debe ser un DAG válido (sin ciclos).
- Para tipo 'refactor', el primer nodo debe ser de exploración.
- Cada nodo debe declarar sus skills y subagents.
- El output debe ser compatible con el formato de roadmap.json de higpertext.
- Si --save=true, debe escribir en .higpertext/config/roadmap.json.
- El decomposer NO debe invocar ningún LLM — solo heurísticas determinísticas.
<!-- higpertext:generated-by=common.docs-sync -->
