# Capability: `common.agent-builder`

**Nombre**: Agent Builder
**Versión**: 1.0.0

## Propósito
Crea la estructura base de un nuevo agente higpertext independiente: scaffolding de carpetas, perfil inicial, htx.py launcher y ambiente .higpertext/ compilado. El agente generado usa el motor central sin copiar src/core/.

**Entrypoint**: `capabilities/common/scripts/core/agents/agent_builder.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--profile` | Sí | Nombre del perfil inicial del agente (ej: content_creator). No puede ser un nombre reservado del motor. |
| `--target` | Sí | Ruta del directorio donde se creará el agente. Si no existe, se crea automáticamente. |
| `--description` | No | Descripción del agente para incluir en el perfil generado. |

## Contrato Técnico (Reglas)

- Debe crear src/capabilities/, src/config/profiles/, src/config/governance/, src/config/environments/, src/config/templates/, src/workflows/, src/config/hooks/profiles/, src/config/hooks/custom/, content/.
- Debe copiar agent_designer.json al directorio de perfiles del agente.
- Debe generar htx.py launcher apuntando al motor central — nunca copiar src/core/.
- Debe rechazar nombres de perfil reservados: base_agent, agent_designer, global, base_developer, base_operator, base_auditor.
- Debe compilar las capas de hooks en .higpertext/config/hooks_config.json.
- Debe generar el grafo semántico en .higpertext/state/semantic_graph.md.
- Debe ser idempotente: no sobreescribir htx.py ni src/config/hooks/custom/ si ya existen.
- El output debe indicar claramente la ruta del agente creado y el comando de verificación.

## Ejemplos de uso

### Crear un agente de análisis de datos
```bash
htx task common.agent-builder --profile data_analyst --target ../data-analyst-agent
```

### Crear un agente con descripción
```bash
htx task common.agent-builder --profile sre_bot --target ../sre-agent --description "Agente SRE para monitoreo y alertas"
```
<!-- higpertext:generated-by=common.docs-sync -->
