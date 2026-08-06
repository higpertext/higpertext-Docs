# Capability: `common.doctor`

**Nombre**: Higpertext Doctor
**Versión**: 1.0.0

## Propósito
Ejecuta un diagnóstico integral del workspace: launcher, entorno, capabilities, hooks, caché y reportes.

**Entrypoint**: `capabilities/common/scripts/core/session/doctor.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe validar launcher htx disponible.
- Debe validar catálogo de capabilities y hooks críticos.
- Debe revisar estado básico de caché y reportes.
<!-- higpertext:generated-by=common.docs-sync -->
