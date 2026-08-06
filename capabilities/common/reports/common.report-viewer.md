# Capability: `common.report-viewer`

**Nombre**: Catálogo de Reportes
**Versión**: 1.0.0

## Propósito
Muestra en terminal los reportes persistidos e indexados por higpertext.

**Entrypoint**: `capabilities/common/scripts/core/reports/report_viewer.py`
**Lenguaje**: `python`

## Parámetros

Esta capacidad no requiere parámetros.

## Contrato Técnico (Reglas)

- Debe leer el índice de OutputStore como fuente de verdad.
- Debe listar en terminal únicamente reportes existentes, con su ruta, tamaño y fecha.
- No debe generar HTML ni escribir archivos.
<!-- higpertext:generated-by=common.docs-sync -->
