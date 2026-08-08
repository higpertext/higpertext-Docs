# Guía de Usuario Extendida - higpertext Engine

El `htx` es el orquestador central (Hub) del higpertext Engine, usado para construir y gestionar agentes de IA (por ejemplo, vía el perfil `agent_designer`). Esta guía cubre desde la configuración inicial hasta flujos de trabajo avanzados.

---

## 🛠️ 1. Configuración Inicial

Antes de empezar, asegúrate de tener el entorno preparado.

### Variables de Entorno (`.env`)
higpertext requiere acceso a APIs externas para muchas de sus capacidades. Crea un archivo `.env` en la raíz del proyecto:
```bash
# Archivo .env
ADO_PAT=tu_personal_access_token_aqui
GITHUB_TOKEN=tu_token_opcional
```
*Nota: Este archivo está en el `.gitignore`, tus tokens son personales y seguros.*

### Entornos Virtuales (`.venv`)
Cada capacidad puede tener su propio entorno. Si una tarea falla por falta de librerías:
1. Ve a `app/<Tu Repositorio>/capabilities/[area]/scripts/`
2. Crea el venv: `python -m venv .venv`
3. Activa e instala: `pip install -r requirements.txt`
higpertext detectará el `.venv` y lo usará automáticamente.

---

## 🤖 2. Arquitectura de los 4 Pilares y Gestión de IA

higpertext Engine organiza la interacción con agentes de inteligencia artificial a través de 4 pilares conceptuales estrictos:

1. **`init`**: Prepara el espacio de trabajo (workspace) en blanco e instala el puente de comunicación.
2. **`profile`**: Asigna un rol o identidad técnica específica al workspace inyectando sus permisos y contratos.
3. **`task` / `skills`**: Ejecuta herramientas o capacidades atómicas en el código.
4. **`memory` / `activity`**: Registra la actividad, minutas y lecciones en la memoria del agente.

---

### Inicialización de Workspace (`init`)
Este comando prepara un proyecto desde cero para trabajar con un asistente de IA sin asignarle ningún rol por defecto. Soporta la tilde `~/` para apuntar de forma transparente al directorio del usuario.

```powershell
# Inicializar en el directorio actual o repositorio base
htx init --assistant [codex|gemini|claude|copilot|opencode|antigravity|agent]

# Inicializar en un proyecto externo (con ruta absoluta o relativa al home con ~/)
htx init --assistant gemini --target "~/Documents/MiAppCliente"
```

**¿Qué ocurre internamente?**
1. Crea la carpeta del asistente (ej. `.gemini/`) en el proyecto destino y genera la guía inicial `GEMINI.md`. Esta guía incluye automáticamente:
   - Sección explicativa sobre autogestión y creación de perfiles personalizados.
   - **⚡ Reglas Críticas de Reducción de Tokens** (para forzar concisión y diffs en lugar de archivos completos).
   - Guía de persistencia de memoria con ejemplos de registro manual.
2. Crea el archivo de metadatos `.higpertext/environment.json` en el proyecto destino. Si posteriormente inicializas un segundo asistente (ej. `htx init --assistant claude`), higpertext mantiene un registro acumulativo en la clave `"assistants": ["gemini", "claude"]` sin sobrescribir ni perder el historial.
3. Crea un archivo delegador `htx` en ese proyecto. **Este archivo contiene la ruta absoluta exacta** hacia el orquestador central del Hub, lo que te permite abrir una terminal en `~/Documents/MiAppCliente` y ejecutar ahí directamente `htx task audit-vars` sin configurar rutas relativas ni variables complejas.

---

### Asignación e Inyección de Perfiles (`profile load`)
Una vez inicializado el proyecto para el asistente requerido, este comando inyecta el "cerebro" y los permisos específicos de un rol en el asistente. Soporta de igual forma `~/` en `--target`.

```powershell
# Asignar rol Agent Designer al proyecto destino para Claude
htx profile load agent_designer --assistant claude --target "~/Documents/agent-designer"
```

