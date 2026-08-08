# Referencia CLI

## Flujo esencial

```bash
htx init --assistant <codex|claude|gemini|opencode|copilot|antigravity>
htx profile load <perfil> --assistant <asistente>
```

`init` prepara la integración en el proyecto. `profile load` aplica un rol. Después de ambos comandos puedes empezar a trabajar; `session-start` no es obligatorio.

## Comandos principales

| Comando | Uso |
|---|---|
| `htx init --assistant <asistente> [--target <ruta>]` | Inicializa la integración del asistente |
| `htx profile load <perfil> --assistant <asistente> [--target <ruta>]` | Carga un perfil |
| `htx task <capability-id> [parámetros]` | Ejecuta una capacidad |
| `htx workflow run <workflow-id> [parámetros]` | Ejecuta un workflow |
| `htx roadmap <new|add|list|remove>` | Gestiona roadmaps del proyecto |
| `htx completion install` | Instala el autocompletado |
| `htx health` / `htx doctor` | Comprueba el motor o el workspace |

## Gestión de agentes externos

`htx agent` no integra un asistente: prepara y mantiene el estado compilado de un agente externo.

| Comando | Uso |
|---|---|
| `htx agent init --profile <perfil> [--target <ruta>]` | Compila el ambiente del agente |
| `htx agent status [--target <ruta>]` | Muestra su estado |
| `htx agent clean [--target <ruta>]` | Elimina solo estado efímero |
| `htx agent register --name <nombre> --path <ruta> --profile <perfil>` | Registra un agente |
| `htx agent sync [--name <nombre>] [--assistant <asistente>]` | Sincroniza agentes registrados |
| `htx agent list` | Lista agentes registrados |

Para el ciclo completo, incluidos portabilidad y sincronización, consulta [Agentes externos](../getting-started/external-agents.md).
