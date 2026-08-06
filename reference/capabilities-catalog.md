<!-- higpertext:generated-by=docs-maintenance -->
# Catálogo de Capacidades

Referencia técnica exhaustiva de cada capacidad del higpertext Engine.

**Total**: 53 capacidades | **Áreas**: `common`, `git`, `security`

---

## Índice de áreas

- [`common`](#common)
- [`git`](#git)
- [`security`](#security)

---

## Resumen de Hook Intercepts

Comandos bash interceptados automáticamente y redirigidos a su capacidad higpertext.

| Patrón bash | Capacidad | Comando correcto |
|---|---|---|
| `\bwc\s+-l\b|\bfind\s+.*\.(py|ts|js|cs)\b` | `common.code-skeletonizer` | `htx task common.code-skeletonizer --path src/my_module.py` |
| `\bpip\s+(install|uninstall|freeze)\b` | `common.dep-manager` | `htx task common.dep-manager --action install --packages requests` |
| `\bgrep\b` | `common.grep-search` | `htx task common.grep-search --pattern "<patrón>" --path <ruta>` |
| `\bcat\s+.*\.(md|json|yaml|yml|txt)\b` | `common.knowledge-asker` | `htx task common.knowledge-asker --query "<pregunta>"` |
| `\bgit\s+commit\b` | `git.committer` | `htx task git.committer --message "<mensaje del commit>"` |
| `\bgit\s+(diff|status|log)\b` | `git.diff` | `htx task git.git-diff --detail true` |
| `(^|[;&|]\s*)ls(\s|$)|\bgit\s+ls-files\b` | `git.ls-files` | `htx task git.ls-files --path src --mode summary` |
| `\bgit\s+rm\b` | `git.rm` | `htx task git.git-rm --files "<archivo1,archivo2>"` |

---

## Área: `common` {#common}

### `common.agent-bootstrap`
**Propósito**: Instala higpertext-cli en el venv propio de un agente ya registrado y lo sincroniza, para que sus hooks nativos apunten a ese intérprete local en vez del venv del motor.

- **Entrypoint**: `src/higpertext/capabilities/common/scripts/core/agents/agent_bootstrap.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--name` | Sí | Nombre del agente ya registrado en agents_registry.json (ver common.agent-sync --action list) |
  | `--mode` | No | editable: pip install -e apuntando al checkout del motor (default, rápido). wheel: construye e instala un wheel real (más lento, más fiel a producción). |
  | `--engine-source` | No | Ruta al checkout de higpertext-cli a instalar (default: este motor). |
  | `--assistant` | No | Asistente objetivo: claude|gemini|opencode|antigravity|codex|copilot |
  | `--force` | No | Recrea el venv del agente si ya existe. |
- **Contrato técnico**:
  - Debe fallar con [ERROR] si --name no corresponde a un agente registrado.
  - Debe fallar con [ERROR] si --assistant no es uno soportado por HookRenderer.
  - Debe crear (o reutilizar) un venv dentro de la ruta del agente e instalar higpertext-cli ahí.
  - Debe re-sincronizar hooks tras instalar, dejando settings.json del agente apuntando a su propio venv.
  - Debe emitir [SUCCESS] al completar correctamente.

---

### `common.agent-builder`
**Propósito**: Crea la estructura base de un nuevo agente higpertext independiente: scaffolding de carpetas, perfil inicial, htx.py launcher y ambiente .higpertext/ compilado. El agente generado usa el motor central sin copiar src/core/.

- **Entrypoint**: `capabilities/common/scripts/core/agents/agent_builder.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--profile` | Sí | Nombre del perfil inicial del agente (ej: content_creator). No puede ser un nombre reservado del motor. |
  | `--target` | Sí | Ruta del directorio donde se creará el agente. Si no existe, se crea automáticamente. |
  | `--description` | No | Descripción del agente para incluir en el perfil generado. |
- **Contrato técnico**:
  - Debe crear src/capabilities/, src/config/profiles/, src/config/governance/, src/config/environments/, src/config/templates/, src/workflows/, src/config/hooks/profiles/, src/config/hooks/custom/.
  - Debe crear content/ únicamente si el nombre del perfil sugiere un agente de contenido (ej. content_creator) — el resto de agentes no lo necesita.
  - Debe copiar agent_designer.json al directorio de perfiles del agente.
  - Debe generar htx.py launcher apuntando al motor central — nunca copiar src/core/.
  - Debe rechazar nombres de perfil reservados: base_agent, agent_designer, global, base_developer, base_operator, base_auditor.
  - Debe compilar las capas de hooks en .higpertext/config/hooks_config.json.
  - Debe generar el grafo semántico en .higpertext/state/semantic_graph.md.
  - Debe ser idempotente: no sobreescribir htx.py ni src/config/hooks/custom/ si ya existen.
  - El output debe indicar claramente la ruta del agente creado y el comando de verificación.

---

### `common.agent-sync`
**Propósito**: Registra y sincroniza agentes externos con el motor higpertext: propaga hooks y perfiles actualizados.

- **Entrypoint**: `src/higpertext/capabilities/common/scripts/core/agents/agent_sync.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción: register | sync | list |
  | `--name` | No | Nombre del agente |
  | `--path` | No | Ruta absoluta al agente externo (para register) |
  | `--profile` | No | Nombre del perfil del agente (para register) |
  | `--assistant` | No | Asistente objetivo: claude|gemini|opencode|all (para sync) |
- **Contrato técnico**:
  - Debe rechazar 'register' si falta --name, --path o --profile.
  - Debe fallar con [ERROR] si --path no existe o el agente ya está registrado (register).
  - Debe propagar hooks y perfil actualizados del motor hacia el agente (sync).
  - Debe emitir [SUCCESS] al completar register/sync correctamente.

---

### `common.code-skeletonizer`
**Propósito**: Genera una versión 'esqueleto' de un archivo de código fuente (especialmente Python mediante AST, y otros lenguajes mediante expresiones regulares), extrayendo solo firmas de clases, funciones/métodos, docstrings e imports, omitiendo el código de implementación para ahorrar tokens de contexto.

- **Entrypoint**: `capabilities/common/scripts/core/context/code_skeletonizer.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--path` | Sí | Ruta absoluta o relativa del archivo de código a esqueletizar. |
  | `--output` | No | Ruta de destino opcional para guardar el archivo esqueletizado. Si no se especifica, se imprime en consola. |
- **Contrato técnico**:
  - Debe validar que el archivo de entrada exista.
  - Debe extraer imports, nombres de clases, docstrings y firmas de métodos/funciones.
  - Debe reemplazar la implementación interna de métodos y funciones por el token '...'.
  - Para Python debe utilizar la biblioteca 'ast' para un parsing sintáctico robusto.
  - Debe imprimir en la salida estándar la representación del esqueleto si no se especifica un archivo de salida.
- **Hook de intercepción bash**:
  - Patrón: `\bwc\s+-l\b|\bfind\s+.*\.(py|ts|js|cs)\b`
  - Acción: Explorar estructura de archivos de código
  - Comando correcto: `htx task common.code-skeletonizer --path src/my_module.py`

---

### `common.commit-report`
**Propósito**: Genera un reporte explicativo de un commit o rango de commits: resumen en lenguaje natural, archivos impactados por capa DDD, estadísticas y análisis de impacto. Soporta salida Markdown y HTML.

- **Entrypoint**: `capabilities/common/scripts/core/reports/commit_report.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--commit` | No | Hash del commit a analizar. Acepta HEAD, HEAD~N o hash corto. Default: HEAD. |
  | `--range` | No | Rango de commits en formato git (e.g. HEAD~5..HEAD). Si se especifica, sobreescribe --commit. |
  | `--output` | No | Ruta de salida del reporte. Default: .higpertext/reports/commits/<hash>_report.<ext>. |
  | `--no-diff` | No | Omite las líneas modificadas del reporte. |
- **Contrato técnico**:
  - Debe leer el historial git real del repositorio.
  - Debe mostrar resumen ejecutivo en lenguaje natural.
  - Debe agrupar archivos impactados por capa: domain, application, infrastructure, test, capability, other.
  - Debe mostrar estadísticas: archivos cambiados, líneas añadidas, líneas eliminadas.
  - Debe indicar si el commit incluye nuevos tests o nuevas capacidades.
  - Debe completar con [SUCCESS] o [ERROR] explícito.

---

### `common.context-assembler`
**Propósito**: Ensambla un 'context pack' curado para una tarea concreta. Dado un objetivo y tipo, extrae keywords, selecciona del semantic graph solo los símbolos relevantes bajo un presupuesto de tokens, y genera un artefacto Markdown en .higpertext/state/context_packs/. Provee al agente el contexto mínimo-suficiente en vez de explorar el repo a ciegas.

- **Entrypoint**: `capabilities/common/scripts/core/context/context_assembler.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--goal` | Sí | Objetivo de la tarea en lenguaje natural, ej: 'refactorizar el sistema de hooks de sesión'. |
  | `--type` | No | Tipo de tarea: refactor|feature|bugfix|review. Default: feature. |
  | `--budget` | No | Presupuesto máximo de tokens del pack. Default: 8000. |
- **Contrato técnico**:
  - Debe extraer keywords del objetivo filtrando stopwords (ES/EN).
  - Debe seleccionar símbolos del semantic graph relevantes a los keywords.
  - El pack generado nunca debe exceder el presupuesto de tokens indicado.
  - Debe persistir el artefacto Markdown en .higpertext/state/context_packs/.
  - Debe reportar el número de símbolos seleccionados y los tokens estimados.
  - Si el semantic graph no existe, debe generar un pack vacío sin fallar.

---

### `common.context-budget-report`
**Propósito**: Estima cuánto contexto consume una lectura, búsqueda o skeleton antes de ejecutarla y recomienda read range, skeleton, summary o grep.

- **Entrypoint**: `capabilities/common/scripts/core/context/context_budget_report.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--path` | Sí | Archivo a estimar. |
  | `--operation` | No | read, search o skeleton. Default: read. |
  | `--budget` | No | Presupuesto de tokens. Default: 4000. |
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe estimar tokens por tamaño de archivo.
  - Debe recomendar una estrategia de contexto antes de leer blobs grandes.
  - Debe soportar JSON.

---

### `common.dep-manager`
**Propósito**: Gestiona dependencias del proyecto: instala, desinstala y lista paquetes usando el .venv activo. Valida que no se instale en el Python del sistema.

- **Entrypoint**: `capabilities/common/scripts/core/system/dep_manager.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción: install | uninstall | list |
  | `--packages` | No | Paquete(s) separados por coma (requerido para install/uninstall) |
  | `--dev` | No | Si 'true', instala como dependencia de desarrollo |
- **Contrato técnico**:
  - Siempre usar .venv/bin/pip — nunca el pip del sistema.
  - Nunca instalar como root o con sudo.
  - Después de instalar, actualizar requirements.txt si existe.
- **Hook de intercepción bash**:
  - Patrón: `\bpip\s+(install|uninstall|freeze)\b`
  - Acción: Gestionar dependencias del proyecto
  - Comando correcto: `htx task common.dep-manager --action install --packages requests`

---

### `common.doctor`
**Propósito**: Ejecuta un diagnóstico integral del workspace: launcher, entorno, capabilities, hooks, caché y reportes.

- **Entrypoint**: `capabilities/common/scripts/core/session/doctor.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe validar launcher htx disponible.
  - Debe validar catálogo de capabilities y hooks críticos.
  - Debe revisar estado básico de caché y reportes.

---

### `common.drift-check`
**Propósito**: Compara agent_designer.json de cada agente externo registrado contra la versión actual del motor y reporta drift.

- **Entrypoint**: `capabilities/common/scripts/core/agents/drift_check.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--engine-root` | No | Raíz del motor higpertext (default: raíz del repo actual). |
- **Contrato técnico**:
  - Debe comparar las capabilities de agent_designer.json de cada agente registrado contra la del motor.
  - Debe emitir [SUCCESS] si todos los agentes están sincronizados.
  - Debe emitir [ERROR] y salir con código distinto de cero si detecta drift en al menos un agente.

---

### `common.efficiency-meter`
**Propósito**: Mide la eficiencia de una sesión de agente cruzando telemetry.jsonl con los context packs generados. Calcula: tokens totales, costo estimado, exploration_waste_ratio (lecturas sin higpertext / total reads), context_hit_rate (archivos del pack realmente leídos) y hook_intercepts. Retroalimenta la calidad de los context packs.

- **Entrypoint**: `capabilities/common/scripts/core/reports/efficiency_meter.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--session` | No | ID de sesión a analizar. Si no se indica, usa la sesión activa. |
  | `--format` | No | Formato de salida: table|json|markdown. Default: table. |
- **Contrato técnico**:
  - Si no se indica --session, debe leer la sesión activa de .higpertext/state/session.json.
  - context_hit_rate debe estar en el rango [0, 1].
  - exploration_waste_ratio debe estar en el rango [0, 1].
  - Si no hay eventos de telemetría para la sesión, debe devolver métricas en cero sin fallar.
  - El formato 'json' debe producir JSON válido serializable.

---

### `common.error-context-locator`
**Propósito**: Extrae file:line desde trazas, errores o logs y devuelve contexto mínimo con sugerencias de smart-read focalizado.

- **Entrypoint**: `capabilities/common/scripts/core/context/error_context_locator.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--error` | No | Texto del error o traza. |
  | `--error_file` | No | Archivo con error/log a analizar. |
  | `--max_context` | No | Líneas alrededor de cada ubicación. Default: 5. |
  | `--include_tests` | No | Incluye ubicaciones de tests. Default: true. |
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe detectar patrones Python File path, line N y file:line genéricos.
  - Debe devolver contexto mínimo por ubicación detectada.
  - Debe sugerir common.smart-read con around_line para cada ubicación.
  - Debe soportar error inline, error_file, include_tests y JSON.

---

### `common.eval-agent`
**Propósito**: Ejecuta el framework de evaluación de modelos y configuración del higpertext Engine. Valida que los archivos generados contienen las secciones correctas, que los hooks responden bien y que el modelo se comporta según el perfil.

- **Entrypoint**: `capabilities/common/scripts/core/system/eval_agent.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--profile` | No | Perfil higpertext a evaluar. Usa 'all' para evaluar todos los perfiles. |
  | `--mode` | No | Modo de evaluación: static (sin API) | hooks (sin API) | behavioral (API) | safety (API) | all |
  | `--provider` | No | Provider LLM para modos behavioral/safety: gemini | anthropic |
- **Contrato técnico**:
  - Retorna exit code 0 si todos los tests pasan, 1 si hay fallos.
  - Los modos behavioral y safety requieren RUN_BEHAVIORAL_TESTS=1.
  - El modo static y hooks no requieren API keys y siempre deben pasar.

---

### `common.file-map`
**Propósito**: Inspecciona estructura de archivos trackeados sin leer blobs: directorios, extensiones y archivos grandes candidatos a smart-read/skeleton.

- **Entrypoint**: `capabilities/common/scripts/core/context/file_map.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--path` | No | Ruta/prefijo a mapear. |
  | `--preset` | No | Reservado para filtros: all, code, docs, config. |
  | `--max_depth` | No | Profundidad máxima de agrupación. Default: 2. |
  | `--show_sizes` | No | Reservado para mostrar tamaños detallados. |
  | `--large_threshold_kb` | No | Umbral de archivo grande. Default: 100. |
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe usar git ls-files para evitar dependencias/generados.
  - Debe reportar directorios, extensiones y archivos grandes.
  - Debe sugerir smart-read para archivos grandes.

---

### `common.governance-exception`
**Propósito**: Registra o lista excepciones aprobadas a reglas de gobernanza para un perfil.

- **Entrypoint**: `capabilities/common/scripts/core/governance/governance_exception.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción: register | list |
  | `--rule_id` | No | ID de la regla a exceptuar (requerido para register) |
  | `--reason` | No | Justificación de la excepción |
  | `--approver` | No | Quien aprueba la excepción |
  | `--expires` | No | Fecha de expiración ISO (YYYY-MM-DD) |
  | `--profile` | No | Perfil al que aplica (por defecto el activo) |
- **Contrato técnico**:
  - No puede exceptuar reglas de severidad critical.
  - Toda excepción debe tener reason y approver.
  - Las excepciones expiradas no se aplican aunque existan en el store.

---

### `common.graph-query`
**Propósito**: Consulta el grafo semántico para encontrar símbolos relacionados con un nombre o keyword, expandiendo vecinos hasta una profundidad configurable y permitiendo filtrar, limitar o emitir resultados compactos.

- **Entrypoint**: `capabilities/common/scripts/core/search/graph_query.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--symbol` | Sí | Nombre o keyword del símbolo a buscar (búsqueda parcial, case-insensitive). |
  | `--depth` | No | Profundidad de expansión de vecinos en el grafo. Default: 2. |
  | `--budget` | No | Presupuesto de tokens para el resultado. Default: 8000. |
  | `--type` | No | Filtra por tipo de símbolo: class, function, method, variable o module. |
  | `--limit` | No | Máximo de símbolos a mostrar. Default: 50. |
  | `--files_only` | No | Si es true, muestra solo archivos únicos relacionados. |
  | `--json` | No | Si es true, emite JSON estructurado. |
- **Contrato técnico**:
  - Debe leer .higpertext/state/semantic_graph.json. Si no existe, indicar que hay que ejecutar common.graph-rebuild primero.
  - Debe mostrar cada símbolo con su archivo, línea y tipo.
  - Debe soportar filtro por tipo, límite de resultados, modo files_only y salida JSON.
  - Debe indicar el total de símbolos retornados y tokens estimados.
  - Debe completar con [SUCCESS] o [NOT FOUND] explícito.

---

### `common.graph-rebuild`
**Propósito**: Regenera el grafo semántico del proyecto parseando todos los archivos Python con AST. Persiste el resultado en .higpertext/state/semantic_graph.json y actualiza el resumen en .higpertext/state/semantic_graph.md.

- **Entrypoint**: `capabilities/common/scripts/core/search/graph_rebuild.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--root` | No | Directorio raíz del proyecto a analizar. Por defecto el directorio actual. |
  | `--god_threshold` | No | Umbral de in-degree para clasificar un nodo como god node. Default: 5. |
- **Contrato técnico**:
  - Debe generar .higpertext/state/semantic_graph.json con campos symbols y relations.
  - Debe imprimir el total de símbolos y relaciones indexados al finalizar.
  - Debe indicar cuántos god nodes fueron detectados.
  - Nunca debe exponer contenido de archivos .env o secrets.
  - Debe completar con [SUCCESS] o [ERROR] explícito.

---

### `common.grep-search`
**Propósito**: Busca patrones de texto, código o símbolos del grafo semántico Nexus/Higpertext. Soporta búsqueda recursiva, presets, filtros glob/extensión, exclusiones, priorización por relevancia, límites por archivo/línea/tamaño, contexto, modo regex, conteo por archivo y salida JSON estructurada.

- **Entrypoint**: `capabilities/common/scripts/core/search/grep_search.py`
- **Parámetros**:
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
- **Contrato técnico**:
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
- **Hook de intercepción bash**:
  - Patrón: `\bgrep\b`
  - Acción: Buscar en el código
  - Comando correcto: `htx task common.grep-search --pattern "<patrón>" --path <ruta>`

---

### `common.higpertext-tester`
**Propósito**: Suite de validación y testing para asegurar la integridad de capacidades y el cumplimiento de contratos técnicos.

- **Entrypoint**: `capabilities/common/scripts/core/system/higpertext_tester.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--level` | No | Nivel de prueba a ejecutar: integrity, smoke o full. |
  | `--target` | No | Capacidad específica a testear (opcional, por defecto todas). |
- **Contrato técnico**:
  - Generar un reporte de salud del motor en formato Markdown.
  - Listar fallos de integridad (archivos faltantes) con prioridad crítica.
  - Validar cumplimiento de reglas de contrato (Regex/Heurística).

---

### `common.hook-health`
**Propósito**: Valida que los hooks de reducción de contexto estén registrados, detecten outputs grandes, bloqueen Read masivo y no recomienden flags inexistentes conocidos.

- **Entrypoint**: `capabilities/common/scripts/core/session/hook_health.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe verificar hook_post_observer, hook_context_manager, hook_session_prompt, hook_read_guard y hook_bash_guard.
  - Debe validar detección de output grande en PostToolUse.
  - Debe validar bloqueo de Read grande con sugerencia common.smart-read.
  - Debe fallar si encuentra recomendaciones de --source_path en hooks.

---

### `common.hook-sync-check`
**Propósito**: Compara los hooks desplegados en asistentes contra la fuente canónica del motor.

- **Entrypoint**: `capabilities/common/scripts/core/session/hook_sync_check.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--json` | No | Emite JSON estructurado. |
  | `--strict` | No | Si true, asistentes sin hooks desplegados fallan. |
- **Contrato técnico**:
  - Debe comparar hashes de hooks fuente contra hooks desplegados.
  - Debe reportar drift por asistente.
  - Debe soportar salida JSON.

---

### `common.hooks-manager`
**Propósito**: Gestiona el ciclo de vida de hooks nativos de higpertext: listar, agregar, habilitar, deshabilitar y eliminar entradas en .higpertext/hooks_config.json. Regenera la configuración nativa del asistente activo tras cada cambio.

- **Entrypoint**: `capabilities/common/scripts/core/session/hooks_manager.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción a ejecutar: list | add | remove | enable | disable | render |
  | `--id` | No | ID del hook (requerido para remove, enable, disable). |
  | `--event` | No | Evento del hook para --action add: PreToolUse | PostToolUse | Stop | Notification |
  | `--script` | No | Ruta relativa al script del hook (requerido para --action add). |
  | `--description` | No | Descripción del hook (opcional para --action add). |
  | `--assistant` | No | Asistente para --action render: claude | gemini | antigravity | copilot. Por defecto usa el activo en environment.json. |
  | `--profile` | No | Perfil para --action render. Por defecto usa el activo en environment.json. |
- **Contrato técnico**:
  - Debe mostrar la lista completa de hooks con su estado (enabled/disabled) al usar --action list.
  - Al agregar un hook debe validar que el evento sea uno de los soportados.
  - Al remover un hook debe confirmar que el ID existe antes de eliminar.
  - Tras enable/disable/add/remove debe llamar a HookRenderer para regenerar la config nativa del asistente activo.
  - Nunca debe exponer variables de entorno o secrets en el output.
  - Debe indicar claramente el número de hooks activos por perfil al listar.

---

### `common.investigation-report`
**Propósito**: Persiste un informe de investigación o de justificación técnica de contenido libre en .higpertext/reports/<category>/, usando el índice unificado de OutputStore.

- **Entrypoint**: `capabilities/common/scripts/core/reports/investigation_report.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--title` | Sí | Título del informe. |
  | `--category` | No | Subcarpeta dentro de reports/ (default: investigation). |
  | `--slug` | No | Identificador del reporte; si se omite se deriva del título. |
  | `--content` | No | Contenido Markdown inline del informe. |
  | `--content_file` | No | Ruta a un archivo con el contenido Markdown del informe (alternativa a --content). |
- **Contrato técnico**:
  - Requiere exactamente uno de --content o --content-file.
  - El reporte se persiste vía OutputStore, nunca sobreescribe reportes con slug distinto.
  - No ejecuta código externo ni interpreta el contenido; lo escribe tal cual como Markdown.

---

### `common.knowledge-asker`
**Propósito**: Consulta la base de conocimiento de Gobernanza y Minutas para resolver dudas sobre normas y decisiones previas.

- **Entrypoint**: `capabilities/common/scripts/core/governance/ask_higpertext.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--query` | Sí | La pregunta o tema sobre el cual se desea obtener información de gobernanza. |
- **Contrato técnico**:
  - Citar siempre el nombre del archivo fuente encontrado.
  - Si no se encuentra información, sugerir consultar al administrador de gobernanza.
  - Resumir los puntos clave de forma ejecutiva.
- **Hook de intercepción bash**:
  - Patrón: `\bcat\s+.*\.(md|json|yaml|yml|txt)\b`
  - Acción: Consultar documentación o gobernanza
  - Comando correcto: `htx task common.knowledge-asker --query "<pregunta>"`

---

### `common.list-rules`
**Propósito**: Lista todas las capacidades disponibles para el perfil activo con su namespace, ID corto y descripción de una línea. Útil para descubrir qué capacidades están disponibles antes de invocar load-rules.

- **Entrypoint**: `capabilities/common/scripts/core/governance/list_rules.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--type` | No | Tipo de recursos a listar: 'capabilities', 'workflows' o 'all'. |
- **Contrato técnico**:
  - Debe leer el perfil activo desde .higpertext/environment.json.
  - Debe cruzar los IDs del perfil con los JSONs en src/capabilities/.
  - Debe mostrar una tabla con columnas: NAMESPACE, ID, DESCRIPCIÓN.
  - Debe indicar el total de capacidades al finalizar.
  - Si un JSON de capacidad no existe, debe marcarlo como '(JSON no encontrado)' sin fallar.
  - Debe imprimir el comando de ejemplo para invocar load-rules al finalizar.

---

### `common.llm-invoke`
**Propósito**: Invoca un modelo LLM por API (Anthropic, OpenAI, Gemini, Ollama). Soporta completion y streaming.

- **Entrypoint**: `capabilities/common/scripts/core/system/llm_invoke.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--prompt` | Sí | Prompt principal a enviar al modelo. |
  | `--provider` | No | Provider a usar: anthropic | openai | gemini | ollama. Default: según environment.json o HIGPERTEXT_LLM_PROVIDER. |
  | `--model` | No | ID del modelo. Default: según environment.json del provider. |
  | `--system` | No | System prompt opcional. |
  | `--max_tokens` | No | Tokens máximos de respuesta. Default: 1024. |
  | `--temperature` | No | Temperatura de sampling (0.0-1.0). Default: 0.7. |
  | `--stream` | No | Activar streaming (true/false). Default: false. |
  | `--output_file` | No | Ruta de archivo donde guardar el resultado. Opcional. |
- **Contrato técnico**:
  - El prompt no puede estar vacío.
  - El provider debe ser uno de: anthropic, openai, gemini, ollama.
  - Las API keys deben estar en variables de entorno, nunca en parámetros.
  - El output incluye el contenido generado y métricas de tokens.

---

### `common.load-rules`
**Propósito**: Carga las reglas detalladas de capacidades seleccionadas al contexto del LLM activo. Escribe un archivo de reglas en la ruta correspondiente al asistente (.claude/rules/, .opencode/rules/, etc.) para que sea cargado automáticamente en el siguiente turno.

- **Entrypoint**: `capabilities/common/scripts/core/governance/load_rules.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--rules` | Sí | IDs de capacidades a cargar, separados por coma. Acepta short ID (grep-search) o full ID (common.grep-search). Usa 'all' para cargar todas las del perfil activo. |
  | `--workflows` | No | IDs de workflows a cargar, separados por coma. Acepta short ID (higpertext-build) o full ID (workflow.higpertext-build). Usa 'all' para cargar todos los compilados. |
  | `--clear` | No | Si es 'true', elimina el archivo session-capabilities.md (o el bloque equivalente) sin generar nuevo contenido. |
- **Contrato técnico**:
  - Debe leer el asistente activo desde .higpertext/environment.json para determinar la ruta de destino.
  - Debe leer el perfil activo para validar que los IDs solicitados existen en el perfil.
  - Para claude y opencode: debe escribir .claude/rules/session-capabilities.md o equivalente.
  - Para gemini, copilot y antigravity: debe reemplazar el bloque session-capabilities en el archivo principal si existe, o appenderlo si no.
  - Si un ID no existe en el perfil, debe fallar con mensaje descriptivo antes de escribir.
  - Debe reportar cuántas capacidades se cargaron y en qué archivo.

---

### `common.memory-manager`
**Propósito**: Persiste aprendizajes y estados después de cada acción del agente.

- **Entrypoint**: `capabilities/common/scripts/core/governance/memory_manager.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Nombre de la acción realizada. |
  | `--status` | Sí | Estado final de la acción. |
  | `--notes` | Sí | Aprendizajes o notas técnicas de la acción. |
  | `--action_id` | No | ID único de la acción (si no se proporciona se autogenera). |
  | `--learned` | No | Aprendizajes clave obtenidos (separados por comas). |
  | `--failure_root_cause` | No | Causa raíz de la falla si el estado es 'failure'. |
  | `--tags` | No | Tags asociados a la acción (separados por comas). |
- **Contrato técnico**:
  - Cada entrada debe incluir un 'action_id' único.
  - El campo 'learned' debe ser una lista de aprendizajes clave.
  - Si la acción falló, incluir 'failure_root_cause'.
  - La memoria se guarda en la carpeta .memory/ del proyecto activo.

---

### `common.project-explainer`
**Propósito**: Analiza la estructura del proyecto y genera de forma automática y auto-incremental una skill explicativa (.md) sobre sus módulos, dependencias y archivos clave.

- **Entrypoint**: `capabilities/common/scripts/core/system/project_explainer.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | No | Acción a realizar: 'explain' (analiza y genera skill) o 'build' (crea/completa andamiaje faltante). |
  | `--target_path` | No | Ruta específica del proyecto a analizar. |
- **Contrato técnico**:
  - Debe escanear recursivamente los archivos del proyecto ignorando patrones excluidos.
  - Debe mapear la estructura de carpetas y archivos principales.
  - Debe generar o actualizar de forma incremental una skill ('project-explanation') dentro del espacio de trabajo del asistente activo.
  - Si la acción es 'build', debe generar plantillas base si faltan archivos clave del estándar (como gitignore, README, etc.).

---

### `common.quality-resolver`
**Propósito**: Actualiza el checklist de remediación a partir de un reporte de calidad.

- **Entrypoint**: `capabilities/common/scripts/core/quality_resolver.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--report` | No | Ruta o nombre del reporte de calidad. |
  | `--todo_file` | No | Ruta o nombre del checklist de remediación. |
  | `--mode` | No | 'update' conserva las violaciones resueltas; 'create' recrea el checklist. |
- **Contrato técnico**:
  - Debe fallar con [ERROR] si el reporte de calidad no existe.
  - En modo 'update', debe marcar como resueltas [x] las violaciones ya no presentes.
  - En modo 'create', debe sobreescribir el checklist con las violaciones actuales.
  - Debe emitir [SUCCESS] al actualizar el checklist correctamente.

---

### `common.rag-index`
**Propósito**: Indexa el código fuente y documentación del proyecto para habilitar la búsqueda semántica.

- **Entrypoint**: `capabilities/common/scripts/core/search/rag_index.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--root` | No | Ruta al directorio raíz del proyecto. |
- **Contrato técnico**:
  - Debe generar fragmentos semánticos usando AST.
  - Debe generar embeddings y guardarlos en .higpertext/state/vector_store.json.
  - Debe usar el proveedor local por defecto y respetar HIGPERTEXT_EMBEDDING_PROVIDER=gemini.
  - Debe generar embeddings por lote cuando el proveedor lo soporte.

---

### `common.report-viewer`
**Propósito**: Muestra en terminal los reportes persistidos e indexados por higpertext.

- **Entrypoint**: `capabilities/common/scripts/core/reports/report_viewer.py`
- **Parámetros**: No requiere parámetros.
- **Contrato técnico**:
  - Debe leer el índice de OutputStore como fuente de verdad.
  - Debe listar en terminal únicamente reportes existentes, con su ruta, tamaño y fecha.
  - No debe generar HTML ni escribir archivos.

---

### `common.roadmap-phase-close`
**Propósito**: Único camino gobernado para marcar una fase de un roadmap como done. Ejecuta un reviewer independiente (comando HTX_ROADMAP_REVIEWER, p.ej. claude -p) sobre el diff actual y solo cierra la fase si el veredicto es PASS.

- **Entrypoint**: `capabilities/common/scripts/core/reports/roadmap_phase_close.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--roadmap` | Sí | Path al archivo roadmap JSON. |
  | `--phase-id` | Sí | ID de la fase a cerrar. |
- **Contrato técnico**:
  - No debe marcar una fase como done si HTX_ROADMAP_REVIEWER no está configurado.
  - No debe marcar una fase como done si el veredicto del reviewer no es exactamente PASS.
  - Debe persistir SIEMPRE el intento en reviewer_validations del roadmap JSON, incluso cuando el veredicto es CONCERNS o FAIL.
  - El comando del reviewer se ejecuta dentro de la misma sesión, con el diff actual (git diff HEAD) como contexto.
  - Debe completar con [OK]/[ERROR] explícito y código de salida distinto de 0 cuando no se cierra la fase.

---

### `common.roadmap-report`
**Propósito**: Genera un reporte explicativo del roadmap activo: progreso por fase, skills y subagents usados, y timeline HTML visual. Se puede invocar al completar cada fase via /plan.

- **Entrypoint**: `capabilities/common/scripts/core/reports/roadmap_report.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--roadmap` | No | Path al archivo roadmap JSON. Default: roadmap activo en .higpertext/config/roadmaps/. |
  | `--output` | No | Ruta de salida del reporte. Default: .higpertext/reports/roadmap/<roadmap-id>_report.<ext>. |
  | `--commit-range` | No | Rango Git exacto a documentar, por ejemplo HEAD~5..HEAD. |
  | `--verify` | No | Ejecuta el arnés de evaluación estático y de hooks e incorpora su resultado al reporte. |
  | `--agent-validation-summary` | No | Resumen corto de la validación del agente que cierra la tarea. Se persiste en el roadmap JSON (acumulativo). |
  | `--agent-validation-file` | No | Path a un archivo con el texto de validación, para resúmenes largos. Alternativa a agent-validation-summary. |
  | `--agent-validation-verdict` | No | PASS | CONCERNS | FAIL. Default: PASS. |
  | `--agent-name` | No | Identifica al agente que registra la validación. Default: Claude Sonnet 5. |
- **Contrato técnico**:
  - Debe leer el roadmap indicado o el roadmap activo en .higpertext/config/roadmaps/.
  - Debe mostrar resumen ejecutivo en lenguaje natural con % de completitud.
  - Debe mostrar todas las fases con su status (done/active/pending) e iconos visuales.
  - Debe listar skills y subagents por fase y totales.
  - Debe generar HTML con barra de progreso y timeline coloreado por status.
  - La validación del agente se persiste en el roadmap JSON fuente, no solo en el markdown generado — se acumula entre corridas, nunca se sobreescribe.
  - El cierre efectivo de una fase (status=done) NO ocurre aquí — es responsabilidad exclusiva de common.roadmap-phase-close, gateada por el reviewer.
  - Debe completar con [SUCCESS] o [ERROR] explícito.

---

### `common.search-router`
**Propósito**: Recomienda el plan de capacidades adecuado para localizar contexto: error-context-locator, smart-read, graph-query o grep-search según intención y query.

- **Entrypoint**: `capabilities/common/scripts/core/search/search_router.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--query` | Sí | Consulta, símbolo, path o error. |
  | `--intent` | No | error, feature, refactor, docs, symbol o general. |
  | `--scope` | No | Ruta base para grep-search. Default: . |
  | `--preset` | No | Preset para grep-search. Default: code. |
  | `--budget` | No | Presupuesto de tokens sugerido. Default: 4000. |
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe recomendar error-context-locator para intención error o trazas.
  - Debe recomendar graph-query para feature/refactor/symbol.
  - Debe incluir grep-search como fallback general con límites de contexto.
  - Debe soportar salida JSON.

---

### `common.semantic-diff`
**Propósito**: Detecta qué funciones, clases y métodos cambiaron entre dos commits (o entre HEAD y el working tree), usando AST parsing. Útil para identificar qué tests reejecutar tras un cambio.

- **Entrypoint**: `capabilities/common/scripts/core/search/semantic_diff.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--base` | No | Commit/branch de referencia base (default: HEAD~1). |
  | `--head` | No | Commit/branch destino a comparar (default: HEAD). |
  | `--files` | No | Lista de archivos específicos separados por comas (opcional). |
  | `--format` | No | Formato de salida: 'text' o 'json' (default: text). |
- **Contrato técnico**:
  - Listar funciones y clases modificadas, añadidas o eliminadas entre los dos commits.
  - Agrupar los cambios por archivo.
  - Indicar el tipo de cambio: added, removed, modified.
  - Si format=json, devolver un objeto JSON estructurado con la lista de símbolos cambiados.

---

### `common.semantic-search`
**Propósito**: Busca fragmentos de código y documentación semánticamente usando RAG.

- **Entrypoint**: `capabilities/common/scripts/core/search/semantic_search.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--query` | Sí | Consulta o concepto semántico a buscar. |
  | `--limit` | No | Número máximo de fragmentos a retornar. |
  | `--root` | No | Ruta al directorio raíz del proyecto. |
- **Contrato técnico**:
  - Debe calcular el embedding del query.
  - Debe devolver una lista ordenada de fragmentos por similitud coseno en JSON.
  - Debe usar el mismo proveedor y modelo configurados durante la indexación.

---

### `common.session-clean`
**Propósito**: Cierra la sesión de desarrollo y desmonta/borra todos los recursos efímeros del workspace.

- **Entrypoint**: `capabilities/common/scripts/core/session/session_control.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción a realizar: 'clean' |
- **Contrato técnico**:
  - Debe desmontar skills, subagentes y playbooks efímeros de la sesión activa.
  - No debe fallar si no hay ninguna sesión activa (no-op silencioso).
  - Debe emitir [SUCCESS] al limpiar los recursos correctamente.

---

### `common.session-start`
**Propósito**: Bootstraps a temporal development session by mounting required skills, subagents, and compiling dynamic playbooks.

- **Entrypoint**: `capabilities/common/scripts/core/session/session_control.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción a realizar: 'start' |
  | `--profile` | No | Nombre del perfil activo. |
  | `--assistant` | No | Asistente destino: codex, claude, gemini, antigravity, copilot, opencode. Por defecto usa el del environment.json. |
  | `--skills` | No | Lista de skills separadas por comas. |
  | `--subagents` | No | Lista de subagentes separados por comas. |
- **Contrato técnico**:
  - Debe montar los skills y subagentes declarados en el perfil activo.
  - Debe compilar los playbooks dinámicos (spec/plan/build/review) para el asistente destino.
  - Debe emitir [SUCCESS] al iniciar la sesión correctamente.

---

### `common.smart-read`
**Propósito**: Lee archivos de forma segura para LLM. En modo auto evita volcar archivos grandes y devuelve skeleton, rangos, símbolos o resumen con mapa de líneas.

- **Entrypoint**: `capabilities/common/scripts/core/context/smart_read.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--path` | Sí | Archivo a leer. |
  | `--mode` | No | auto, skeleton, range, symbol, full o summary. Default: auto. |
  | `--symbol` | No | Símbolo a localizar con mode=symbol. |
  | `--offset` | No | Línea inicial para mode=range. Default: 1. |
  | `--limit` | No | Cantidad de líneas para mode=range o radio para around_line. Default: 120. |
  | `--around_line` | No | Lee alrededor de una línea específica. |
  | `--max_bytes` | No | Umbral para considerar archivo grande. Default: 102400. |
  | `--max_tokens` | No | Si el resultado excede el presupuesto, degrada a summary. |
  | `--json` | No | Emite JSON estructurado. |
- **Contrato técnico**:
  - Debe validar que el archivo exista.
  - En mode=auto debe devolver skeleton para archivos de código grandes.
  - Debe soportar lectura por rango, por símbolo, resumen y JSON.
  - Debe bloquear mode=full para archivos que superen max_bytes.
  - Debe incluir números de línea en rangos para facilitar lecturas focalizadas.

---

### `common.subagent-executor`
**Propósito**: Lanza la ejecución aislada de un subagente especializado para resolver una subtarea.

- **Entrypoint**: `capabilities/common/scripts/core/agents/subagent_executor.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--agent` | Sí | Nombre del subagente a ejecutar (ej: test-engineer). |
  | `--task` | Sí | Descripción detallada de la subtarea a resolver. |
  | `--timeout` | No | Tiempo máximo de ejecución en segundos. |
- **Contrato técnico**:
  - Debe validar que --agent y --task no estén vacíos.
  - Debe resolver el subagente desde el entorno activo.
  - Debe ejecutar el provider LLM configurado y devolver un resultado real.
  - Debe emitir [SUCCESS] solo cuando exista una respuesta no vacía.
  - Debe emitir [ERROR] ante agente inexistente, provider no configurado, timeout o respuesta vacía.
  - Nunca debe exponer variables de entorno, credenciales ni secretos.

---

### `common.sync-agents`
**Propósito**: Sincroniza y proyecta las reglas canónicas de AgentSystem (workflows primarios y subagentes) hacia las carpetas específicas del asistente IA en el proyecto destino.

- **Entrypoint**: `capabilities/common/scripts/core/agents/sync_agents.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--target` | No | Ruta al proyecto destino donde proyectar las reglas y subagentes (opcional, por defecto el proyecto actual). |
  | `--source` | No | Ruta origen de AgentSystem (opcional, por defecto se detecta automáticamente o vía variable de entorno AGENT_SYSTEM_PATH). |
  | `--assistant` | No | Asistente objetivo para proyectar (gemini, claude, copilot, opencode, antigravity, all). |
- **Contrato técnico**:
  - Proyectar flujos de trabajo primarios (spec, plan, build, review) en las rutas del asistente.
  - Proyectar subagentes especializados en subagents/.
  - Generar un reporte claro en consola con la lista de archivos creados o actualizados.

---

### `common.task-decomposer`
**Propósito**: Descompone un objetivo de ingeniería en un task-graph determinístico (DAG). Produce fases con dependencias, skills y subagentes por nodo, compatible con roadmap.json de higpertext. Motor heurístico (NO LLM): plantillas por tipo de tarea (refactor/feature/bugfix/review). El resultado es un punto de partida para iterar.

- **Entrypoint**: `capabilities/common/scripts/core/system/task_decomposer.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--goal` | Sí | Objetivo en lenguaje natural, ej: 'implementar el Efficiency Meter'. |
  | `--type` | No | Tipo de tarea: refactor|feature|bugfix|review. Default: feature. |
  | `--save` | No | Si 'true', escribe .higpertext/config/roadmaps/<roadmap-id>.json. Default: true. |
- **Contrato técnico**:
  - El grafo generado debe ser un DAG válido (sin ciclos).
  - Para tipo 'refactor', el primer nodo debe ser de exploración.
  - Cada nodo debe declarar sus skills y subagents.
  - El output debe ser compatible con el formato de roadmap.json de higpertext.
  - Si --save=true, debe escribir en .higpertext/config/roadmaps/<roadmap-id>.json.
  - El decomposer NO debe invocar ningún LLM — solo heurísticas determinísticas.

---

### `common.telemetry-report`
**Propósito**: Muestra dashboard de telemetría higpertext en terminal: tokens estimados, costo, sesiones, commits y correlaciones.

- **Entrypoint**: `capabilities/common/scripts/core/reports/telemetry_report.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--days` | No | Período en días a analizar (default: 7). |
- **Contrato técnico**:
  - Lee únicamente .higpertext/telemetry.jsonl — nunca expone tokens de API ni variables de entorno.
  - Si no hay datos, muestra mensaje informativo sin error.
  - Tokens y costo son estimados (chars/4) — no son valores exactos del proveedor.

---

### `common.training-recommender`
**Propósito**: Analiza la telemetría higpertext y sugiere acciones de entrenamiento del agente: adopción baja, capacidades subutilizadas y herramientas de alto costo.

- **Entrypoint**: `capabilities/common/scripts/core/reports/training_recommender.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--days` | No | Período en días a analizar (default: 7). |
- **Contrato técnico**:
  - Lee únicamente .higpertext/state/telemetry.jsonl — nunca expone tokens de API ni variables de entorno.
  - Si no hay datos suficientes, muestra mensaje informativo sin error.
  - Las recomendaciones son heurísticas basadas en umbrales fijos, no juicios definitivos.

---

### `common.truth-keeper`
**Propósito**: Lee, escribe y gestiona el contexto de la Fuente de Verdad en .memory/truth.json para consulta rápida por agentes.

- **Entrypoint**: `src/higpertext/capabilities/common/scripts/core/governance/truth_keeper.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción a realizar: set, get, delete, list. |
  | `--domain` | No | El dominio/namespace bajo el cual guardar o consultar la información. |
  | `--key` | No | La llave específica del dato a consultar, definir o eliminar. |
  | `--value` | No | El valor a guardar. |
  | `--description` | No | Descripción para el dominio (usado al crear/actualizar un dominio). |
- **Contrato técnico**:
  - Debe crear automáticamente el directorio .memory/ y el archivo truth.json si no existen.
  - La acción 'set' requiere '--domain' y '--key'.
  - La acción 'get' con '--domain' retorna el contexto de ese dominio. Sin '--domain' retorna la Fuente de Verdad completa.
  - Debe retornar salida formateada en JSON estructurado cuando se use 'get' o 'list'.
  - Debe emitir [SUCCESS] al completar una operación de mutación correctamente.
  - Debe emitir [ERROR] ante fallos de formato o argumentos ausentes.

---

## Área: `git` {#git}

### `git.committer`
**Propósito**: Realiza un commit en Git siguiendo el estándar Conventional Commits y opcionalmente hace push a la rama en Azure DevOps.

- **Entrypoint**: `capabilities/git/scripts/commit_changes.py`
- **Parámetros**:
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
- **Contrato técnico**:
  - El mensaje de commit debe seguir Conventional Commits (prefijos válidos: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert).
  - Verificar cambios pendientes antes de realizar el commit.
- **Hook de intercepción bash**:
  - Patrón: `\bgit\s+commit\b`
  - Acción: Hacer un commit
  - Comando correcto: `htx task git.committer --message "<mensaje del commit>"`

---

### `git.diff`
**Propósito**: Detecta cambios locales en el repositorio Git (archivos modificados, eliminados, sin seguimiento) y muestra un resumen estructurado o las diferencias detalladas (diffs) para ayudar al agente en tareas de versionado y documentación.

- **Entrypoint**: `capabilities/git/scripts/git_diff.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--detail` | No | Si es 'true', muestra el diff detallado del código. Si es 'false', solo lista los archivos. (default: 'false') |
  | `--files` | No | Lista de archivos específicos separados por comas para analizar su diff. (opcional) |
- **Contrato técnico**:
  - Mostrar el listado de archivos clasificados por su estado en Git (Staged, Unstaged, Untracked).
  - Si el parámetro detail está activo, formatear el diff en un bloque markdown legible con sintaxis diff.
- **Hook de intercepción bash**:
  - Patrón: `\bgit\s+(diff|status|log)\b`
  - Acción: Ver diff/estado del repositorio
  - Comando correcto: `htx task git.git-diff --detail true`

---

### `git.ls-files`
**Propósito**: Lista e inspecciona archivos trackeados por git como alternativa segura a ls/find. Incluye filtros por ruta, glob, extensión, presets, árbol compacto, resumen, directorios, tamaños, JSON y límites de contexto.

- **Entrypoint**: `capabilities/git/scripts/git_ls_files.py`
- **Parámetros**:
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
- **Contrato técnico**:
  - Listar archivos trackeados en el índice git, uno por línea o mediante resúmenes compactos, como reemplazo gobernado de ls/find.
  - Debe filtrar por path, pattern, include, exclude, extension y preset.
  - Debe limitar resultados mediante max_results para evitar output excesivo.
  - Debe soportar modos list, tree, summary, dirs y json.
  - Debe poder mostrar tamaños, ordenar por path/size/extension y agrupar por directorio o extensión.
  - Debe marcar archivos grandes y sugerir common.code-skeletonizer --path cuando aplique.
  - Debe emitir JSON parseable con total y files cuando json=true o mode=json.
  - Debe indicar total de archivos encontrados al final en salidas de texto.
- **Hook de intercepción bash**:
  - Patrón: `(^|[;&|]\s*)ls(\s|$)|\bgit\s+ls-files\b`
  - Acción: Listar archivos trackeados en el índice git
  - Comando correcto: `htx task git.ls-files --path src --mode summary`

---

### `git.rm`
**Propósito**: Remueve archivos del índice git (git rm --cached) sin eliminarlos del sistema de archivos local. Útil para dejar de trackear archivos que deben ser ignorados.

- **Entrypoint**: `capabilities/git/scripts/git_rm.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--files` | Sí | Lista de archivos o patrones separados por comas a remover del índice (e.g. 'AGENTS.md,.higpertext/environment.json'). |
- **Contrato técnico**:
  - Ejecutar git rm --cached sobre cada archivo especificado.
  - Reportar cada archivo removido exitosamente.
  - Si un archivo no está trackeado, reportarlo como advertencia sin fallar.
  - Nunca eliminar archivos del sistema de archivos local (solo del índice).
- **Hook de intercepción bash**:
  - Patrón: `\bgit\s+rm\b`
  - Acción: Remover archivos del índice git
  - Comando correcto: `htx task git.git-rm --files "<archivo1,archivo2>"`

---

## Área: `security` {#security}

### `security.k8s-auditor`
**Propósito**: Audita archivos YAML de Kubernetes buscando fallos de seguridad o malas prácticas de configuración.

- **Entrypoint**: `capabilities/security/scripts/k8s_auditor.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--target` | No | Directorio o archivo YAML a auditar. |
  | `--save_report` | No | Nombre del archivo de reporte Markdown resultante. |
- **Contrato técnico**:
  - Debe buscar archivos .yaml o .yml recursivamente.
  - Debe evaluar el contexto de seguridad (privileged, runAsNonRoot, readOnlyRootFilesystem, allowPrivilegeEscalation).
  - Debe reportar riesgos de red (hostNetwork, hostPort) e inyecciones de volumen.
  - Debe clasificar los hallazgos por severidad: CRITICAL, HIGH, MEDIUM, LOW.

---

### `security.secret-scanner`
**Propósito**: Escanea el código fuente e historial en busca de claves API expuestas, PATs, tokens y contraseñas.

- **Entrypoint**: `capabilities/security/scripts/secret_scanner.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--target_path` | No | Directorio o archivo a escanear en busca de secretos. |
  | `--output_report` | No | Nombre del archivo de reporte Markdown resultante. |
- **Contrato técnico**:
  - Debe buscar claves API, contraseñas, PATs de Azure DevOps, tokens de AWS y claves privadas.
  - Debe enmascarar los secretos encontrados en el reporte de salida para evitar filtraciones secundarias.
  - Debe clasificar los hallazgos según la criticidad del secreto.

---

### `security.secret-set`
**Propósito**: Guarda, elimina o consulta el estado de la API key de un provider LLM en el llavero nativo del sistema operativo (Keychain / Secret Service / Credential Manager).

- **Entrypoint**: `capabilities/security/scripts/secret_manager.py`
- **Parámetros**:
  | Parámetro | Requerido | Descripción |
  |---|---|---|
  | `--action` | Sí | Acción a ejecutar: set | delete | status. |
  | `--provider` | Sí | Provider LLM objetivo: anthropic | openai | gemini | ollama. |
- **Contrato técnico**:
  - La API key nunca se acepta como parámetro en texto plano.
  - Para 'set', el valor se toma de la variable de entorno HTX_SECRET_VALUE o, si hay TTY interactiva, se solicita vía input oculto (getpass).
  - El almacenamiento usa el keyring nativo del SO, nunca escribe la clave en environment.json.
  - Si el keyring no está disponible en el host, la acción 'set'/'delete' falla explícitamente en vez de degradar en silencio.

---

[Volver al Índice](../README.md)