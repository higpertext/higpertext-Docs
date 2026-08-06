# Capability: `common.eval-agent`

**Nombre**: common.eval-agent
**Versión**: 1.0.0

## Propósito
Ejecuta el framework de evaluación de modelos y configuración del higpertext Engine. Valida que los archivos generados contienen las secciones correctas, que los hooks responden bien y que el modelo se comporta según el perfil.

**Entrypoint**: `capabilities/common/scripts/core/system/eval_agent.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--profile` | No | Perfil higpertext a evaluar. Usa 'all' para evaluar todos los perfiles. |
| `--mode` | No | Modo de evaluación: static (sin API) | hooks (sin API) | behavioral (API) | safety (API) | all |
| `--provider` | No | Provider LLM para modos behavioral/safety: gemini | anthropic |

## Contrato Técnico (Reglas)

- Retorna exit code 0 si todos los tests pasan, 1 si hay fallos.
- Los modos behavioral y safety requieren RUN_BEHAVIORAL_TESTS=1.
- El modo static y hooks no requieren API keys y siempre deben pasar.
<!-- higpertext:generated-by=common.docs-sync -->
