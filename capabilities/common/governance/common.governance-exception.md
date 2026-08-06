# Capability: `common.governance-exception`

**Nombre**: Governance Exception
**Versión**: 1.0.0

## Propósito
Registra o lista excepciones aprobadas a reglas de gobernanza para un perfil.

**Entrypoint**: `capabilities/common/scripts/core/governance/governance_exception.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción: register | list |
| `--rule_id` | No | ID de la regla a exceptuar (requerido para register) |
| `--reason` | No | Justificación de la excepción |
| `--approver` | No | Quien aprueba la excepción |
| `--expires` | No | Fecha de expiración ISO (YYYY-MM-DD) |
| `--profile` | No | Perfil al que aplica (por defecto el activo) |

## Contrato Técnico (Reglas)

- No puede exceptuar reglas de severidad critical.
- Toda excepción debe tener reason y approver.
- Las excepciones expiradas no se aplican aunque existan en el store.
<!-- higpertext:generated-by=common.docs-sync -->
