# Capability: `common.env-clean`

**Nombre**: Environment Clean
**Versión**: 1.0.0

## Propósito
Limpia metadata local de runs finalizados o todos los runs si --all=true.

**Entrypoint**: `capabilities/common/scripts/core/environment/env_clean.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--all` | No | Elimina todos los runs locales. Default: false. |
| `--older_than` | No | Reservado para limpieza por antigüedad. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe limpiar .higpertext/env/runs según estado.
- No debe ejecutar comandos destructivos fuera del directorio de runs.
<!-- higpertext:generated-by=common.docs-sync -->
