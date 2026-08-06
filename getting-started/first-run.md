# Primer Arranque

Tres comandos para tener el asistente listo después de instalar.

> Si quieres autocompletado, configúralo una sola vez durante la instalación:
> `htx completion install` y luego `source ~/.bashrc`. Las actualizaciones no
> requieren repetir `completion install`.

---

## Paso 1 — Inicializar el workspace

```bash
htx agent init --profile global

# Después, genera la integración para el asistente elegido
htx profile load global --assistant claude
```

Asistentes disponibles para `profile load`: `claude`, `gemini`, `opencode`, `copilot`, `antigravity`.

| Asistente | Archivos generados |
|-----------|-------------------|
| `claude` | `CLAUDE.md`, `.claude/rules/` |
| `gemini` | `GEMINI.md`, `.gemini/rules/` |
| `opencode` | `AGENTS.md`, `.opencode/rules/` |
| `copilot` | `.github/copilot-instructions.md` |

---

## Paso 2 — Cargar un perfil

```bash
htx profile load software_developer --assistant claude
```

Perfiles disponibles:

| Perfil | Especialidad |
|--------|-------------|
| `software_developer` | Desarrollo, Clean Code, TDD |
| `devsecops` | Seguridad, vulnerabilidades, compliance |
| `sre` | Kubernetes, postmortems, monitoreo |
| `ado_admin` | Azure DevOps, pipelines, PRs |
| `pwsh_engineer` | Scripts PowerShell |

Ver catálogo completo: [Catálogo de Perfiles](../reference/profiles-catalog.md)

---

## Paso 3 — Iniciar sesión

```bash
htx task common.session-start
```

Activa los comandos `/spec`, `/plan`, `/build`, `/review` en tu asistente.

Al terminar el trabajo:

```bash
htx task common.session-clean
```

---

## Verificar instalación

```bash
htx task common.higpertext-tester
```

Todos los contratos deben aparecer en verde ✓

---

## Siguiente paso

→ [Conceptos clave](concepts.md) — perfiles, capacidades, hooks y sesiones
<!-- higpertext:generated-by=common.docs-sync -->
