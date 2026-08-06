# Capability: `common.file-map`

**Nombre**: File Map
**Versión**: 1.0.0

## Propósito
Inspecciona estructura de archivos trackeados sin leer blobs: directorios, extensiones y archivos grandes candidatos a smart-read/skeleton.

**Entrypoint**: `capabilities/common/scripts/core/context/file_map.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--path` | No | Ruta/prefijo a mapear. |
| `--preset` | No | Reservado para filtros: all, code, docs, config. |
| `--max_depth` | No | Profundidad máxima de agrupación. Default: 2. |
| `--show_sizes` | No | Reservado para mostrar tamaños detallados. |
| `--large_threshold_kb` | No | Umbral de archivo grande. Default: 100. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe usar git ls-files para evitar dependencias/generados.
- Debe reportar directorios, extensiones y archivos grandes.
- Debe sugerir smart-read para archivos grandes.
<!-- higpertext:generated-by=common.docs-sync -->
