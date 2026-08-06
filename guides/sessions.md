# Sesiones de Desarrollo

Las skills y subagentes son **recursos efímeros**: no existen en disco fuera de una sesión activa. Esta guía cubre el ciclo de vida completo.

---

## Conceptos Clave

| Término | Descripción |
|---|---|
| **Sesión** | Estado temporal activo gestionado por `session-start` y `session-clean` |
| **Skill** | Regla de comportamiento especializada (ej. estándares de PowerShell) |
| **Subagente** | Rol especializado que el LLM puede adoptar (ej. `test-engineer`) |
| **Recurso efímero** | Archivo creado al iniciar sesión y eliminado al cerrarla |
| **Roadmap** | `.higpertext/config/roadmaps/<id>.json` (+ `active.json`) — determina qué skills/subagentes monta `session-start` |

---

## Estado Sin Sesión (Predeterminado)

Tras `init` o `profile load`, las carpetas de recursos efímeros **no existen**:

```
.claude/
├── commands/       ← comandos permanentes (spec, plan, build, review)
│   └── spec.md     ← muestra "_No active session_" en la sección de skills
└── rules/          ← reglas del perfil activo (permanentes)
# skills/ y subagents/ NO EXISTEN
```

Los comandos `/spec`, `/plan`, `/build`, `/review` están disponibles pero muestran `_No active session_` hasta que se inicia una sesión.

---

## Iniciar una Sesión

```bash
# Monta skills y subagentes del perfil activo
htx task session-start --action start --profile software_developer

# Monta skills y subagentes específicos
htx task session-start --action start --profile software_developer \
    --skills "cross-cutting-standards" \
    --subagents "test-engineer,architect"
```

**¿Qué ocurre internamente?**

```
session-start --profile software_developer
    │
    ├─ Lee software_developer.json → session_skills + session_subagents
    ├─ Si hay roadmaps activos en .higpertext/config/roadmaps/active.json → usa session_resources (prioridad media)
    ├─ Genera .claude/skills/<skill>/SKILL.md  (desde src/higpertext/templates/skills/)
    ├─ Genera .claude/subagents/<agent>.md     (desde src/higpertext/templates/subagents/)
    ├─ Compila .claude/commands/*.md           (reemplaza placeholders de skills/subagentes)
    └─ Escribe .higpertext/session.json             (session_id, profile, status: active)
```

**Estado con sesión activa:**

```
.claude/
├── commands/
│   └── spec.md     ← lista links reales a skills activas
├── rules/
├── skills/
│   └── cross-cutting-standards/
│       └── SKILL.md
└── subagents/
    ├── test-engineer.md
    └── architect.md
```

---

## Cerrar una Sesión

```bash
htx task session-clean --action clean
```

**¿Qué ocurre internamente?**

```
session-clean
    │
    ├─ rmtree .claude/skills/
    ├─ rmtree .claude/subagents/
    ├─ Restaura .claude/commands/*.md al estado pristino (_No active session_)
    └─ Elimina .higpertext/session.json
```

---

## Prioridad de Recursos en `session-start`

```
--skills / --subagents (args explícitos)       ← máxima prioridad
    │
.higpertext/config/roadmaps/ (roadmaps activos) → session_resources  ← prioridad media (merge)
    │
perfil activo → session_skills/session_subagents  ← fallback
```

---

## Roadmaps de Proyecto (`.higpertext/config/roadmaps/`)

El roadmap conecta la planificación con las sesiones. Lo genera `/plan` al aprobarse un plan,
usando siempre el CLI — nunca escribiendo el JSON a mano:

```bash
# 1. Crear el roadmap (scaffold)
htx roadmap new mi-roadmap --project mi-roadmap --description "Objetivo del proyecto"

# 2. Editar mi-roadmap.json: agregar "id"/"name" (obligatorios) y las phases[]

# 3. Activarlo — sus session_resources empiezan a montarse en cada sesión
htx roadmap add mi-roadmap

# Ver todos los roadmaps (activos e inactivos) y su progreso
htx roadmap list

# Desactivarlo cuando el objetivo esté completo (el archivo queda como historial)
htx roadmap remove mi-roadmap
```

Puede haber **varios roadmaps activos a la vez** — cada uno vive en su propio archivo
`.higpertext/config/roadmaps/<id>.json`, y `active.json` en esa carpeta lista cuáles están
activos. `session-start` mergea los `session_resources` de todos los roadmaps activos (sin
duplicados).

```json
{
  "id": "mi-roadmap",
  "name": "Mi Roadmap",
  "project": "mi-proyecto",
  "profile": "",
  "description": "Objetivo del proyecto",
  "phases": [
    {
      "id": "phase-1",
      "name": "Setup inicial",
      "status": "pending",
      "skills": ["cross-cutting-standards"],
      "subagents": ["architect"]
    }
  ],
  "session_resources": {
    "skills": ["cross-cutting-standards"],
    "subagents": ["architect", "test-engineer"]
  }
}
```

`session_resources` es la unión de todas las skills/subagents de todas las fases — es lo que
`session-start` monta automáticamente si el roadmap está activo. `id`/`name` son obligatorios
(el generador de reportes de cierre falla sin ellos); `profile` es opcional — si se declara, el
roadmap solo se mergea cuando ese perfil está activo en la sesión.

---

## Workflow Rápido de Sesión

```bash
# 1. Inicializar workspace (solo una vez por proyecto)
htx init --assistant claude

# 2. Asignar perfil (solo cuando cambia el rol)
htx profile load software_developer --assistant claude

# 3. Iniciar sesión antes de trabajar
htx task session-start --action start --profile software_developer

# ... usar /spec, /plan, /build, /review en el asistente IA ...

# 4. Cerrar sesión al terminar
htx task session-clean --action clean
```

---

## Usar el Workflow Automatizado

El workflow `higpertext-plan` automatiza los pasos de pre-chequeo + apertura de sesión:

```bash
htx workflow run higpertext-plan \
    --skills "cross-cutting-standards" \
    --subagents "test-engineer"
```

Y `higpertext-review` automatiza la certificación final + cierre:

```bash
htx workflow run higpertext-review \
    --notes "Implementé el módulo X. Aprendizaje: validar contratos antes de deploy."
```

---

## Telemetría de Sesiones

Cada sesión genera eventos automáticos en `.higpertext/telemetry.jsonl`:

- `session_start` al activarse — registra `session_id` y `profile`
- `session_stop` al cerrar — calcula `duration_min` desde el inicio
- `commit` por cada commit vía `ado_admin.committer`
- `tool_call` por cada herramienta usada (estimación de tokens y costo)

Para ver el historial de sesiones y productividad:

```bash
htx task common.telemetry-report --days 7
```

Ver referencia completa: [Telemetría](../reference/telemetry.md)

---

[Volver al Índice](../README.md)
<!-- higpertext:generated-by=common.docs-sync -->
