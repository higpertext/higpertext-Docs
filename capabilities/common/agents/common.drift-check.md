# Capability: `common.drift-check`

Comprueba si la copia de `agent_designer.json` de los agentes externos registrados se ha desviado de la versión del motor.

```bash
htx task common.drift-check
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--engine-root` | No | Raíz del motor; por defecto, el directorio actual |

El resultado es exitoso solo si todos los agentes registrados están sincronizados.
