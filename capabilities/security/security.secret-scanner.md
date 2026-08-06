# Capability: `security.secret-scanner`

**Nombre**: Escáner de Secretos y Credenciales (Secret Scanner)
**Versión**: 1.0.0

## Propósito
Escanea el código fuente e historial en busca de claves API expuestas, PATs, tokens y contraseñas.

**Entrypoint**: `capabilities/security/scripts/secret_scanner.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--target_path` | No | Directorio o archivo a escanear en busca de secretos. |
| `--output_report` | No | Nombre del archivo de reporte Markdown resultante. |

## Contrato Técnico (Reglas)

- Debe buscar claves API, contraseñas, PATs de Azure DevOps, tokens de AWS y claves privadas.
- Debe enmascarar los secretos encontrados en el reporte de salida para evitar filtraciones secundarias.
- Debe clasificar los hallazgos según la criticidad del secreto.
<!-- higpertext:generated-by=common.docs-sync -->
