# Capability: `common.hook-sync-check`

**Nombre**: Hook Sync Check
**Versión**: 1.0.0

## Propósito
Compara los hooks desplegados en asistentes contra la fuente canónica del motor.

**Entrypoint**: `capabilities/common/scripts/core/session/hook_sync_check.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--json` | No | Emite JSON estructurado. |
| `--strict` | No | Si true, asistentes sin hooks desplegados fallan. |

## Contrato Técnico (Reglas)

- Debe comparar hashes de hooks fuente contra hooks desplegados.
- Debe reportar drift por asistente.
- Debe soportar salida JSON.
<!-- higpertext:generated-by=common.docs-sync -->
