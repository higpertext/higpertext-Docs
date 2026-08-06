# Capability: `common.llm-invoke`

**Nombre**: common.llm-invoke
**Versión**: 1.0.0

## Propósito
Invoca un modelo LLM por API (Anthropic, OpenAI, Gemini, Ollama). Soporta completion y streaming.

**Entrypoint**: `capabilities/common/scripts/core/system/llm_invoke.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--prompt` | Sí | Prompt principal a enviar al modelo. |
| `--provider` | No | Provider a usar: anthropic | openai | gemini | ollama. Default: según environment.json o HIGPERTEXT_LLM_PROVIDER. |
| `--model` | No | ID del modelo. Default: según environment.json del provider. |
| `--system` | No | System prompt opcional. |
| `--max_tokens` | No | Tokens máximos de respuesta. Default: 1024. |
| `--temperature` | No | Temperatura de sampling (0.0-1.0). Default: 0.7. |
| `--stream` | No | Activar streaming (true/false). Default: false. |
| `--output_file` | No | Ruta de archivo donde guardar el resultado. Opcional. |

## Contrato Técnico (Reglas)

- El prompt no puede estar vacío.
- El provider debe ser uno de: anthropic, openai, gemini, ollama.
- Las API keys deben estar en variables de entorno, nunca en parámetros.
- El output incluye el contenido generado y métricas de tokens.
<!-- higpertext:generated-by=common.docs-sync -->
