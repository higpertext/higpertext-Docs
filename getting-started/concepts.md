# Conceptos clave

higpertext organiza el trabajo con agentes IA en cuatro pilares.

---

## Los 4 pilares

| Pilar | Comando | Qué hace |
|-------|---------|----------|
| **Init** | `htx agent init` | Prepara el proyecto para un asistente. |
| **Profile** | `htx profile load` | Asigna un rol al asistente con capacidades y reglas. |
| **Task** | `htx task <id>` | Ejecuta una capacidad atómica. |
| **Workflow** | `htx workflow run <id>` | Encadena capacidades en secuencia. |

---

## Perfil

Rol técnico con capacidades, reglas de gobernanza y recursos de sesión. Se carga con:

```bash
htx profile load <nombre> --assistant <claude|gemini|opencode>
```

---

## Capacidad (Task)

Herramienta atómica con parámetros de entrada y salida definidos:

```bash
htx task common.grep-search --pattern "class.*Service" --path src/
htx task common.list-rules   # ver todas las capacidades disponibles
```

---

## Hook

Script que se ejecuta automáticamente antes/después de una acción del asistente. Intercepta comandos y los redirige a la capacidad equivalente.

Ver: [Guía de Hooks](../guides/hooks-guide.md)

---

## Sesión

Estado activo que monta skills y subagentes en el asistente:

```
htx agent init + htx profile load  →  workspace base
htx task common.session-start      →  /spec /plan /build /review disponibles
htx task common.session-clean      →  workspace limpio
```

Ver: [Guía de Sesiones](../guides/sessions.md)
<!-- higpertext:generated-by=common.docs-sync -->
