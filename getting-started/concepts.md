# Conceptos clave

## El límite del motor

**higpertext Engine configura y orquesta; no implementa tu dominio.** El motor integra un asistente, carga perfiles y ejecuta capacidades. Tu aplicación conserva su propia lógica. Si quieres empaquetar un comportamiento especializado, lo creas en un agente externo con sus propias capabilities, perfiles, workflows y documentación.

## Las piezas

| Pieza | Qué es | Cuándo importa |
|---|---|---|
| Proyecto | El repositorio donde trabajas | Siempre; es donde ejecutas `htx init` |
| Asistente | Codex, Claude, Gemini, OpenCode, Copilot o Antigravity | Se elige con `--assistant` |
| Perfil | Rol y conjunto de capacidades disponibles | Se carga con `htx profile load` |
| Capacidad | Operación atómica con contrato y parámetros | Se ejecuta con `htx task <id>` |
| Workflow | Secuencia predefinida de tareas | Se ejecuta con `htx workflow run <id>` |
| Sesión | Montaje temporal de skills y subagentes | Solo cuando necesitas esos recursos |
| Agente externo | Proyecto independiente que añade lógica especializada | Cuando el motor base no cubre tu dominio |

## Flujo normal

```text
tu proyecto → htx init → htx profile load → trabajar con el asistente y htx task
```

No hace falta abrir una sesión para empezar. `session-start` es una ampliación temporal del perfil, no un requisito de instalación.

## Identificadores de capacidad

Usa siempre el ID completo para que la documentación, los scripts y los perfiles sean inequívocos:

```bash
htx task common.grep-search --pattern "class.*Service" --path src/
htx task git.diff --detail true
htx task security.secret-scanner --path .
```

→ [Mapa por objetivo](../capabilities/walkhrough.md) · [Catálogo técnico](../reference/capabilities-catalog.md)
