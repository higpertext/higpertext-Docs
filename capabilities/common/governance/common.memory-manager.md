# Capability: `common.memory-manager`

**Nombre**: Gestor de Memoria de Acciones
**Versión**: 1.0.0

## Propósito
Persiste aprendizajes y estados después de cada acción del agente.

**Entrypoint**: `capabilities/common/scripts/core/governance/memory_manager.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Nombre de la acción realizada. |
| `--status` | Sí | Estado final de la acción. |
| `--notes` | Sí | Aprendizajes o notas técnicas de la acción. |
| `--action_id` | No | ID único de la acción (si no se proporciona se autogenera). |
| `--learned` | No | Aprendizajes clave obtenidos (separados por comas). |
| `--failure_root_cause` | No | Causa raíz de la falla si el estado es 'failure'. |
| `--tags` | No | Tags asociados a la acción (separados por comas). |

## Contrato Técnico (Reglas)

- Cada entrada debe incluir un 'action_id' único.
- El campo 'learned' debe ser una lista de aprendizajes clave.
- Si la acción falló, incluir 'failure_root_cause'.
- La memoria se guarda en la carpeta .memory/ del proyecto activo.
<!-- higpertext:generated-by=common.docs-sync -->
