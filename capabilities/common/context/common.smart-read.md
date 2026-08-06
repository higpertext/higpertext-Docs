# Capability: `common.smart-read`

**Nombre**: Smart Read
**Versión**: 1.0.0

## Propósito
Lee archivos de forma segura para LLM. En modo auto evita volcar archivos grandes y devuelve skeleton, rangos, símbolos o resumen con mapa de líneas.

**Entrypoint**: `capabilities/common/scripts/core/context/smart_read.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--path` | Sí | Archivo a leer. |
| `--mode` | No | auto, skeleton, range, symbol, full o summary. Default: auto. |
| `--symbol` | No | Símbolo a localizar con mode=symbol. |
| `--offset` | No | Línea inicial para mode=range. Default: 1. |
| `--limit` | No | Cantidad de líneas para mode=range o radio para around_line. Default: 120. |
| `--around_line` | No | Lee alrededor de una línea específica. |
| `--max_bytes` | No | Umbral para considerar archivo grande. Default: 102400. |
| `--max_tokens` | No | Si el resultado excede el presupuesto, degrada a summary. |
| `--json` | No | Emite JSON estructurado. |

## Contrato Técnico (Reglas)

- Debe validar que el archivo exista.
- En mode=auto debe devolver skeleton para archivos de código grandes.
- Debe soportar lectura por rango, por símbolo, resumen y JSON.
- Debe bloquear mode=full para archivos que superen max_bytes.
- Debe incluir números de línea en rangos para facilitar lecturas focalizadas.
<!-- higpertext:generated-by=common.docs-sync -->
