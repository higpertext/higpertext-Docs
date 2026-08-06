# Capability: `common.subagent-executor`

**Nombre**: Ejecutor de Subagentes
**Versión**: 1.0.0

## Propósito
Lanza la ejecución aislada de un subagente especializado para resolver una subtarea.

**Entrypoint**: `python src/capabilities/common/scripts/core/agents/subagent_executor.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--agent` | Sí | Nombre del subagente a ejecutar (ej: test-engineer). |
| `--task` | Sí | Descripción detallada de la subtarea a resolver. |
| `--timeout` | No | Tiempo máximo de ejecución en segundos. Default: `120`. |

## Contrato técnico

- Valida que `--agent` y `--task` no estén vacíos.
- Resuelve el subagente desde el entorno activo.
- Ejecuta el provider LLM configurado y devuelve un resultado real.
- Emite `[SUCCESS]` solo con una respuesta no vacía.
- Emite `[ERROR]` ante agente inexistente, provider no configurado, timeout o respuesta vacía.
- Nunca expone variables de entorno, credenciales ni secretos.
<!-- higpertext:generated-by=common.docs-sync -->