**Control Estricto de Gobernanza y Acumulación Multitarea**:
1. **Validación de Asistente**: Si intentas cargar un perfil para un asistente que no está registrado en la lista de `"assistants"` de `environment.json`, higpertext bloqueará de inmediato la operación por seguridad y te indicará el comando exacto para autorizarlo (`htx init --assistant <nombre>`).
2. **Perfiles Acumulativos**: Cada vez que ejecutas exitosamente `profile load` para un nuevo rol (ej. `agent_designer`, `ado_admin`), higpertext lo añade a una lista acumulativa `"active_profiles": ["agent_designer", "ado_admin"]` en `environment.json`, resguardando permanentemente la trazabilidad de todos los roles asignados a tu proyecto.
3. **Subcarpeta Estandarizada `agents/`**: Las reglas del rol, el *system prompt*, los contratos técnicos y las directivas de ahorro de tokens se inyectan limpiamente dentro de una subcarpeta estandarizada según el entorno:
   - Gemini: `.gemini/agents/<perfil>.agent.md`
   - Claude: `.claude/agents/<perfil>.md` (y su respectiva copia en `.clauderules`)
   - Copilot: `.github/agents/<perfil>.md` (y `.github/copilot-instructions.md`)
   - OpenCode: `.opencode/agents/<perfil>.md`

---

## 🏃 3. Ejecución de Tareas (`task`)

El comando `task` es el corazón de higpertext. Permite invocar cualquier script registrado en el sistema.

### Sintaxis General
```powershell
htx task [id-tarea] --[parametro1] [valor1] --[parametro2] [valor2]
```

### Ejemplos Reales:
1.  **Generar Presentación Premium**:
    `htx task html-presentation --input mi_reporte.md --output final.html`
2.  **Recolectar Datos de Incidente**:
    `htx task postmortem-generator --incident incident.json`
3.  **Monitorear Servicio**:
    `htx task monitor-service --resource PROD-API-01`

---

## 🔁 4. Sesiones de Desarrollo (`session-start` / `session-clean`) {#sesiones-de-desarrollo}

Las skills y subagentes son **recursos efímeros**: no se almacenan en disco de forma permanente. Solo existen dentro de las carpetas del asistente (`skills/`, `subagents/`) mientras hay una sesión activa. Al hacer `init` o `profile load` esas carpetas **no se crean**.

### Ciclo de vida

```
init / profile load  →  workspace vacío (sin skills ni subagents en disco)
         │
session-start        →  monta skills y subagents del perfil en .claude/skills/ y .claude/subagents/
         │               compila los comandos (/spec, /plan, /build, /review) con las listas reales
         │
   [trabajo activo]  →  el asistente tiene acceso a skills y subagents montados
         │
session-clean        →  desmonta skills y subagents (rmtree)
                         restaura comandos al estado pristino (_No active session_)
```

### Comandos

```powershell
# Iniciar sesión — lee session_skills y session_subagents del perfil activo
htx task session-start --profile <nombre-perfil>

# Iniciar sesión con recursos explícitos (sobreescribe los del perfil)
htx task session-start --profile pwsh_engineer --skills "cross-cutting-powershell-standards" --subagents "test-engineer"

# Cerrar sesión y limpiar todos los recursos efímeros
htx task session-clean
```

### Recursos por perfil

Cada perfil JSON declara sus recursos de sesión en los campos `session_skills` y `session_subagents`:

| Perfil | `session_skills` | `session_subagents` |
|---|---|---|
| `pwsh_engineer` | `cross-cutting-powershell-standards`, `testing-pester-testing` | `test-engineer` |
| `ado_admin` | — | `ado-pipeline-guardian`, `architect` |
| `agent_designer` | `best-practices` | — |
| `sre` | — | `ado-pipeline-guardian`, `architect` |
| `agents_architect` | — | todos los subagents disponibles |

> Perfiles privados de proyectos específicos no se listan aquí — cada agente documenta sus propios `session_skills`/`session_subagents` en su repositorio.

### Estado de los comandos según sesión

Los comandos `/spec`, `/plan`, `/build` y `/review` reflejan el estado de sesión en la sección `## Active skills` / `## Active subagents`:

- **Sin sesión**: `_No active session — run session-start to mount skills._`
- **Con sesión**: lista de links a los archivos montados en `.claude/skills/`

---

## 🕒 6. Automatización con Scheduler

higpertext puede ejecutar tareas de forma periódica usando el `scheduler`.

```powershell
htx task scheduler --task_command "[comando_completo]" --interval [tiempo]
```
**Formatos de tiempo**: `30s` (segundos), `10m` (minutos), `1h` (horas).

*Ejemplo de monitoreo cada 5 minutos*:
`htx task scheduler --task_command "htx task monitor-service --resource DB-01" --interval 5m`

---

