# Instalación

Setup mínimo para tener `htx` operativo desde cero.

---

## Requisitos

- Python 3.10+

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
## Siguiente paso

→ [Primer arranque](first-run.md) — inicializar el asistente y cargar un perfil
<!-- higpertext:generated-by=common.docs-sync -->
