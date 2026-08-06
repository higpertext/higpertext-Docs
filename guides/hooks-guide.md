<!-- higpertext:generated-by=common.docs-sync -->
# Guía de Hooks — Para el Agente y el Usuario

Los hooks de higpertext actúan como un **guardia inteligente** entre el agente IA
y el sistema. Cuando el agente intenta usar un comando bash que tiene una
capacidad higpertext equivalente, el hook lo intercepta y le indica el comando correcto.

---

## ¿Por qué existen los hooks?

El objetivo es que **todo pase por higpertext** en lugar de bash directo:

| Sin hooks | Con hooks |
|---|---|
| `grep pattern ./src` | `htx task common.grep-search --pattern ...` |
| `git status` | `htx task git.diff` |
| `git ls-files` | `htx task git.ls-files` |

**Ventaja**: el output de cada capacidad está formateado y controlado,
lo que permite mejorar la respuesta que recibe el agente sin tocar el código del modelo.

---

## ¿Cómo funciona para el agente?

1. El agente intenta ejecutar `git status`
2. El hook intercepta antes de ejecutar
3. El agente recibe feedback con el comando correcto
4. El agente **no pierde el hilo de la conversación** — el turno continúa
5. El agente re-ejecuta con `htx task git.diff`

---

## Comandos interceptados actualmente

| Si intentas... | Usa en su lugar... |
|---|---|
| `wc  -l|find  .*.(py|ts|js|cs)` | `htx task common.code-skeletonizer --path src/my_module.py` |
| `pip  (install|uninstall|freeze)` | `htx task common.dep-manager --action install --packages requests` |
| `grep` | `htx task common.grep-search --pattern "<patrón>" --path <ruta>` |
| `cat  .*.(md|json|yaml|yml|txt)` | `htx task common.knowledge-asker --query "<pregunta>"` |
| `git  commit` | `htx task git.committer --message "<mensaje del commit>"` |
| `git  (diff|status|log)` | `htx task git.git-diff --detail true` |
| `(^|[;&|]s*)ls(s|$)|git  ls-files` | `htx task git.ls-files --path src --mode summary` |
| `git  rm` | `htx task git.git-rm --files "<archivo1,archivo2>"` |

---

## Comandos que NUNCA se interceptan

Estos comandos git los ejecuta el **usuario**, no el agente:

```bash
git push          # solo el usuario hace push al remoto
git checkout      # cambio de rama — decisión del usuario
git merge         # merges — requieren revisión humana
git add           # staging — parte del flujo del committer
```

---

## ¿Cómo se activan los hooks?

Los hooks se despliegan automáticamente al cargar un perfil:

```bash
htx profile load software_developer --assistant claude
```

Esto copia los scripts de hook a `.claude/hooks/` y actualiza `.claude/settings.json`.

---

## ¿Qué pasa si el hook bloquea algo por error?

Si un comando legítimo es interceptado y no tiene capacidad equivalente,
puedes agregarlo a la whitelist en `higpertext_enforcer.py` o crear una nueva
capacidad con su `bash_intercept`. Ver la [Referencia Técnica de Hooks](../reference/hooks-reference.md).

---

[Volver al Índice](../README.md)