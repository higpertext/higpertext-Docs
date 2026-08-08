# higpertext Engine — documentación

higpertext es un **motor de orquestación**: prepara un proyecto para un asistente de IA, carga un perfil y le entrega capacidades, reglas, hooks y recursos de sesión. No contiene la lógica de negocio de tu producto. Esa lógica vive en tu repositorio o en un **agente externo** creado con el motor.

## Empieza aquí

Con un ambiente inicializado y un perfil cargado ya puedes empezar a trabajar con el asistente:

```bash
# 1. Desde la raíz de tu proyecto
htx init --assistant codex

# 2. Carga un rol incluido en el motor
htx profile load base_developer --assistant codex

# 3. Consulta o ejecuta una capacidad
htx task common.list-rules
htx task common.project-explainer --action explain --target_path .
```

Usa `claude`, `gemini`, `opencode`, `copilot`, `antigravity` o `codex` según el asistente que vayas a integrar. El flujo completo está en [Primer arranque](getting-started/first-run.md).

## Guías de inicio

| Documento | Para qué sirve |
|---|---|
| [Instalación](getting-started/installation.md) | Instalar y verificar `htx` |
| [Primer arranque](getting-started/first-run.md) | Inicializar, cargar un perfil y comenzar |
| [Conceptos clave](getting-started/concepts.md) | Entender motor, proyecto, perfil, tarea y sesión |
| [Trabajar con agentes externos](getting-started/external-agents.md) | Crear, registrar y distribuir un agente que contiene tu lógica |

## Encuentra lo que necesitas hacer

| Si necesitas… | Empieza por… |
|---|---|
| Entender un proyecto | `common.project-explainer`, `common.file-map` |
| Buscar código o documentación | `common.search-router`, `common.grep-search`, `common.semantic-search` |
| Investigar un error | `common.error-context-locator`, `common.smart-read` |
| Preparar contexto para una tarea | `common.context-assembler` |
| Crear o mantener un agente externo | `common.agent-builder`, `common.agent-sync`, `common.agent-bootstrap` |
| Revisar el estado de hooks o del workspace | `common.doctor`, `common.hook-health` |
| Gestionar secretos de proveedores LLM | `security.secret-set` |

Consulta el [mapa de capacidades](capabilities/walkhrough.md) para elegir por objetivo, y el [catálogo técnico](reference/capabilities-catalog.md) para parámetros y contratos.

## Referencia

| Documento | Contenido |
|---|---|
| [Referencia CLI](reference/agent-cli.md) | Comandos principales de `htx` |
| [Catálogo de capacidades](reference/capabilities-catalog.md) | Las 53 capacidades registradas |
| [Catálogo de perfiles](reference/profiles-catalog.md) | Los 6 perfiles incluidos en el motor |
| [Catálogo de workflows](reference/workflows-catalog.md) | Los 5 workflows empaquetados |
| [Guía de hooks](guides/hooks-guide.md) | Qué automatizan los hooks |
| [Sesiones](guides/sessions.md) | Skills y subagentes temporales |

*higpertext Engine v0.8.5*