## 🧠 7. Gestión de Memoria

Para que los agentes no "olviden" lo aprendido y para mantener un registro de soluciones y minutas, higpertext Engine incorpora un sistema de memoria aislado por proyecto.

### Auto-guardado en Segundo Plano (Background Recording)
Cada vez que el agente o tú ejecutan una tarea (`htx task ...`), higpertext captura en segundo plano el estado final (`success` o `failure`), todos los parámetros utilizados y las salidas estándar (`stdout`/`stderr`).
- Si un comando falla, se registra inmediatamente el error en el historial para futuras consultas.

### Registro Manual de Soluciones y Minutas
Cuando resuelvas un bug o completes una actividad clave en el proyecto, registra la lección explícitamente:
```powershell
htx task memory-manager --action "Resolución Bug de Compilación" --status success --notes "Causa raíz: versión de librería incompatible. Solución: fijada v2.1 en requirements.txt."
```

### Aislamiento Estricto por Proyecto
La memoria se almacena y aísla exactamente en la carpeta `.memory/` dentro del directorio de trabajo actual (`CWD`).
-   **Contexto**: `.memory/context.md` (Resumen acumulado en Markdown para fácil lectura por el agente y el humano).
-   **Historial**: `.memory/session_history.json` (Historial estructurado JSON para procesos de auditoría y análisis del sistema).

---

## 🔍 8. Consulta de Conocimiento (`ask`)

Busca en toda la base de conocimientos del proyecto (Gobernanza, Minutas, Docs):
```powershell
htx ask "reglas de firewall"
```

---

## 📖 9. Gestión de Wiki y Documentación (`wiki-manager`)

higpertext mantiene la documentación técnica estandarizada e indexada en la carpeta de la wiki de tu proyecto.

```powershell
# Inicializar estructura base (Home, Arquitectura, Runbooks...)
htx task wiki-manager --action init --path wiki

# Crear o actualizar una página específica (asegura título H1 y reindexa)
htx task wiki-manager --action page --title "Guía Operativa" --content "# Guía Operativa\n\nPasos..." --path wiki

# Regenerar el índice automático (_Sidebar e Index)
htx task wiki-manager --action index --path wiki
```

---

## 💡 10. Mejores Prácticas

1.  **Sigue el Contrato**: Si editas un Markdown manualmente, asegúrate de cumplir el contrato técnico de la capacidad para que la presentación no se rompa.
2.  **Limpia la Memoria**: Si el contexto del agente se vuelve muy pesado, puedes archivar o limpiar la carpeta `.memory/`.
3.  **Detección de Errores**: Si higpertext lanza un `[WARNING]`, léelo. Normalmente te dirá si te falta un PAT o una librería.

---
## 🛡️ 11. Calidad y Testing

higpertext Engine incluye una suite de pruebas para garantizar la estabilidad del sistema.

### Preparación del Entorno
Antes de ejecutar los tests, asegúrate de activar el entorno de pruebas:
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Unit Tests (Pruebas de Motor)
Validan la lógica interna del núcleo de higpertext (Python).
```powershell
# Ejecutar todos los tests unitarios con pytest (recomendado) o unittest
pytest tests/
```

### Suite de Diagnóstico (higpertext-tester)
Valida la integridad de los archivos y los contratos técnicos de las capacidades.
```powershell
htx task higpertext-tester
```

### Pruebas de Comportamiento del Modelo (LLM Evals)
Evalúan cómo interactúan los Agentes (ej. Gemini) frente al contexto proporcionado por higpertext (restricción de rol, seguimiento del tono profesional y tool-calling preciso).

**Requisitos Previos:**
Para ejecutar esta suite, necesitas configurar tu clave de API de Gemini. Copia el archivo `.env.example` como `.env` e inserta tu token:
```env
GEMINI_API_KEY="AIzaSy..."
```

**Ejecución:**
```powershell
# Ejecuta los test con salida de terminal para ver las respuestas del modelo
pytest tests/behavioral/ -v -s
```

### Reglas para Colaboradores
- Antes de un commit, el `higpertext-tester` y los Unit Tests deben estar en verde (OK).
- Si creas una nueva lógica en el `kernel`, añade su correspondiente test en `tests/`.
- Si agregas un nuevo rol o perfil, añade sus casos de prueba en `tests/behavioral/eval_cases.json`.

---
[Volver al Índice](../README.md)
<!-- higpertext:generated-by=common.docs-sync -->
