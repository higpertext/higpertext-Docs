# Capability: `common.agent-sync`

**Nombre**: common.agent-sync
**Versión**: 1.0.0

## Propósito
Registra y sincroniza agentes externos con el motor higpertext: propaga hooks y perfiles actualizados.

**Entrypoint**: `src/capabilities/common/scripts/core/agents/agent_sync.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción: register | sync | list |
| `--name` | No | Nombre del agente |
| `--path` | No | Ruta absoluta al agente externo (para register) |
| `--profile` | No | Nombre del perfil del agente (para register) |
| `--assistant` | No | Asistente objetivo: claude|gemini|opencode|all (para sync) |
<!-- higpertext:generated-by=common.docs-sync -->
