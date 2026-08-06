# Capability: `common.efficiency-meter`

**Nombre**: Efficiency Meter
**Versión**: 1.0.0

## Propósito
Mide la eficiencia de una sesión de agente cruzando telemetry.jsonl con los context packs generados. Calcula: tokens totales, costo estimado, exploration_waste_ratio (lecturas sin higpertext / total reads), context_hit_rate (archivos del pack realmente leídos) y hook_intercepts. Retroalimenta la calidad de los context packs.

**Entrypoint**: `capabilities/common/scripts/core/reports/efficiency_meter.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--session` | No | ID de sesión a analizar. Si no se indica, usa la sesión activa. |
| `--format` | No | Formato de salida: table|json|markdown. Default: table. |

## Contrato Técnico (Reglas)

- Si no se indica --session, debe leer la sesión activa de .higpertext/state/session.json.
- context_hit_rate debe estar en el rango [0, 1].
- exploration_waste_ratio debe estar en el rango [0, 1].
- Si no hay eventos de telemetría para la sesión, debe devolver métricas en cero sin fallar.
- El formato 'json' debe producir JSON válido serializable.
<!-- higpertext:generated-by=common.docs-sync -->
