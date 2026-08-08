# Capability: `git.rm`

**Nombre**: git.rm
**Versión**: 1.0.0

## Propósito
Remueve archivos del índice git (git rm --cached) sin eliminarlos del sistema de archivos local. Útil para dejar de trackear archivos que deben ser ignorados.

**Entrypoint**: `capabilities/git/scripts/git_rm.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--files` | Sí | Lista de archivos o patrones separados por comas a remover del índice (e.g. 'AGENTS.md,.higpertext/environment.json'). |

## Contrato Técnico (Reglas)

- Ejecutar git rm --cached sobre cada archivo especificado.
- Reportar cada archivo removido exitosamente.
- Si un archivo no está trackeado, reportarlo como advertencia sin fallar.
- Nunca eliminar archivos del sistema de archivos local (solo del índice).

## Intercepción de Bash

- **Comando Bash equivalente**: `git rm <archivo>`
- **Descripción**: Remover archivos del índice git
- **Ejemplo de reemplazo**: `htx task git.rm --files "<archivo1,archivo2>"`
<!-- higpertext:generated-by=common.docs-sync -->
