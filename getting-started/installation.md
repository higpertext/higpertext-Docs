# Instalación

Setup mínimo para tener `htx` operativo desde cero.

---

## Requisitos

- Python 3.10+
- PowerShell 5.1+ *(solo para capacidades `ado_admin` y `pwsh_engineer`)*
- Azure CLI *(solo para capacidades `sre.*`)*

---

## Instalar la herramienta

```bash
pip install higpertext-cli
```

Verifica que el CLI está disponible:

```bash
htx --help
```

### Autocompletado del CLI

En Bash, instala el autocompletado una sola vez por usuario:

```bash
htx completion install
source ~/.bashrc
```

Después de actualizar `higpertext-cli`, no necesitas repetir la instalación.
Solo actualiza la sesión actual con `source ~/.bashrc` si estaba abierta durante
la actualización. Para verificarlo:

```bash
htx __complete ta
```

El resultado esperado incluye `task`. Para Zsh, Fish o PowerShell, reemplaza
`bash` por el shell correspondiente en `htx completion <shell>`.

---

## Instalación con extras opcionales

```bash
pip install higpertext-cli[gemini]    # soporte Google Gemini
```

Anthropic, OpenAI y Ollama se integran vía variables de entorno — sin dependencias adicionales.

---

## Configurar credenciales

Crea un archivo `.env` en tu proyecto con las variables que necesites:

| Variable | Requerida para |
|----------|---------------|
| `ANTHROPIC_API_KEY` | Asistente Claude |
| `GEMINI_API_KEY` | Asistente Gemini |
| `GITHUB_TOKEN` | Capacidades GitHub |
| `ADO_PAT` | Capacidades `ado_admin.*` |

> El archivo `.env` debe estar en `.gitignore` — nunca commitear tokens.

---

## Siguiente paso

→ [Primer arranque](first-run.md) — inicializar el asistente y cargar un perfil
<!-- higpertext:generated-by=common.docs-sync -->
