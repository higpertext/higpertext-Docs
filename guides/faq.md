# FAQ — Preguntas Frecuentes

---

## Instalación y Entorno

**¿Dónde debo colocar mi archivo `.env`?**

Siempre en la raíz del proyecto (donde está `htx`). El motor establece el directorio de ejecución en la raíz, por lo que los scripts buscarán las variables allí.

---

**¿Por qué usar `.venv` y no instalar globalmente?**

Las capacidades tienen dependencias específicas (`radon`, `bandit`, `pytest-cov`) que pueden entrar en conflicto con el sistema. El motor detecta automáticamente `.venv/bin/python` si existe.

```bash
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
# .venv\Scripts\Activate.ps1  # Windows
pip install -r requirements.txt
```

---

**¿Puedo usar higpertext en múltiples proyectos simultáneamente?**

Sí. higpertext es 100% portable. Ejecuta `init` y `profile load` en cada proyecto con `--target`:

```bash
htx init --assistant claude --target ~/Documents/ProyectoA
htx profile load sre --assistant claude --target ~/Documents/ProyectoA
```

Cada proyecto tiene su propio `.higpertext/environment.json` aislado.

---

**¿Qué pasa si muevo la carpeta del proyecto?**

Nada. Gracias a `ROOT_DIR = Path(__file__).parent.absolute()`, todas las rutas internas se calculan dinámicamente. Puedes mover la carpeta completa a cualquier ruta.

---

## Perfiles y Capacidades

**¿Puedo asignar múltiples perfiles a un proyecto?**

Sí, los perfiles se acumulan sin sobrescribirse:

```bash
htx profile load sre --assistant claude
htx profile load devsecops --assistant claude
# → "active_profiles": ["sre", "devsecops"]
```

---

**Una capacidad falló con "Contrato técnico no cumplido". ¿Qué hago?**

1. Lee el mensaje de error — indica qué regla falló
2. Ejecuta el script directamente para ver el output real:

```bash
python src/capabilities/ado_admin/scripts/code_quality.py --path ./src
```

3. Revisa el contrato en el JSON de la capacidad: `required_patterns`, `expected_files`

---

## Seguridad

**¿Mis tokens y PATs están seguros?**

El archivo `.env` está en `.gitignore` — nunca se commitea. higpertext nunca loguea valores de variables de entorno.

Si expones un secreto accidentalmente: rótalo inmediatamente y registra el incidente con `memory-manager`.

---

## Sesiones y Workflows

**¿Cuál es la diferencia entre `task` y `workflow run`?**

- `task`: Ejecuta una sola capacidad atómica
- `workflow run`: Ejecuta una cadena de capacidades en secuencia con manejo de fallos

---

**Los comandos `/spec`, `/plan` no muestran skills activas. ¿Por qué?**

Necesitas iniciar una sesión:

```bash
htx task session-start --action start --profile <perfil>
```

Ver: [Sesiones de Desarrollo](sessions.md)

---

## Memoria y Documentación

**¿La memoria es compartida entre usuarios del equipo?**

No. `.memory/` está en `.gitignore`. Los aprendizajes e historial son locales y personales para cada desarrollador.

---

**¿Cómo funcionan los Contratos Técnicos en la IA?**

Los contratos se inyectan como instrucciones críticas en los archivos de reglas del agente. La IA no adivina el formato — el motor le dicta exactamente qué output producir para que sea compatible con el siguiente paso del pipeline.

---

**¿Cómo actualizo el catálogo de documentación tras agregar capacidades?**

```bash
htx workflow run docs-update
```

---

## Problemas Comunes

| Error | Causa | Solución |
|---|---|---|
| `ModuleNotFoundError: No module named 'radon'` | Dependencias no instaladas | `pip install -r requirements.txt` |
| `Profile not found: mi_perfil` | Nombre incorrecto | `ls src/config/profiles/` para ver nombres exactos |
| `Assistant 'claude' not registered` | Asistente no inicializado | `htx init --assistant claude` primero |
| `Contrato técnico no cumplido` | Script no generó output esperado | Ejecutar script directamente para debuggear |

---

[Volver al Índice](../README.md)
<!-- higpertext:generated-by=common.docs-sync -->
