# Memoria del Agente

El sistema de memoria persiste el historial de acciones, aprendizajes y sesiones del agente en `.memory/`. Permite que el agente aprenda de ejecuciones anteriores y evite repetir errores.

---

## Estructura de Archivos

```
.memory/
├── context.md          ← Resumen legible (regenerado automáticamente)
├── journal.json        ← Historial completo de acciones (append-only)
├── learnings.json      ← Aprendizajes indexados por tag
└── session_history.json ← Historial de sesiones anteriores
```

---

## `journal.json` — Historial de Acciones

Cada entrada tiene esta estructura:

```json
{
  "action_id": "act-20260525-001",
  "timestamp": "2026-05-25T10:30:00",
  "action": "Refactorización del módulo de calidad",
  "status": "success",
  "notes": "Se detectó que radon no estaba instalado. Se añadió a requirements.txt.",
  "learned": ["verificar dependencias antes de ejecutar análisis estático"],
  "tags": ["code-quality", "dependencies"],
  "failure_root_cause": null
}
```

---

## `learnings.json` — Aprendizajes por Tag

Indexa los aprendizajes del `journal.json` agrupados por etiquetas para búsqueda rápida:

```json
{
  "code-quality": [
    "verificar dependencias antes de ejecutar análisis estático",
    "radon require Python 3.10+"
  ],
  "SECURITY": [
    "nunca loguear valores de PAT aunque sea en modo debug"
  ]
}
```

---

## `context.md` — Resumen Legible

Regenerado automáticamente por `memory-manager` tras cada acción. Contiene:
- Últimas 10 acciones con estado
- Top 5 aprendizajes por tag más activo
- Fallas recientes con causa raíz

**No editar manualmente** — es sobreescrito en cada registro.

---

## Registrar una Acción Manualmente

```bash
htx task common.memory-manager \
    --action "Implementación del módulo de autenticación" \
    --status "success" \
    --notes "Se usó JWT con expiración de 1h. El refresh token se guarda en httpOnly cookie." \
    --learned "JWT,cookies httpOnly para refresh token" \
    --tags "security,auth"
```

### Registrar una Falla

```bash
htx task common.memory-manager \
    --action "Deploy a staging" \
    --status "failure" \
    --notes "El pipeline falló en el step de tests de integración." \
    --failure_root_cause "Variable DATABASE_URL no estaba configurada en el entorno de staging." \
    --tags "deploy,config"
```

### Parámetros Completos

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Nombre descriptivo de la acción realizada |
| `--status` | Sí | `success` o `failure` |
| `--notes` | Sí | Notas técnicas, contexto, detalles |
| `--action_id` | No | ID único (autogenerado si se omite) |
| `--learned` | No | Aprendizajes clave separados por comas |
| `--failure_root_cause` | No | Causa raíz (obligatorio si status = failure) |
| `--tags` | No | Tags para indexación, separados por comas |

---

## Usar el Workflow de Revisión Final

El workflow `higpertext-review` automatiza el cierre de sesión con persistencia en memoria:

```bash
htx workflow run higpertext-review \
    --notes "Se completó la integración del adaptador OpenCode. Aprendizaje clave: los schemas de OpenCode requieren campo 'model' explícito."
```

Este workflow ejecuta en secuencia:
1. `higpertext-tester` — verifica integridad del motor
2. `memory-manager` — persiste las notas como aprendizaje
3. `session-clean` — desmonta la sesión temporal

---

## Consultar la Memoria

```bash
# Ver el resumen legible de las últimas acciones
htx task common.smart-read --path .memory/context.md

# Buscar en la base de conocimiento (incluye memoria + gobernanza)
htx ask "qué aprendimos sobre deployments fallidos"
```

---

## Datos que NO se Guardan

Para mantener la memoria compacta y útil:

- No guardar contenido completo de archivos (usar rutas)
- No guardar outputs completos de herramientas (usar resúmenes)
- No guardar datos sensibles: tokens, contraseñas, PATs

---

[Volver al Índice](../README.md)
<!-- higpertext:generated-by=common.docs-sync -->
