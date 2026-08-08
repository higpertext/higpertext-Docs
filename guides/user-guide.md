# Guía de uso

## Trabajo diario: motor + proyecto

El motor se instala una vez. En cada proyecto donde quieras usar un asistente, inicializa la integración, carga un perfil y trabaja sobre tu código:

```bash
cd mi-proyecto
htx init --assistant codex
htx profile load base_developer --assistant codex
```

Desde ese punto ya puedes conversar y trabajar con el asistente. Las tareas de higpertext sirven para operaciones estructuradas; no sustituyen la lógica de tu aplicación.

```bash
# Ver qué recursos están disponibles
htx task common.list-rules --type all

# Entender el proyecto
htx task common.project-explainer --action explain --target_path .

# Buscar una referencia
htx task common.grep-search --pattern "NombreDelSimbolo" --path src

# Preparar contexto para una modificación
htx task common.context-assembler --goal "corregir el fallo de autenticación" --type bugfix
```

## Elegir una capacidad

No memorices los 53 IDs. Empieza por [el mapa por objetivo](../capabilities/walkhrough.md). Si no sabes cómo localizar información, usa:

```bash
htx task common.search-router --intent "entender un componente" --query "nombre o síntoma"
```

El [catálogo de capacidades](../reference/capabilities-catalog.md) es la fuente de verdad para parámetros, contratos y efectos de cada tarea.

## Sesiones y workflows

Una sesión monta recursos temporales del perfil. Solo es necesaria para skills, subagentes y comandos de sesión:

```bash
htx task common.session-start --action start
htx task common.session-clean --action clean
```

Los workflows encadenan tareas existentes. Consulta los que el motor incluye antes de usarlos: [catálogo de workflows](../reference/workflows-catalog.md).

## Crear un agente externo

Cuando necesitas capacidades de un dominio propio, crea un agente externo. El motor seguirá siendo el runtime y el agente contendrá la lógica, los contratos y la configuración específicos.

```bash
htx profile load agent_designer --assistant codex
htx task common.agent-builder \
  --profile mi_perfil \
  --target ../mi-agente \
  --description "Agente especializado en mi dominio"
```

Sigue la [guía de agentes externos](../getting-started/external-agents.md) para registrar, sincronizar y hacer portátil el agente.

## Seguridad

No pases claves en argumentos ni las escribas en archivos de configuración. Para proveedores LLM usa el llavero del sistema:

```bash
htx task security.secret-set --action status --provider openai
```

→ [FAQ](faq.md) · [Hooks](hooks-guide.md) · [Memoria](agent-memory.md)
