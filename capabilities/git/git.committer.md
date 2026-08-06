# Capability: `git.committer`

**Nombre**: git.committer
**Versión**: 1.0.0

## Propósito
Realiza un commit en Git siguiendo el estándar Conventional Commits y opcionalmente hace push a la rama en Azure DevOps.

**Entrypoint**: `capabilities/git/scripts/commit_changes.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--message` | Sí | Mensaje del commit siguiendo Conventional Commits (ej. 'feat: añade capacidad de committer'). |
| `--files` | No | Archivos a incluir en el commit (por defecto '.' para todos). Especifique rutas separadas por espacios o comas si es selectivo. |
| `--rationale` | No | Justificación técnica, arquitectónica y decisiones de negocio asociadas a este commit (opcional). Si no se indica, se deducirá automáticamente. |
| `--branch` | No | Nombre de la rama destino para realizar 'git push origin <branch>' (opcional). |
| `--tag` | No | Si se especifica '--tag', crea un tag anotado y hace bump de versión automático tras el commit. Soporta Python (pyproject.toml/setup.py/setup.cfg), Node (package.json), Java (pom.xml) y .NET (*.csproj/*.fsproj). |
| `--bump` | No | Tipo de incremento semver al usar --tag: 'patch' (default), 'minor' o 'major'. |
| `--tag-message` | No | Mensaje personalizado para el tag anotado (opcional). Si se omite, se genera automáticamente. |
| `--version` | No | Versión explícita para el tag (ej. '2.1.0' o 'v2.1.0'). Si se indica, omite el cálculo de --bump y usa esta versión directamente. |

## Contrato Técnico (Reglas)

- El mensaje de commit debe seguir Conventional Commits (prefijos válidos: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert).
- Verificar cambios pendientes antes de realizar el commit.

## Intercepción de Bash

- **Patrón**: `\bgit\s+commit\b`
- **Descripción**: Hacer un commit
- **Ejemplo de reemplazo**: `htx task git.committer --message "<mensaje del commit>"`
<!-- higpertext:generated-by=common.docs-sync -->
