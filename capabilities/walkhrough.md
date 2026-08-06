# Guía de Uso: Fuente de Verdad (`common.truth-keeper`)

La capacidad `common.truth-keeper` ya está implementada y registrada globalmente en el motor. Permite guardar, leer y gestionar contextos específicos a nivel de dominio en `.memory/truth.json` para accesibilidad ultra rápida por parte de otros agentes o perfiles.

---

## 1. Guardar contexto (`set`)

Para definir o actualizar información en un dominio específico, usa:

```bash
htx task common.truth-keeper \
  --action set \
  --domain <nombre_dominio> \
  --key <nombre_llave> \
  --value <valor_o_json> \
  --description "Descripción opcional del dominio"
```

### Ejemplo
```bash
htx task common.truth-keeper \
  --action set \
  --domain "deployments" \
  --key "production_url" \
  --value "https://prod.my-app.com" \
  --description "Direcciones y credenciales de entornos activos"
```

---

## 2. Consultar contexto (`get`)

Puedes consultar un valor específico, todo un dominio, o la Fuente de Verdad completa.

### Obtener un valor específico:
```bash
htx task common.truth-keeper --action get --domain "deployments" --key "production_url"
```

### Obtener todo el dominio:
```bash
htx task common.truth-keeper --action get --domain "deployments"
```

### Obtener la Fuente de Verdad completa:
```bash
htx task common.truth-keeper --action get
```

---

## 3. Listar dominios registrados (`list`)

Para ver un mapa resumido de todos los dominios activos, su descripción, última fecha de modificación y sus llaves correspondientes (sin volcar todos los valores):

```bash
htx task common.truth-keeper --action list
```

---

## 4. Eliminar información (`delete`)

### Eliminar una llave específica de un dominio:
```bash
htx task common.truth-keeper --action delete --domain "deployments" --key "production_url"
```

### Eliminar un dominio completo:
```bash
htx task common.truth-keeper --action delete --domain "deployments"
```
<!-- higpertext:generated-by=common.docs-sync -->
