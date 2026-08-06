# Capability: `common.knowledge-asker`

**Nombre**: common.knowledge-asker
**Versión**: 1.0.0

## Propósito
Consulta la base de conocimiento de Gobernanza y Minutas para resolver dudas sobre normas y decisiones previas.

**Entrypoint**: `capabilities/common/scripts/core/ask_htx.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--query` | Sí | La pregunta o tema sobre el cual se desea obtener información de gobernanza. |

## Contrato Técnico (Reglas)

- Citar siempre el nombre del archivo fuente encontrado.
- Si no se encuentra información, sugerir consultar al administrador de gobernanza.
- Resumir los puntos clave de forma ejecutiva.

## Intercepción de Bash

- **Patrón**: `\bcat\s+.*\.(md|json|yaml|yml|txt)\b`
- **Descripción**: Consultar documentación o gobernanza
- **Ejemplo de reemplazo**: `htx task common.knowledge-asker --query "<pregunta>"`
<!-- higpertext:generated-by=common.docs-sync -->
