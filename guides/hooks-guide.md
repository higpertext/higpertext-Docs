# Guía de hooks

Los hooks conectan el asistente con las capacidades de higpertext. Se generan al ejecutar `htx init` y `htx profile load`; no necesitas configurarlos para empezar a trabajar.

Cuando un asistente intenta una operación con una capability equivalente, el hook puede recomendar la tarea estructurada correspondiente. Por ejemplo:

| Intención | Capacidad sugerida |
|---|---|
| Buscar texto | `common.grep-search` |
| Revisar cambios Git | `git.diff` |
| Listar archivos Git | `git.ls-files` |
| Crear un commit | `git.committer` |
| Gestionar dependencias | `common.dep-manager` |

Los hooks no sustituyen tu criterio: protegen flujos repetibles y conservan contratos de salida para el asistente. Para diagnosticar la integración usa:

```bash
htx task common.hook-health
htx task common.hook-sync-check
```

En agentes externos, define hooks y scripts propios dentro de `src/config/hooks/`; no edites `.higpertext/` a mano. Ver [Agentes externos](../getting-started/external-agents.md) y la [referencia técnica](../reference/hooks-reference.md).
