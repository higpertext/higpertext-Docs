# Capability: `common.load-rules`

**Nombre**: Load Rules
**Versión**: 1.0.0

## Propósito
Carga las reglas detalladas de capacidades seleccionadas al contexto del LLM activo. Escribe un archivo de reglas en la ruta correspondiente al asistente (.claude/rules/, .opencode/rules/, etc.) para que sea cargado automáticamente en el siguiente turno.

**Entrypoint**: `capabilities/common/scripts/core/governance/load_rules.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--rules` | Sí | IDs de capacidades a cargar, separados por coma. Acepta short ID (grep-search) o full ID (common.grep-search). Usa 'all' para cargar todas las del perfil activo. |
| `--workflows` | No | IDs de workflows a cargar, separados por coma. Acepta short ID (higpertext-build) o full ID (workflow.higpertext-build). Usa 'all' para cargar todos los compilados. |
| `--clear` | No | Si es 'true', elimina el archivo session-capabilities.md (o el bloque equivalente) sin generar nuevo contenido. |

## Contrato Técnico (Reglas)

- Debe leer el asistente activo desde .higpertext/environment.json para determinar la ruta de destino.
- Debe leer el perfil activo para validar que los IDs solicitados existen en el perfil.
- Para claude y opencode: debe escribir .claude/rules/session-capabilities.md o equivalente.
- Para gemini, copilot y antigravity: debe reemplazar el bloque session-capabilities en el archivo principal si existe, o appenderlo si no.
- Si un ID no existe en el perfil, debe fallar con mensaje descriptivo antes de escribir.
- Debe reportar cuántas capacidades se cargaron y en qué archivo.
<!-- higpertext:generated-by=common.docs-sync -->
