# Capability: `common.roadmap-report`

**Nombre**: Roadmap Report
**Versión**: 1.0.0

## Propósito
Genera un reporte explicativo del roadmap activo: progreso por fase, skills y subagents usados, y timeline HTML visual. Se puede invocar al completar cada fase via /plan.

**Entrypoint**: `capabilities/common/scripts/core/reports/roadmap_report.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--roadmap` | No | Path al archivo roadmap JSON. Default: roadmap activo en .higpertext/config/roadmaps/. |
| `--output` | No | Ruta de salida del reporte. Default: .higpertext/reports/roadmap/<roadmap-id>_report.<ext>. |
| `--commit-range` | No | Rango Git exacto a documentar, por ejemplo `HEAD~5..HEAD`. |
| `--verify` | No | Ejecuta el arnés de evaluación estático y de hooks e incorpora su resultado al reporte. |
| `--agent-validation-summary` | No | Resumen corto de la validación del agente que cierra la tarea. Se persiste en el roadmap JSON (acumulativo). |
| `--agent-validation-file` | No | Path a un archivo con el texto de validación, para resúmenes largos. Alternativa a `--agent-validation-summary`. |
| `--agent-validation-verdict` | No | `PASS` \| `CONCERNS` \| `FAIL`. Default: `PASS`. |
| `--agent-name` | No | Identifica al agente que registra la validación. Default: `Claude Sonnet 5`. |

## Contrato Técnico (Reglas)

- Debe leer el roadmap indicado o el roadmap activo en .higpertext/config/roadmaps/.
- Debe mostrar resumen ejecutivo en lenguaje natural con % de completitud.
- Debe mostrar todas las fases con su status (done/active/pending) e iconos visuales.
- Debe listar skills y subagents por fase y totales.
- Debe generar HTML con barra de progreso y timeline coloreado por status.
- La validación del agente se persiste en el roadmap JSON fuente, no solo en el markdown generado — se acumula entre corridas, nunca se sobreescribe.
- El cierre efectivo de una fase (status=done) NO ocurre aquí — es responsabilidad exclusiva de `common.roadmap-phase-close`, gateada por el reviewer.
- Debe completar con [SUCCESS] o [ERROR] explícito.
<!-- higpertext:generated-by=common.docs-sync -->
