# Capability: `common.hook-health`

**Nombre**: Hook Health
**Versión**: 1.0.0

## Propósito
Valida que los hooks de reducción de contexto estén registrados, detecten outputs grandes, bloqueen Read masivo y no recomienden flags inexistentes conocidos.

**Entrypoint**: `capabilities/common/scripts/core/session/hook_health.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe verificar hook_post_observer, hook_context_manager, hook_session_prompt, hook_read_guard y hook_bash_guard.
- Debe validar detección de output grande en PostToolUse.
- Debe validar bloqueo de Read grande con sugerencia common.smart-read.
- Debe fallar si encuentra recomendaciones de --source_path en hooks.
<!-- higpertext:generated-by=common.docs-sync -->
