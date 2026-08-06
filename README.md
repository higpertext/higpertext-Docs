<!-- higpertext:generated-by=docs-maintenance -->
# Documentación — higpertext Engine v0.8.5

Framework de orquestación de agentes IA para DevSecOps.
**53** capacidades · **6** perfiles · **8** hook intercepts activos

---

## Primeros pasos (`getting-started/`)

| Doc | Contenido |
|---|---|
| [Instalación](getting-started/installation.md) | Requisitos, `.venv`, `.env` |
| [Primer arranque](getting-started/first-run.md) | `init`, `profile load`, `session-start` |
| [Conceptos clave](getting-started/concepts.md) | Perfiles, capacidades, hooks, sesiones |

---

## Guías de uso (`guides/`)

| Doc | Contenido |
|---|---|
| [Guía de usuario](guides/user-guide.md) | Uso general del CLI |
| [Sesiones de Desarrollo](guides/sessions.md) | Ciclo de vida de sesiones, skills y subagentes |
| [Hooks — Guía de uso](guides/hooks-guide.md) | Qué interceptan los hooks y por qué existen |
| [Gobernanza](guides/governance.md) | Lineamientos obligatorios: PRs, seguridad, deployments |
| [Memoria del Agente](guides/agent-memory.md) | Cómo persiste y consulta el historial de acciones |
| [FAQ](guides/faq.md) | Portabilidad, entornos virtuales, secretos |

---

## Referencia técnica (`reference/`)

Especificaciones exhaustivas — qué existe y cómo funciona.

| Doc | Contenido |
|---|---|
| [Referencia del CLI](reference/agent-cli.md) | Comandos `htx` disponibles |
| [Catálogo de Capacidades](reference/capabilities-catalog.md) | 53 capacidades con parámetros, contratos y hook intercepts |
| [Catálogo de Perfiles](reference/profiles-catalog.md) | 6 perfiles con capacidades y recursos de sesión |
| [Hooks — Referencia técnica](reference/hooks-reference.md) | Arquitectura de hooks, reglas dinámicas, cómo extender |

---

## Capacidades (`capabilities/`)

Ficha individual por cada una de las 53 capacidades, agrupadas por área.

---

## Inicio rápido

```bash
# 1. Entorno
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2. Inicializar asistente
htx init --assistant claude

# 3. Cargar perfil
htx profile load base_developer --assistant claude

# 4. Verificar integridad
htx task common.higpertext-tester

# 5. Ejecutar una tarea
htx task common.quality-resolver --report code_quality_report.md
```

---

## Mapa de la documentación

```
docs/external/
├── README.md                  ← este archivo
├── getting-started/           ← empieza aquí
│   ├── installation.md
│   ├── first-run.md
│   └── concepts.md
├── guides/                    ← uso diario
│   ├── user-guide.md
│   ├── sessions.md
│   ├── hooks-guide.md         ← auto-generado
│   ├── governance.md
│   ├── agent-memory.md
│   └── faq.md
├── reference/                 ← técnico / exhaustivo
│   ├── agent-cli.md
│   ├── capabilities-catalog.md ← auto-generado
│   ├── profiles-catalog.md    ← auto-generado
│   └── hooks-reference.md     ← auto-generado
└── capabilities/              ← ficha por capacidad, por área
    ├── common/
    ├── git/
    └── security/
```

---

*higpertext Engine v0.8.5 · auto-generado por `scripts/docs_maintenance.py`*