# Catálogo de workflows

Un workflow encadena tareas registradas. Antes de ejecutarlo, verifica que el perfil requerido y las capacidades que usa existan en tu agente o proyecto. El motor incluye cinco workflows JSON.

| ID | Propósito | Perfil requerido |
|---|---|---|
| `workflow.higpertext-plan` | Verifica el motor y abre una sesión con resources opcionales | `global` |
| `workflow.higpertext-review` | Verifica, registra notas y limpia la sesión | `global` |
| `workflow.guidelines-sync` | Sincroniza lineamientos y registra el resultado | `global` |
| `workflow.pr-quality-check` | Define una revisión de PR para un agente ADO que aporte sus tareas | `ado_admin` |
| `workflow.ado-release-flow` | Define un flujo de release para un agente ADO que aporte sus tareas | `ado_admin` |

## `higpertext-plan`

Comprueba la integridad del motor y abre una sesión. `--skills` y `--subagents` son opcionales y aceptan listas separadas por comas.

```bash
htx workflow run higpertext-plan --skills "best-practices"
```

## `higpertext-review`

Ejecuta la validación final, guarda las notas en memoria y limpia la sesión.

```bash
htx workflow run higpertext-review --notes "Cambios revisados y pruebas ejecutadas."
```

## `guidelines-sync`

Sincroniza lineamientos desde una URL Git o una ruta local. La fuente también puede estar configurada por el proyecto.

```bash
htx workflow run guidelines-sync --source ./mis-lineamientos
```

## Workflows de agentes externos

`pr-quality-check` y `ado-release-flow` requieren el perfil `ado_admin` y tareas especializadas que el motor base no registra. Úsalos únicamente dentro del agente externo que proporciona ese perfil y esas tareas. No son comandos de inicio rápido del motor.

Los agentes externos pueden añadir workflows propios en su proyecto. Documenta siempre su perfil requerido, parámetros, tareas encadenadas y efectos.
