# Primer arranque

En tres pasos dejas tu proyecto listo para trabajar con un asistente. El motor no crea lógica de negocio: configura la integración y el perfil; tú y el agente trabajan sobre el código del proyecto.

## 1. Inicializa el proyecto

Desde la raíz del repositorio donde trabajarás:

```bash
htx init --assistant codex
```

Sustituye `codex` por `claude`, `gemini`, `opencode`, `copilot` o `antigravity` si corresponde. Este paso prepara los archivos de integración del asistente y el estado de higpertext; no modifica la lógica de tu aplicación.

## 2. Carga un perfil

Elige un perfil incluido en el motor y cárgalo para el mismo asistente:

```bash
htx profile load base_developer --assistant codex
```

Perfiles incluidos:

| Perfil | Úsalo cuando… |
|---|---|
| `global` | Necesitas capacidades transversales y orientación general |
| `base_developer` | Vas a desarrollar, explorar código y aplicar prácticas de calidad |
| `base_operator` | Vas a diagnosticar u operar servicios |
| `base_auditor` | Vas a auditar seguridad, cumplimiento o calidad |
| `base_agent` | Estás preparando la base de un agente externo |
| `agent_designer` | Vas a crear o evolucionar un agente externo |

Los nombres como `sre`, `devsecops` o `software_developer` pertenecen a agentes externos si esos agentes los definen; no son perfiles incluidos por este motor.

### Ejemplo: cargar el Agent Designer externo

El agente externo diseñado para crear y mantener otros agentes vive en su
propio repositorio: `https://github.com/higpertext/agent-designer.git`. Su
perfil `agent_designer` vive en ese repositorio, no en el motor. No necesitas
clonarlo ni cambiar de directorio: instala el perfil y sus recursos desde la
URL en el proyecto ya inicializado y después cárgalo.

```bash
# Desde la raíz de tu proyecto, después de htx init
htx profile install agent_designer \
  --source https://github.com/higpertext/agent-designer.git \
  --target .

htx profile load agent_designer --assistant codex
```

`profile install` obtiene el perfil, capabilities, plantillas y reglas que ese
agente expone y las incorpora al estado de tu proyecto; `profile load` lo
activa para Codex.
Con ello el Agent Designer queda listo para guiar la creación, activación y
mantenimiento de otros agentes. Si quieres sus recursos temporales, abre una
sesión después de cargar el perfil:

```bash
htx task common.session-start --action start --profile agent_designer
```

No copies ese perfil al repositorio del motor: `--source` conserva la separación
entre el motor y el agente externo.

## 3. Empieza a trabajar

El perfil ya deja al asistente con reglas y capacidades. Puedes empezar con una consulta o una tarea:

```bash
htx task common.list-rules
htx task common.project-explainer --action explain --target_path .
htx task common.search-router --intent "investigar un error" --query "mensaje de error"
```

Las sesiones son opcionales. Úsalas únicamente si necesitas montar skills, subagentes o los comandos de trabajo del asistente:

```bash
htx task common.session-start --action start
# al terminar
htx task common.session-clean --action clean
```

## Verificar

```bash
htx task common.higpertext-tester
```

→ [Conceptos clave](concepts.md) · [Agentes externos](external-agents.md)
