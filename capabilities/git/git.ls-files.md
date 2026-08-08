# Capability: `git.ls-files`

**Nombre**: git.ls-files
**Versión**: 1.1.0

## Propósito
Lista e inspecciona archivos trackeados por git como alternativa segura a ls/find. Incluye filtros por ruta, glob, extensión, presets, árbol compacto, resumen, directorios, tamaños, JSON y límites de contexto.

**Entrypoint**: `capabilities/git/scripts/git_ls_files.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--path` | No | Prefijo o ruta a explorar, ejemplo: src/higpertext. |
| `--pattern` | No | Filtro substring opcional para rutas. |
| `--include` | No | Globs incluidos separados por coma, ejemplo: *.py,*.json. |
| `--exclude` | No | Globs o rutas excluidas separados por coma. |
| `--extension` | No | Alias de include para extensiones, ejemplo: py,json. |
| `--preset` | No | Preset de filtro: all, code, python, web, docs, config o tests. Default: all. |
| `--mode` | No | Modo de salida: list, tree, summary, dirs o json. Default: summary. |
| `--max_results` | No | Máximo de rutas, grupos o nodos impresos. Default: 100. |
| `--max_depth` | No | Profundidad máxima para tree/dirs. Default: 3. |
| `--show_size` | No | Si es true, muestra tamaño por archivo en mode=list. |
| `--large_threshold_kb` | No | Umbral para marcar archivos grandes y sugerir skeletonizer. Default: 100. |
| `--sort` | No | Orden: path, size o extension. Default: path. |
| `--group_by` | No | Agrupación para mode=list: none, dir o extension. Default: none. |
| `--files_only` | No | Si es true, imprime solo rutas, una por línea. |
| `--json` | No | Si es true, emite JSON estructurado. |
| `--include_untracked` | No | Si es true, incluye archivos no trackeados respetando .gitignore. |

## Contrato Técnico (Reglas)

- Listar archivos trackeados en el índice git, uno por línea o mediante resúmenes compactos, como reemplazo gobernado de ls/find.
- Debe filtrar por path, pattern, include, exclude, extension y preset.
- Debe limitar resultados mediante max_results para evitar output excesivo.
- Debe soportar modos list, tree, summary, dirs y json.
- Debe poder mostrar tamaños, ordenar por path/size/extension y agrupar por directorio o extensión.
- Debe marcar archivos grandes y sugerir common.code-skeletonizer --path cuando aplique.
- Debe emitir JSON parseable con total y files cuando json=true o mode=json.
- Debe indicar total de archivos encontrados al final en salidas de texto.

## Intercepción de Bash

- **Comando Bash equivalente**: `ls src` o `git ls-files`
- **Descripción**: Listar archivos trackeados en el índice git
- **Ejemplo de reemplazo**: `htx task git.ls-files --path src --mode summary`
<!-- higpertext:generated-by=common.docs-sync -->
