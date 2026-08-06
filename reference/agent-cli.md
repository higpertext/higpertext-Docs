# Referencia CLI — `higpertext agent`

Referencia completa del subcomando `agent` para gestionar agentes higpertext sin asistente.

---

## Subcomandos

```
htx agent <subcomando> [opciones]
```

| Subcomando | Descripción |
|------------|-------------|
| `init` | Crea o actualiza el ambiente del agente |
| `status` | Muestra el estado del ambiente actual |
| `clean` | Elimina la sesión efímera |

---

## `agent init`

Crea la estructura completa del agente e inicializa su ambiente.

```bash
htx agent init --profile <nombre> [--target <ruta>]
```

| Parámetro | Requerido | Descripción |
|-----------|-----------|-------------|
| `--profile` | Sí | Nombre del perfil a activar |
| `--target` | No | Ruta del directorio del agente (default: directorio actual) |

**Qué hace:**

1. Crea la estructura `src/` si no existe
2. Copia `agent_designer.json` a `src/config/profiles/`
3. Genera `htx` launcher apuntando al motor (solo si no existe)
4. Escribe `.higpertext/config/environment.json`
5. Compila hook layers → `.higpertext/config/hooks_config.json`
6. Actualiza `.higpertext/state/semantic_graph.md`

**Idempotente**: seguro de ejecutar múltiples veces. No sobreescribe `htx` ni `src/config/hooks/custom/`.

**Nombres reservados** (error si se usan como `--profile`):

```
base_agent, agent_designer, global,
base_developer, base_operator, base_auditor
```

**Ejemplo:**
```bash
htx agent init --profile content_creator --target ../content-creator
```

---

## `agent status`

Muestra el estado del ambiente del agente.

```bash
htx agent status [--target <ruta>]
```

**Output:**
```
── Agent Status ───────────────────────────
  Perfil(es) : ['content_creator']
  Entorno    : .higpertext/config/environment.json
  Hooks      : compilados
```

---

## `agent clean`

Elimina la sesión efímera sin borrar la configuración base.

```bash
htx agent clean [--target <ruta>]
```

Elimina `.higpertext/state/session.json`. No toca `src/`, `htx` ni `.higpertext/config/`.

---

## Comparación con `init` y `profile load`

| Comando | Asistente | Genera `.claude/` | Genera hooks | Crea `src/` |
|---------|-----------|-------------------|--------------|-------------|
| `init --assistant claude` | Requerido | Sí | Sí (nativos) | No |
| `profile load` | Requerido | Sí | Sí (nativos) | No |
| `agent init` | **No** | **No** | Sí (`.higpertext/`) | **Sí** |

`agent init` es el punto de entrada para agentes que no se integran con un asistente específico, o que lo harán después con `profile load`.

---

## Flujo típico de un agente nuevo

```bash
# 1. Crear el agente
htx agent init --profile mi_perfil --target ../mi-agente

# 2. Verificar
htx agent status --target ../mi-agente

# 3. Desarrollar (editar src/)
# ...

# 4. Recompilar tras cambios
htx agent init --profile mi_perfil --target ../mi-agente

# 5. Integrar con asistente (opcional, desde el agente)
htx profile load mi_perfil --assistant claude
```

---

## Ver también

- [Creación de Agentes](../03-extending/agent-development.md) — guía paso a paso
- [Hooks de Agente](../03-extending/agent-hooks.md) — sistema de hooks por capas
- [Perfiles — Referencia](../04-reference/profiles-catalog.md) — catálogo de perfiles
<!-- higpertext:generated-by=common.docs-sync -->
