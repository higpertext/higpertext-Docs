# Capability: `common.list-rules`

**Nombre**: List Rules
**Versión**: 1.0.0

## Propósito
Lista todas las capacidades disponibles para el perfil activo con su namespace, ID corto y descripción de una línea. Útil para descubrir qué capacidades están disponibles antes de invocar load-rules.

**Entrypoint**: `capabilities/common/scripts/core/governance/list_rules.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--type` | No | Tipo de recursos a listar: 'capabilities', 'workflows' o 'all'. |

## Contrato Técnico (Reglas)

- Debe leer el perfil activo desde .higpertext/environment.json.
- Debe cruzar los IDs del perfil con los JSONs en src/capabilities/.
- Debe mostrar una tabla con columnas: NAMESPACE, ID, DESCRIPCIÓN.
- Debe indicar el total de capacidades al finalizar.
- Si un JSON de capacidad no existe, debe marcarlo como '(JSON no encontrado)' sin fallar.
- Debe imprimir el comando de ejemplo para invocar load-rules al finalizar.
<!-- higpertext:generated-by=common.docs-sync -->
