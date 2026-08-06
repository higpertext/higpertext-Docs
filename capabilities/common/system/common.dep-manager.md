# Capability: `common.dep-manager`

**Nombre**: Dependency Manager
**Versión**: 1.0.0

## Propósito
Gestiona dependencias del proyecto: instala, desinstala y lista paquetes usando el .venv activo. Valida que no se instale en el Python del sistema.

**Entrypoint**: `capabilities/common/scripts/core/system/dep_manager.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--action` | Sí | Acción: install | uninstall | list |
| `--packages` | No | Paquete(s) separados por coma (requerido para install/uninstall) |
| `--dev` | No | Si 'true', instala como dependencia de desarrollo |

## Contrato Técnico (Reglas)

- Siempre usar .venv/bin/pip — nunca el pip del sistema.
- Nunca instalar como root o con sudo.
- Después de instalar, actualizar requirements.txt si existe.

## Intercepción de Bash

- **Patrón**: `\bpip\s+(install|uninstall|freeze)\b`
- **Descripción**: Gestionar dependencias del proyecto
- **Ejemplo de reemplazo**: `htx task common.dep-manager --action install --packages requests`
<!-- higpertext:generated-by=common.docs-sync -->
