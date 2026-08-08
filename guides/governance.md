# Gobernanza

La gobernanza define los límites de calidad, seguridad y entrega que un perfil puede aplicar. El motor trae mecanismos transversales para consultar reglas, cargar instrucciones y solicitar excepciones; la gobernanza específica de un dominio debe vivir en el agente externo que la necesita.

## Capacidades disponibles

```bash
# Consultar conocimiento o lineamientos disponibles
htx task common.knowledge-asker --query "reglas para revisar un cambio"

# Ver reglas y recursos que puede usar el perfil activo
htx task common.list-rules --type all

# Cargar reglas seleccionadas en el asistente integrado
htx task common.load-rules --rules common.grep-search
```

Usa `common.governance-exception` cuando una excepción esté definida y autorizada por tus reglas. No supongas que un workflow de un agente externo existe en el motor base.

## Agentes externos

Un agente externo puede declarar sus propias reglas en `src/config/governance/` y asignar las capabilities mínimas a cada perfil. Esa es la ubicación correcta para políticas de una organización, plataforma o producto; el motor no debe incorporar esa lógica de dominio.
