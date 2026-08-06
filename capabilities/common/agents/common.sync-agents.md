# Capability: `common.sync-agents`

**Nombre**: common.sync-agents
**Versión**: 1.0.0

## Propósito
Sincroniza y proyecta las reglas canónicas de AgentSystem (workflows primarios y subagentes) hacia las carpetas específicas del asistente IA en el proyecto destino.

**Entrypoint**: `capabilities/common/scripts/core/agents/sync_agents.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--target` | No | Ruta al proyecto destino donde proyectar las reglas y subagentes (opcional, por defecto el proyecto actual). |
| `--source` | No | Ruta origen de AgentSystem (opcional, por defecto se detecta automáticamente o vía variable de entorno AGENT_SYSTEM_PATH). |
| `--assistant` | No | Asistente objetivo para proyectar (gemini, claude, copilot, opencode, antigravity, all). |

## Contrato Técnico (Reglas)

- Proyectar flujos de trabajo primarios (spec, plan, build, review) en las rutas del asistente.
- Proyectar subagentes especializados en subagents/.
- Generar un reporte claro en consola con la lista de archivos creados o actualizados.
<!-- higpertext:generated-by=common.docs-sync -->
