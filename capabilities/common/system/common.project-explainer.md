# Capability: `common.project-explainer`

**Nombre**: Project Explainer
**Versión**: 1.0.0

## Propósito
Analiza la estructura del proyecto y genera de forma automática y auto-incremental una skill explicativa (.md) sobre sus módulos, dependencias y archivos clave.

**Entrypoint**: `capabilities/common/scripts/core/system/project_explainer.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | No | Acción a realizar: 'explain' (analiza y genera skill) o 'build' (crea/completa andamiaje faltante). |
| `--target_path` | No | Ruta específica del proyecto a analizar. |

## Contrato Técnico (Reglas)

- Debe escanear recursivamente los archivos del proyecto ignorando patrones excluidos.
- Debe mapear la estructura de carpetas y archivos principales.
- Debe generar o actualizar de forma incremental una skill ('project-explanation') dentro del espacio de trabajo del asistente activo.
- Si la acción es 'build', debe generar plantillas base si faltan archivos clave del estándar (como gitignore, README, etc.).
<!-- higpertext:generated-by=common.docs-sync -->
