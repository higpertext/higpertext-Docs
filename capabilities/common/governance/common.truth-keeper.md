# Capability: `common.truth-keeper`

**Nombre**: Truth Keeper
**Versión**: 1.0.0

## Propósito
Lee, escribe y gestiona el contexto de la Fuente de Verdad en .memory/truth.json para consulta rápida por agentes.

**Entrypoint**: `src/higpertext/capabilities/common/scripts/core/governance/truth_keeper.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción a realizar: set, get, delete, list. |
| `--domain` | No | El dominio/namespace bajo el cual guardar o consultar la información. |
| `--key` | No | La llave específica del dato a consultar, definir o eliminar. |
| `--value` | No | El valor a guardar. |
| `--description` | No | Descripción para el dominio (usado al crear/actualizar un dominio). |

## Contrato Técnico (Reglas)

- Debe crear automáticamente el directorio .memory/ y el archivo truth.json si no existen.
- La acción 'set' requiere '--domain' y '--key'.
- La acción 'get' con '--domain' retorna el contexto de ese dominio. Sin '--domain' retorna la Fuente de Verdad completa.
- Debe retornar salida formateada en JSON estructurado cuando se use 'get' o 'list'.
- Debe emitir [SUCCESS] al completar una operación de mutación correctamente.
- Debe emitir [ERROR] ante fallos de formato o argumentos ausentes.
<!-- higpertext:generated-by=common.docs-sync -->
