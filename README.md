<!-- higpertext:generated-by=common.docs-sync -->
# Documentación — higpertext Engine v0.8.5

Framework de orquestación de agentes IA para DevSecOps.
**54** capacidades · **0** perfiles · **8** hook intercepts activos

---

## Primeros pasos (`01-getting-started/`)

| Doc | Contenido |
|---|---|
| [Instalación](01-getting-started/installation.md) | Requisitos, `.venv`, `.env` |
| [Primer arranque](01-getting-started/first-run.md) | `init`, `profile load`, `session-start` |
| [Conceptos clave](01-getting-started/concepts.md) | Perfiles, capacidades, hooks, sesiones, workflows |

---

## Guías de uso (`02-guides/`)

| Doc | Contenido |
|---|---|
| [Sesiones de Desarrollo](02-guides/sessions.md) | Ciclo de vida de sesiones, skills y subagentes |
| [Hooks — Guía de uso](02-guides/hooks-guide.md) | Qué interceptan los hooks y por qué existen |
| [Gobernanza](02-guides/governance.md) | Lineamientos obligatorios: PRs, seguridad, deployments |
| [Memoria del Agente](02-guides/agent-memory.md) | Cómo persiste y consulta el historial de acciones |
| [FAQ](02-guides/faq.md) | Portabilidad, entornos virtuales, secretos |

---

## Extender el framework (`03-extending/`)

| Doc | Contenido |
|---|---|
| [Desarrollo de Capacidades](03-extending/capability-development.md) | Crear nueva capacidad con contrato y hook |
| [Workflows Personalizados](03-extending/custom-workflows.md) | Cómo crear flujos YAML por proyecto |
| [Gobernanza Personalizada](03-extending/custom-guidelines.md) | Cómo estructurar lineamientos propios |

---

## Referencia técnica (`04-reference/`)

Especificaciones exhaustivas — qué existe y cómo funciona internamente.

| Doc | Contenido |
|---|---|
| [Arquitectura](04-reference/architecture.md) | Kernel, perfiles, capacidades, adaptadores LLM y flujo de ejecución |
| [Catálogo de Capacidades](04-reference/capabilities-catalog.md) | 54 capacidades con parámetros, contratos y hook intercepts |
| [Catálogo de Perfiles](04-reference/profiles-catalog.md) | 0 perfiles con capacidades y recursos de sesión |
| [Hooks — Referencia técnica](04-reference/hooks-reference.md) | Arquitectura de hooks, reglas dinámicas, cómo extender |
| [Gobernanza — Referencia](04-reference/governance-reference.md) | Lineamientos, formato de commit, reglas activas |
| [Módulos del Kernel](04-reference/kernel-modules.md) | API interna de cada módulo Python del kernel |
| [Adaptadores LLM](04-reference/llm-adapters.md) | Cómo cada adaptador genera reglas para su asistente IA |
| [Telemetría](04-reference/telemetry.md) | Métricas de uso, tokens y productividad |

---

## Flujos de Trabajo (`workflows/`)

| Doc | Dominio |
|---|---|
| [Spec → Plan → Sesión](workflows/how-to/spec-plan-session.md) | Ciclo de desarrollo |
| [Motor de Playbooks](workflows/engine/playbooks-reference.md) | Técnico |
| [De Incidente a Presentación](workflows/sre/postmortem-to-presentation.md) | SRE |
| [Monitoreo Continuo](workflows/sre/continuous-monitoring.md) | SRE |
| [Bootstrap Multi-Proyecto](workflows/ops/multi-project-migration.md) | Ops |

Ver criterio workflow vs capacidad: [workflows/README.md](workflows/README.md)

---

## Inicio rápido

```bash
# 1. Entorno
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2. Inicializar asistente
htx init --assistant claude

# 3. Cargar perfil
htx profile load software_developer --assistant claude

# 4. Verificar integridad
htx task common.higpertext-tester

# 5. Ejecutar una tarea
htx task common.quality-scan --path ./src
```

---

## Mapa de la documentación

```
docs/
├── README.md                          ← este archivo
├── 01-getting-started/                ← empieza aquí
│   ├── installation.md
│   ├── first-run.md
│   └── concepts.md
├── 02-guides/                         ← uso diario
│   ├── sessions.md
│   ├── hooks-guide.md                 ← auto-generado
│   ├── governance.md
│   ├── agent-memory.md
│   └── faq.md
├── 03-extending/                      ← extender el framework
│   ├── capability-development.md      ← auto-generado
│   ├── custom-workflows.md
│   └── custom-guidelines.md
├── 04-reference/                      ← técnico / exhaustivo
│   ├── architecture.md
│   ├── capabilities-catalog.md        ← auto-generado
│   ├── profiles-catalog.md            ← auto-generado
│   ├── hooks-reference.md             ← auto-generado
│   ├── kernel-modules.md
│   ├── llm-adapters.md
│   ├── telemetry.md
│   └── governance-reference.md
└── workflows/
    ├── README.md                      ← criterio workflow vs capacidad
    ├── how-to/
    ├── engine/
    ├── sre/
    └── ops/
```

---

*higpertext Engine v0.8.5 · auto-generado por `common.docs-sync`*