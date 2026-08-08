# Trabajar con agentes externos

Un agente externo es un proyecto independiente que contiene la especialización que no pertenece al motor: capabilities propias, perfiles, reglas, hooks, workflows, pruebas y documentación. **higpertext Engine no copia ni absorbe esa lógica; la ejecuta y la integra.**

## Cuándo crear uno

Crea un agente externo cuando el comportamiento corresponde a un dominio concreto —por ejemplo, contenido, operaciones internas o una plataforma determinada— y debe evolucionar de forma independiente del motor.

No crees un agente para una tarea aislada que ya cubra una capability del motor. Primero consulta [el mapa de capacidades](../capabilities/walkhrough.md).

## Flujo completo

### 1. Diseñar y crear la estructura

Carga el perfil de diseño en un proyecto inicializado y crea el agente:

```bash
htx init --assistant codex
htx profile load agent_designer --assistant codex

htx task common.agent-builder \
  --profile mi_perfil \
  --target ../mi-agente \
  --description "Responsabilidad concreta del agente"
```

El agente queda en `../mi-agente`. Ahí vive el código que tú mantienes; el directorio `.higpertext/` es estado generado y no debe editarse a mano.

```text
mi-agente/
├── src/
│   ├── capabilities/       # lógica propia del agente
│   ├── config/profiles/    # roles del agente
│   ├── config/governance/  # reglas propias
│   └── workflows/          # flujos propios
├── tests/                  # pruebas de esa lógica
└── .higpertext/            # estado compilado por el motor
```

### 2. Definir el perfil y las capabilities

En el agente, crea `src/config/profiles/mi_perfil.json` y limita las capabilities al mínimo necesario. Las capabilities propias van en `src/capabilities/<namespace>/`; cada una debe declarar ID, parámetros, contrato y entrypoint.

Usa IDs completos, por ejemplo `mi_dominio.generar-reporte`, y no reutilices nombres reservados del motor (`global`, `base_agent`, `base_developer`, `base_operator`, `base_auditor`, `agent_designer`).

### 3. Compilar y registrar

```bash
# Recompila solo el estado generado del agente
htx agent init --profile mi_perfil --target ../mi-agente

# Registra el agente en el motor
htx task common.agent-sync --action register \
  --name mi-agente \
  --path ../mi-agente \
  --profile mi_perfil
```

### 4. Hacerlo portable

Instala el motor dentro del entorno del agente para que los hooks y el launcher no dependan de tu checkout local:

```bash
htx task common.agent-bootstrap --name mi-agente --assistant codex
```

Después, desde la raíz de `mi-agente`, carga su perfil y trabaja normalmente:

```bash
cd ../mi-agente
htx profile load mi_perfil --assistant codex
```

### 5. Mantener la sincronía

Cuando cambien las reglas o la integración, sincroniza el agente registrado. Para comprobar que su copia de `agent_designer` no se ha desviado del motor, usa `common.drift-check`.

```bash
htx task common.agent-sync --action sync --name mi-agente --assistant codex
htx task common.drift-check
```

## Responsabilidades claras

| Motor higpertext | Agente externo |
|---|---|
| Integración con asistentes, perfiles base, ejecución, hooks y runtime | Lógica de dominio, capabilities propias, perfiles especializados, pruebas y documentación |
| Capacidades transversales reutilizables | Comportamiento específico de una organización o producto |
| Nunca debe contener la lógica de negocio del agente | Es dueño de su lógica y de sus contratos |

→ [Referencia CLI](../reference/agent-cli.md) · [Capacidades de agentes](../capabilities/common/agents/common.agent-builder.md)
