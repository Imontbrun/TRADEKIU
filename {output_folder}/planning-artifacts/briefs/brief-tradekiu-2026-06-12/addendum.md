# Addendum — TRADEKIU Brief

Material de profundidad que no cabe en el brief pero es insumo directo para el PRD y la arquitectura.

---

## Check-in Pre-Sesión — Campos Confirmados

11 campos, todos botones/sliders. Objetivo: completar en menos de 3 minutos.

### 🛏️ Sueño
| Campo | Tipo | Opciones |
|---|---|---|
| Horas de sueño | Botones | 4h / 5h / 6h / 7h / 8h / 9h+ |
| Calidad del sueño | Slider | 1 — 100 |

### 💪 Cuerpo
| Campo | Tipo | Opciones |
|---|---|---|
| ¿Hiciste workout? | Botón | Sí / No |
| ¿Desayunaste? | Botón | Sí / No |
| ¿Tomaste café? | Botón | Sí / No |

### 🧠 Mente
| Campo | Tipo | Opciones |
|---|---|---|
| Estado de ánimo | Slider | 1 — 5 |
| Nivel de estrés | Slider | 1 — 5 |
| ¿Te sientes enfocado? | Botón | Sí / No / Más o menos |

### 📍 Entorno
| Campo | Tipo | Opciones |
|---|---|---|
| ¿Desde dónde tradeas? | Botón | Casa / Otro lugar |
| Dispositivo | Botón | Computadora / Teléfono / Tablet |

### 📅 Preparación
| Campo | Tipo | Opciones |
|---|---|---|
| ¿Revisaste el calendario económico? | Botón | Sí / No |

---

## Notas de diseño del check-in
- El orden de las categorías debe seguir el flujo natural de la mañana: primero lo que pasó mientras dormías, luego el cuerpo, luego la mente, luego el entorno.
- El check-in se completa **antes** de abrir los gráficos — esto es crítico para eliminar el sesgo de resultado.
- En mobile (responsive web) los botones deben ser grandes y touch-friendly.

---

## Audio Post-Sesión
- Grabación libre de voz al cerrar la sesión del día.
- También se graba si NO se tomaron trades (¿por qué no?).
- El audio **no se archiva para escuchar** — se transcribe con IA y se extraen los datos relevantes del día.
- Los extractos alimentan el reporte semanal de psicología.

---

## Referencia Visual Confirmada
- Calendario mensual: diseño tipo P&L calendar (verde/rojo), mostrando P&L del día + número de trades por cuadro.
- Al hacer clic en un día: detalle de trades, check-in de esa mañana, extracto del audio.
