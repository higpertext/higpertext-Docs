# Capability: `security.secret-set`

Gestiona la API key de un proveedor LLM en el llavero nativo del sistema. La clave nunca se pasa como argumento ni se escribe en `environment.json`.

```bash
htx task security.secret-set --action status --provider openai
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | `set`, `delete` o `status` |
| `--provider` | Sí | `anthropic`, `openai`, `gemini` u `ollama` |

Para `set`, usa `HTX_SECRET_VALUE` o la solicitud oculta interactiva si hay terminal disponible.
