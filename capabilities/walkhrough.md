# Mapa de capacidades: ¿qué quieres hacer?

Todas las tareas usan `htx task <id>`. Este mapa agrupa las **53 capacidades del motor** por intención; abre la ficha enlazada o el [catálogo técnico](../reference/capabilities-catalog.md) para ver parámetros y contrato.

## Entender y localizar información

| Necesidad | Capacidad |
|---|---|
| Elegir cómo buscar | [Buscar la ruta correcta](common/search/common.search-router.md) `common.search-router` |
| Buscar texto o patrones | [Buscar con grep](common/search/common.grep-search.md) `common.grep-search` |
| Buscar por significado | [Búsqueda semántica](common/search/common.semantic-search.md) `common.semantic-search` |
| Consultar símbolos y relaciones | [Consultar grafo](common/search/common.graph-query.md) `common.graph-query` |
| Reconstruir el grafo | [Reconstruir grafo](common/search/common.graph-rebuild.md) `common.graph-rebuild` |
| Indexar contenido para RAG | [Indexar RAG](common/search/common.rag-index.md) `common.rag-index` |
| Comparar cambios estructurales | [Diff semántico](common/search/common.semantic-diff.md) `common.semantic-diff` |
| Explicar un proyecto | [Project explainer](common/system/common.project-explainer.md) `common.project-explainer` |
| Ver estructura de archivos | [File map](common/context/common.file-map.md) `common.file-map` |
| Leer un archivo sin saturar contexto | [Smart read](common/context/common.smart-read.md) `common.smart-read` |
| Ver solo firmas de código | [Code skeletonizer](common/context/common.code-skeletonizer.md) `common.code-skeletonizer` |
| Localizar contexto desde un error | [Error context locator](common/context/common.error-context-locator.md) `common.error-context-locator` |

## Preparar y optimizar contexto

| Necesidad | Capacidad |
|---|---|
| Crear contexto mínimo para una tarea | [Context assembler](common/context/common.context-assembler.md) `common.context-assembler` |
| Estimar consumo antes de leer | [Context budget report](common/context/common.context-budget-report.md) `common.context-budget-report` |
| Descomponer un objetivo en fases | [Task decomposer](common/system/common.task-decomposer.md) `common.task-decomposer` |

## Trabajar con agentes externos

| Necesidad | Capacidad |
|---|---|
| Crear la estructura de un agente | [Agent builder](common/agents/common.agent-builder.md) `common.agent-builder` |
| Registrar o sincronizar un agente | [Agent sync](common/agents/common.agent-sync.md) `common.agent-sync` |
| Instalar el motor dentro del agente | [Agent bootstrap](common/agents/common.agent-bootstrap.md) `common.agent-bootstrap` |
| Detectar desviación de configuración | [Drift check](common/agents/common.drift-check.md) `common.drift-check` |
| Proyectar reglas a asistentes | [Sync agents](common/agents/common.sync-agents.md) `common.sync-agents` |
| Ejecutar una subtarea especializada | [Subagent executor](common/agents/common.subagent-executor.md) `common.subagent-executor` |

## Gestionar sesión, integración y salud

| Necesidad | Capacidad |
|---|---|
| Abrir una sesión temporal | [Session start](common/session/common.session-start.md) `common.session-start` |
| Limpiar la sesión | [Session clean](common/session/common.session-clean.md) `common.session-clean` |
| Gestionar hooks | [Hooks manager](common/session/common.hooks-manager.md) `common.hooks-manager` |
| Comprobar sincronía de hooks | [Hook sync check](common/session/common.hook-sync-check.md) `common.hook-sync-check` |
| Diagnosticar hooks | [Hook health](common/session/common.hook-health.md) `common.hook-health` |
| Diagnosticar el workspace | [Doctor](common/session/common.doctor.md) `common.doctor` |
| Validar el motor | [Higpertext tester](common/system/common.higpertext-tester.md) `common.higpertext-tester` |
| Evaluar perfiles o modelos | [Eval agent](common/system/common.eval-agent.md) `common.eval-agent` |
| Invocar un proveedor LLM | [LLM invoke](common/system/common.llm-invoke.md) `common.llm-invoke` |
| Gestionar dependencias del proyecto | [Dependency manager](common/system/common.dep-manager.md) `common.dep-manager` |

## Gobernanza, memoria y reglas

| Necesidad | Capacidad |
|---|---|
| Consultar conocimiento | [Knowledge asker](common/governance/common.knowledge-asker.md) `common.knowledge-asker` |
| Persistir aprendizajes de una acción | [Memory manager](common/governance/common.memory-manager.md) `common.memory-manager` |
| Mantener una fuente de verdad | [Truth keeper](common/governance/common.truth-keeper.md) `common.truth-keeper` |
| Listar recursos disponibles | [List rules](common/governance/common.list-rules.md) `common.list-rules` |
| Cargar reglas al asistente | [Load rules](common/governance/common.load-rules.md) `common.load-rules` |
| Solicitar una excepción | [Governance exception](common/governance/common.governance-exception.md) `common.governance-exception` |

## Reportar y planificar

| Necesidad | Capacidad |
|---|---|
| Ver reportes disponibles | [Report viewer](common/reports/common.report-viewer.md) `common.report-viewer` |
| Guardar una investigación | [Investigation report](common/reports/common.investigation-report.md) `common.investigation-report` |
| Explicar un commit | [Commit report](common/reports/common.commit-report.md) `common.commit-report` |
| Ver telemetría | [Telemetry report](common/reports/common.telemetry-report.md) `common.telemetry-report` |
| Medir eficiencia de una sesión | [Efficiency meter](common/reports/common.efficiency-meter.md) `common.efficiency-meter` |
| Obtener recomendaciones de aprendizaje | [Training recommender](common/reports/common.training-recommender.md) `common.training-recommender` |
| Consultar un roadmap | [Roadmap report](common/reports/common.roadmap-report.md) `common.roadmap-report` |
| Cerrar una fase con revisión independiente | [Roadmap phase close](common/reports/common.roadmap-phase-close.md) `common.roadmap-phase-close` |
| Actualizar tareas desde un reporte de calidad | [Quality resolver](common/reports/common.quality-resolver.md) `common.quality-resolver` |

## Git y seguridad

| Necesidad | Capacidad |
|---|---|
| Revisar cambios | [Git diff](git/git.diff.md) `git.diff` |
| Listar archivos versionados | [Git ls-files](git/git.ls-files.md) `git.ls-files` |
| Crear un commit | [Git committer](git/git.committer.md) `git.committer` |
| Eliminar archivos mediante Git | [Git rm](git/git.rm.md) `git.rm` |
| Buscar secretos expuestos | [Secret scanner](security/security.secret-scanner.md) `security.secret-scanner` |
| Guardar o consultar una clave de proveedor | [Secret set](security/security.secret-set.md) `security.secret-set` |
| Auditar manifiestos Kubernetes | [K8s auditor](security/security.k8s-auditor.md) `security.k8s-auditor` |
