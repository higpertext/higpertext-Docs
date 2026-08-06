# Capability: `common.hooks-manager`

**Nombre**: Hooks Manager
**Versión**: 1.0.0

## Propósito
Gestiona el ciclo de vida de hooks nativos de higpertext: listar, agregar, habilitar, deshabilitar y eliminar entradas en .higpertext/hooks_config.json. Regenera la configuración nativa del asistente activo tras cada cambio.

**Entrypoint**: `capabilities/common/scripts/core/session/hooks_manager.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción a ejecutar: list | add | remove | enable | disable | render |
| `--id` | No | ID del hook (requerido para remove, enable, disable). |
| `--event` | No | Evento del hook para --action add: PreToolUse | PostToolUse | Stop | Notification |
| `--script` | No | Ruta relativa al script del hook (requerido para --action add). |
| `--description` | No | Descripción del hook (opcional para --action add). |
| `--assistant` | No | Asistente para --action render: claude | gemini | antigravity | copilot. Por defecto usa el activo en environment.json. |
| `--profile` | No | Perfil para --action render. Por defecto usa el activo en environment.json. |

## Contrato Técnico (Reglas)

- Debe mostrar la lista completa de hooks con su estado (enabled/disabled) al usar --action list.
- Al agregar un hook debe validar que el evento sea uno de los soportados.
- Al remover un hook debe confirmar que el ID existe antes de eliminar.
- Tras enable/disable/add/remove debe llamar a HookRenderer para regenerar la config nativa del asistente activo.
- Nunca debe exponer variables de entorno o secrets en el output.
- Debe indicar claramente el número de hooks activos por perfil al listar.
<!-- higpertext:generated-by=common.docs-sync -->
