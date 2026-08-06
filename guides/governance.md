# Lineamientos de Gobernanza

Reglas de cumplimiento obligatorio para todo desarrollo en proyectos con higpertext Engine. Fuente canónica: `src/config/governance/guidelines_contract.json`.

---

## Pull Requests

| Regla | Descripción |
|---|---|
| Revisión requerida | Todo PR necesita al menos una revisión aprobada antes del merge |
| Formato de título | `type(scope): description` — Conventional Commits |
| Cobertura de código | No se puede bajar la cobertura por debajo del umbral del proyecto |
| Estrategia de merge | Squash merge en `main`; rebase merge en ramas feature |
| PRs con secretos o infra | Requieren segundo revisor del equipo de seguridad |
| Draft PRs | No se pueden mergear hasta marcarlos explícitamente como listos |

---

## Seguridad

| Regla | Descripción |
|---|---|
| Sin secretos en código | Nunca hardcodear tokens, PATs o contraseñas en código ni configs |
| Referencia a secretos | Siempre via variables de entorno o secrets manager |
| Secretos expuestos | Rotar inmediatamente y registrar el incidente en postmortem |
| Imágenes de contenedor | Escanear CVEs antes de deploy |
| Principio de mínimo privilegio | Aplicar a todas las cuentas de servicio e IAM roles |
| Expiración de SSH/PATs | Máximo 90 días |

---

## Deployments

| Regla | Descripción |
|---|---|
| Pipeline verde | No hay deploy a producción sin CI pasando en el commit destino |
| Aprobación humana | Producción requiere aprobación explícita via deployment gate |
| Rollback documentado | Procedimiento de rollback debe estar probado antes de cada release |
| Estrategia para SLA > 99.5% | Blue/green o canary es obligatorio |
| Ventana de producción | Martes–Jueves 10:00–16:00 UTC únicamente |
| Audit log | Todo deploy debe emitir un evento al audit log |

---

## Calidad de Código

| Regla | Umbral | Descripción |
|---|---|---|
| Longitud de funciones | ≤ 30 líneas | Funciones deben ser atómicas |
| Longitud de clases | ≤ 200 líneas | Clases con responsabilidad única |
| Complejidad ciclomática | < 10 por función | Medir con `radon` |
| Firmas tipadas | 100% en APIs públicas | Type hints obligatorios |
| Código comentado | 0 en ramas mergeadas | Limpiar antes del PR |
| Cobertura de tests | ≥ 80% en módulos nuevos | Validar con `code-coverage` |

---

## Doc-as-Code

| Regla | Descripción |
|---|---|
| Versionado en Markdown | Documentación en VCS junto al código fuente |
| Actualización obligatoria | Cambios a APIs públicas deben incluir docs en el mismo commit o PR |
| DRY documentation | Modular, sin duplicaciones de explicaciones |
| Lint de Markdown | Seguir reglas estándar de Markdown lint |
| Links relativos | Verificados para evitar rutas rotas |

---

## Gitflow — Commits y Ramas

| Regla | Descripción |
|---|---|
| Conventional Commits | `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert` |
| Commits atómicos | Cada commit = un cambio lógico con tests pasando |
| Ramas feature | Branching desde `develop`, merge via PR — nunca directo |
| Ramas release | Desde `develop`, merge a `main` Y `develop` |
| Hotfixes | Desde `main`, merge a `main` Y `develop` inmediatamente |
| Rebase previo al merge | Siempre rebase contra `develop` antes de mergear para historial limpio |

---

## Consultar Gobernanza desde la CLI

```bash
# Pregunta libre a la base de conocimiento de gobernanza
htx ask "¿cuáles son las reglas de aprobación de PRs?"
htx ask "ventanas de deploy a producción"
htx ask "qué hacer si se expone un secreto"
```

El comando `ask` usa `common.knowledge-asker` para buscar en `guidelines_contract.json` y en los archivos Markdown de gobernanza del proyecto.

---

## Sincronizar Lineamientos Personalizados

Si tu equipo mantiene lineamientos propios en un repositorio Git:

```bash
# Sincronizar desde URL git
htx task guidelines-sync --source https://github.com/mi-org/governance.git

# Sincronizar desde path local
htx task guidelines-sync --source ./mis-lineamientos/

# Usando el workflow automatizado (con persistencia en memoria)
htx workflow run guidelines-sync --source https://github.com/mi-org/governance.git
```

---

[Volver al Índice](../README.md)
<!-- higpertext:generated-by=common.docs-sync -->
