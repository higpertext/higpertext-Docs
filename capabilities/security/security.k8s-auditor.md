# Capability: `security.k8s-auditor`

**Nombre**: Auditor de Manifiestos Kubernetes (K8s Auditor)
**Versión**: 1.0.0

## Propósito
Audita archivos YAML de Kubernetes buscando fallos de seguridad o malas prácticas de configuración.

**Entrypoint**: `capabilities/security/scripts/k8s_auditor.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--target` | No | Directorio o archivo YAML a auditar. |
| `--save_report` | No | Nombre del archivo de reporte Markdown resultante. |

## Contrato Técnico (Reglas)

- Debe buscar archivos .yaml o .yml recursivamente.
- Debe evaluar el contexto de seguridad (privileged, runAsNonRoot, readOnlyRootFilesystem, allowPrivilegeEscalation).
- Debe reportar riesgos de red (hostNetwork, hostPort) e inyecciones de volumen.
- Debe clasificar los hallazgos por severidad: CRITICAL, HIGH, MEDIUM, LOW.
<!-- higpertext:generated-by=common.docs-sync -->
