# Capability: `common.session-start`

**Nombre**: Iniciar Sesión Temporal
**Versión**: 1.0.0

## Propósito
Bootstraps a temporal development session by mounting required skills, subagents, and compiling dynamic playbooks.

**Entrypoint**: `capabilities/common/scripts/core/session/session_control.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción a realizar: 'start' |
| `--profile` | No | Nombre del perfil activo. |
| `--assistant` | No | Asistente destino: codex, claude, gemini, antigravity, copilot, opencode. Por defecto usa el del environment.json. |
| `--skills` | No | Lista de skills separadas por comas. |
| `--subagents` | No | Lista de subagentes separados por comas. |
<!-- higpertext:generated-by=common.docs-sync -->
