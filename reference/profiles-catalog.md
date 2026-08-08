# Catálogo de perfiles

Un perfil define el rol, las capacidades y los recursos de sesión disponibles para un asistente. El motor incluye seis perfiles; los perfiles de dominio pertenecen a agentes externos.

| Perfil | Propósito | Capacidades |
|---|---|---:|
| `global` | Trabajo transversal y exploración general | 18 |
| `base_developer` | Desarrollo y calidad de código | 7 |
| `base_operator` | Operación y diagnóstico básico | 3 |
| `base_auditor` | Seguridad, cambios Git y auditoría | 3 |
| `base_agent` | Base para construir un agente externo | 9 |
| `agent_designer` | Diseño, creación y sincronización de agentes externos | 24 |

## Cargar un perfil

```bash
htx init --assistant codex
htx profile load base_developer --assistant codex
```

Elige `codex`, `claude`, `gemini`, `opencode`, `copilot` o `antigravity`. El perfil aplica al proyecto donde ejecutas el comando.

## `global`

Para explorar, buscar, leer con presupuesto de contexto, iniciar sesiones y guardar memoria o investigaciones. Incluye `common.project-explainer`, `common.search-router`, `common.grep-search`, `common.smart-read`, `common.file-map`, `common.memory-manager` y `common.investigation-report`.

## `base_developer`

Para cambios de código y calidad. Incluye `git.diff`, `git.committer`, `common.higpertext-tester`, `common.quality-resolver`, `common.code-skeletonizer`, `common.agent-builder` y `common.eval-agent`.

## `base_operator`

Para inspección operativa básica: `git.diff`, `git.committer` y `common.code-skeletonizer`.

## `base_auditor`

Para revisar secretos y cambios: `security.secret-scanner`, `git.diff` y `git.committer`.

## `base_agent`

Base mínima para crear un agente externo. Incluye búsqueda, conocimiento, reglas, memoria, sesiones y `common.agent-builder`.

## `agent_designer`

Es el perfil recomendado para crear y mantener agentes externos. Añade creación, registro, bootstrap, sincronización y detección de drift de agentes, además de capacidades de exploración y sesiones.

Consulta [Agentes externos](../getting-started/external-agents.md) para su ciclo completo. Para parámetros exactos, usa el [catálogo de capacidades](capabilities-catalog.md).
