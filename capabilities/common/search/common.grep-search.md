# Capability: `common.grep-search`

**Nombre**: Grep Search
**Versión**: 1.0.0

## Propósito
Busca patrones de texto, código o símbolos del grafo semántico Nexus/Higpertext. Soporta búsqueda recursiva, presets, filtros glob/extensión, exclusiones, priorización por relevancia, límites por archivo/línea/tamaño, contexto, modo regex, conteo por archivo y salida JSON estructurada.

**Entrypoint**: `capabilities/common/scripts/core/search/grep_search.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--pattern` | No | Patrón de búsqueda (texto literal o regex si --regex=true). Requerido salvo que se use --query. |
| `--query` | No | Alias ergonómico de --pattern. |
| `--path` | No | Directorio o archivo donde buscar. Por defecto el directorio actual. |
| `--include` | No | Filtro de extensión de archivo, ej: '*.py' o '*.ts'. Acepta múltiples separados por coma. |
| `--extension` | No | Alias ergonómico de include. Acepta 'py,ts' o '*.py,*.ts'. |
| `--exclude` | No | Patrones de directorios o archivos a excluir, ej: '__pycache__,node_modules,.venv'. Por defecto excluye .venv, __pycache__, .git, node_modules. |
| `--regex` | No | Si es 'true', trata el patrón como expresión regular. Default: false (literal). |
| `--case_sensitive` | No | Si es 'true', la búsqueda distingue mayúsculas. Default: false. |
| `--context` | No | Número de líneas de contexto antes y después de cada coincidencia. Default: 0. |
| `--before` | No | Líneas antes de cada coincidencia. Se combina con context usando el valor mayor. |
| `--after` | No | Líneas después de cada coincidencia. Se combina con context usando el valor mayor. |
| `--max_results` | No | Límite máximo de resultados a mostrar. Default: 100. |
| `--max_per_file` | No | Límite máximo de coincidencias mostradas por archivo. Default: 20. |
| `--line_limit` | No | Caracteres máximos por línea impresa; 0 desactiva truncado. Default: 240. |
| `--max_file_size_kb` | No | Ignora archivos mayores al tamaño indicado; 0 desactiva. Default: 1024. |
| `--sort` | No | Orden de resultados: relevance prioriza archivos con más hits; path ordena por ruta. Default: relevance. |
| `--preset` | No | Preset de búsqueda si no se pasa include/extension: all, code, python, web, docs o config. Default: all. |
| `--include_tests` | No | Incluye rutas de tests/specs. Default: true. |
| `--source_first` | No | Prioriza archivos fuente antes que tests al ordenar. Default: true. |
| `--files_only` | No | Si es 'true', solo muestra los nombres de archivos con coincidencias, sin el contenido. |
| `--count` | No | Si es 'true', muestra el conteo de coincidencias por archivo sin contenido de líneas. |
| `--semantic` | No | Si es 'true', busca también en el grafo semántico Nexus/Higpertext (.higpertext/state/semantic_graph.json o .nexus/semantic_graph.*). |
| `--json` | No | Si es 'true', emite una respuesta JSON estructurada para consumo por otras herramientas nativas. |
| `--absolute_paths` | No | Si es 'true', muestra rutas absolutas; por defecto usa rutas relativas para ahorrar contexto. |
| `--all` | No | Si es 'true', busca también en directorios de agentes/configuración normalmente excluidos; mantiene exclusiones seguras para .git, .venv, node_modules, binarios y secretos. |

## Contrato Técnico (Reglas)

- Debe usar grep (Linux/macOS) o el equivalente Python puro como fallback multiplataforma.
- Debe mostrar resultados agrupados por archivo con número de línea y contenido de la línea.
- Debe indicar el total de coincidencias encontradas al finalizar.
- Si no hay coincidencias, debe indicarlo claramente con [NOT FOUND].
- Si el patrón no es válido como regex (cuando regex=true), debe reportar el error.
- Debe respetar el límite de max_results para evitar output excesivo.
- Debe rechazar context negativo y max_results menor o igual a cero.
- Debe aceptar --extension como alias de --include normalizando extensiones simples a globs.
- Debe aceptar --query como alias de --pattern.
- Debe aceptar presets de búsqueda cuando no se indique include/extension.
- Debe soportar límites por archivo, longitud de línea y tamaño máximo de archivo para proteger el contexto.
- Debe ordenar por relevancia por defecto y permitir orden por ruta.
- Debe poder omitir tests/specs con include_tests=false y priorizar fuente con source_first=true.
- Con --semantic=true debe consultar el grafo semántico Nexus/Higpertext sin volcar el blob completo.
- Con --json=true debe devolver salida parseable con total_matches, files_scanned, files_matched y matches.
- Nunca debe exponer contenido de archivos de secretos (.env, secrets.json, *.key).
- Con --all=true debe incluir directorios de agentes/configuración como .opencode, .claude, .agents y .gemini, pero nunca .git, .venv ni node_modules.

## Intercepción de Bash

- **Comando Bash equivalente**: `grep -R "<patrón>" <ruta>`
- **Descripción**: Buscar en el código
- **Ejemplo de reemplazo**: `htx task common.grep-search --pattern "<patrón>" --path <ruta>`
<!-- higpertext:generated-by=common.docs-sync -->
