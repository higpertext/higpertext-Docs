# Capability: `common.investigation-report`

Guarda una investigación o justificación técnica como Markdown en `.higpertext/reports/<category>/`.

```bash
htx task common.investigation-report \
  --title "Decisión de autenticación" \
  --content "# Contexto\n\nDecisión y evidencia."
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--title` | Sí | Título del informe |
| `--category` | No | Subcarpeta; predeterminada: `investigation` |
| `--slug` | No | Identificador del reporte |
| `--content` | Condicional | Contenido Markdown inline |
| `--content_file` | Condicional | Archivo Markdown alternativo |

Indica exactamente uno de `--content` o `--content_file`.
