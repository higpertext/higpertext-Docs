# Capability: `common.docs-sync`

**Nombre**: common.docs-sync
**Versión**: 1.0.0

## Propósito
Analiza capacidades y perfiles JSON, y actualiza únicamente los documentos Markdown que el equipo haya marcado como gestionados por esta capability. Respeta cualquier estructura de documentación existente y no sobrescribe documentos antiguos o personalizados.

**Entrypoint**: `capabilities/common/scripts/core/docs_sync.py`
**Lenguaje**: `python`

## Contrato Técnico (Reglas)

- Detectar documentos gestionados por el marcador `<!-- higpertext:generated-by=common.docs-sync -->`.
- Actualizar todos los documentos existentes donde el equipo haya colocado ese marcador, aunque estén en carpetas distintas.
- Omitir archivos sin marcador y conservar intacta la documentación antigua o personalizada.
- No depender de numeración, nombres de carpetas, división interna/externa ni imponer rutas.
