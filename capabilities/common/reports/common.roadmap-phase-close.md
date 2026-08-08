# Capability: `common.roadmap-phase-close`

Es el único camino gobernado para marcar una fase de roadmap como terminada. Requiere un reviewer independiente configurado mediante `HTX_ROADMAP_REVIEWER`; solo cierra la fase si devuelve exactamente `PASS`.

```bash
htx task common.roadmap-phase-close \
  --roadmap .higpertext/config/roadmaps/mi-roadmap.json \
  --phase-id implementacion
```

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--roadmap` | Sí | Ruta al roadmap JSON |
| `--phase-id` | Sí | ID de la fase |

El intento de validación queda registrado incluso si el reviewer devuelve preocupaciones o error.
