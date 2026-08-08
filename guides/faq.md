# FAQ

## ¿Qué necesito para empezar a trabajar?

En la raíz del proyecto: `htx init --assistant <asistente>` y después `htx profile load <perfil> --assistant <asistente>`. Con eso el asistente ya tiene su integración y perfil; abrir una sesión es opcional.

## ¿Qué perfiles trae el motor?

`global`, `base_developer`, `base_operator`, `base_auditor`, `base_agent` y `agent_designer`. Perfiles como `sre` o `devsecops` solo existen si un agente externo los define.

## ¿Qué asistentes soporta?

`codex`, `claude`, `gemini`, `opencode`, `copilot` y `antigravity`. Usa el mismo valor en `init` y `profile load`.

## ¿Cuál es la diferencia entre motor y agente externo?

El motor ofrece runtime, integración, perfiles base y capacidades transversales. Un agente externo es dueño de la lógica del dominio, sus perfiles, capabilities y pruebas. Consulta [Agentes externos](../getting-started/external-agents.md).

## ¿Cómo encuentro una tarea?

Usa el [mapa de capacidades](../capabilities/walkhrough.md) para partir de tu objetivo y el [catálogo técnico](../reference/capabilities-catalog.md) para los parámetros exactos. Prefiere IDs completos, por ejemplo `common.memory-manager`.

## ¿Cuándo uso una sesión?

Solo al necesitar skills, subagentes o comandos temporales: `htx task common.session-start --action start`. Para cerrarla usa `htx task common.session-clean --action clean`.

## ¿Dónde guardo claves de proveedores LLM?

No las añadas a comandos ni archivos versionados. Usa `security.secret-set`, que emplea el llavero del sistema operativo.

## ¿Cómo verifico el estado?

Ejecuta `htx task common.higpertext-tester` para comprobar contratos o `htx task common.doctor` para diagnosticar el workspace.
