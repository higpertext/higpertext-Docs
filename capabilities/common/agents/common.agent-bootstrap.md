# Capability: `common.agent-bootstrap`

Instala higpertext en el entorno virtual de un agente externo ya registrado y vuelve a sincronizar su integración. Sirve para que el agente sea portable y no dependa de la ruta de tu checkout del motor.

```bash
htx task common.agent-bootstrap --name mi-agente --assistant codex
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--name` | Sí | Nombre del agente registrado |
| `--mode` | No | `editable` (predeterminado) o `wheel` |
| `--engine-source` | No | Checkout del motor que se instalará |
| `--assistant` | No | `codex`, `claude`, `gemini`, `opencode`, `copilot` o `antigravity` |
| `--force` | No | Recrea el entorno del agente |

Debe ejecutarse después de registrar el agente con `common.agent-sync`.
