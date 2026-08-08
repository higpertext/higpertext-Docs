# Sesiones de desarrollo

Una sesión es una extensión temporal del perfil activo: monta skills y subagentes y, según el asistente, habilita los comandos de trabajo disponibles. No es necesaria para inicializar el proyecto ni para comenzar a trabajar.

## Abrir y cerrar

```bash
htx task common.session-start --action start

# Cuando termines
htx task common.session-clean --action clean
```

Puedes indicar recursos explícitos si el perfil o el agente externo los proporciona:

```bash
htx task common.session-start --action start \
  --skills "best-practices" \
  --subagents "architect"
```

Los recursos disponibles dependen del perfil cargado. Los perfiles incluidos por el motor son `global`, `base_developer`, `base_operator`, `base_auditor`, `base_agent` y `agent_designer`; los agentes externos pueden declarar los suyos.

## Workflows de sesión

El motor incluye dos atajos opcionales:

```bash
htx workflow run higpertext-plan
htx workflow run higpertext-review --notes "Resumen y aprendizaje de la sesión."
```

Consulta [el catálogo de workflows](../reference/workflows-catalog.md) antes de ejecutar otros flujos, especialmente los que pertenecen a un agente externo.

## Roadmaps

Los roadmaps organizan fases de trabajo del proyecto. Se crean y activan así:

```bash
htx roadmap new mi-roadmap --project mi-proyecto --description "Objetivo del cambio"
htx roadmap add mi-roadmap
htx roadmap list
```

El cierre de una fase se controla con `common.roadmap-phase-close` y exige un reviewer independiente configurado.
