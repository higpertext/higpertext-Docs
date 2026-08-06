# Capability: `common.training-recommender`

**Nombre**: Training Recommender
**Versión**: 1.0.0

## Propósito
Analiza la telemetría higpertext y sugiere acciones de entrenamiento del agente: adopción baja, capacidades subutilizadas y herramientas de alto costo.

**Entrypoint**: `capabilities/common/scripts/core/reports/training_recommender.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--days` | No | Período en días a analizar (default: 7). |

## Contrato Técnico (Reglas)

- Lee únicamente .higpertext/state/telemetry.jsonl — nunca expone tokens de API ni variables de entorno.
- Si no hay datos suficientes, muestra mensaje informativo sin error.
- Las recomendaciones son heurísticas basadas en umbrales fijos, no juicios definitivos.
<!-- higpertext:generated-by=common.docs-sync -->
