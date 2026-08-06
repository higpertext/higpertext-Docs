<!-- higpertext:generated-by=common.docs-sync -->
# Catálogo de Perfiles

Los perfiles definen el rol, system prompt y capacidades disponibles para cada agente.
Se cargan desde `src/config/profiles/*.json`.

## Resumen

| Perfil | Rol | Caps | Gobernanza | Skills sesión | Subagentes sesión |
|---|---|---|---|---|---|

---

## Cómo agregar un nuevo perfil

1. Crea `src/config/profiles/<nombre>.json`:
   ```json
   {
     "name": "mi-perfil",
     "description": "Descripción del rol.",
     "system_prompt": "Instrucciones base del agente.",
     "capabilities": ["common.memory-manager"],
     "governance_access": false,
     "session_skills": [],
     "session_subagents": []
   }
   ```
2. Ejecuta `htx task common.docs-sync` para regenerar este catálogo.
3. Verifica con `htx profile load mi-perfil --assistant claude`.

---

[Volver al Índice](../README.md)