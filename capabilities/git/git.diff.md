# Capability: `git.diff`

**Nombre**: git.diff
**Versión**: 1.0.0

## Propósito
Detecta cambios locales en el repositorio Git (archivos modificados, eliminados, sin seguimiento) y muestra un resumen estructurado o las diferencias detalladas (diffs) para ayudar al agente en tareas de versionado y documentación.

**Entrypoint**: `capabilities/git/scripts/git_diff.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--detail` | No | Si es 'true', muestra el diff detallado del código. Si es 'false', solo lista los archivos. (default: 'false') |
| `--files` | No | Lista de archivos específicos separados por comas para analizar su diff. (opcional) |

## Contrato Técnico (Reglas)

- Mostrar el listado de archivos clasificados por su estado en Git (Staged, Unstaged, Untracked).
- Si el parámetro detail está activo, formatear el diff en un bloque markdown legible con sintaxis diff.

## Intercepción de Bash

- **Patrón**: `\bgit\s+(diff|status|log)\b`
- **Descripción**: Ver diff/estado del repositorio
- **Ejemplo de reemplazo**: `htx task git.git-diff --detail true`
<!-- higpertext:generated-by=common.docs-sync -->
