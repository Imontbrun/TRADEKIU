# Addendum — PRD TRADEKIU

Profundidad técnica y material que alimenta arquitectura/UX pero no pertenece al cuerpo del PRD.

## Notas técnicas (insumo para `bmad-create-architecture`)

- **Importación v2 — MetaApi:** tier gratuito para 1 cuenta personal MT4/MT5. Es la vía investigada para Vantage live y la mayoría de prop firms (FTMO etc. usan MT4/MT5). El modelo de datos de trades debe ser agnóstico de la fuente (CSV vs API) desde v1 — campo `source` por trade.
- **Pipeline de audio:** grabación (MediaRecorder API en navegador) → transcripción IA → extracción estructurada IA (un solo paso LLM puede hacer transcripción→extracción si el modelo lo soporta, o dos pasos: STT + LLM). Decisión de proveedor/modelo es de arquitectura, no del PRD.
- **CSV TradingView:** el Trade Log del paper trading exporta historial de órdenes/posiciones. Validar columnas reales con un export del usuario en el primer sprint (Pregunta Abierta #2 del PRD).
- **Generación del reporte (FR-20):** si despliegue local → lazy generation al abrir la app después del domingo 18:00; si servidor → cron/scheduled job. Misma lógica, distinto trigger.

## Decisiones de producto con racional (del brief, para no perderlas)

- **Audio en vez de texto post-sesión:** menor fricción y más honestidad emocional. El audio es input de datos, no archivo — el usuario nunca lo re-escucha.
- **No rellenar check-ins retroactivos (FR-3):** protege contra el sesgo de resultado (registrar el estado de ánimo *después* de conocer el P&L contamina el dato). Mismo motivo por el que el check-in va antes de abrir gráficos.
- **Reporte con lenguaje de observación, no causalidad (FR-18):** un solo usuario genera pocos datos; prometer significancia estadística sería deshonesto. El brief fue corregido editorialmente en este mismo sentido.

## Referencia visual

- Calendario mensual tipo P&L calendar (verde/rojo, P&L + nº de trades por día) — validado por el usuario con captura de ejemplo durante el brief.

## Check-in — especificación completa de los 11 campos

La especificación campo a campo (tipos, opciones, agrupación por categorías Sueño/Cuerpo/Mente/Entorno/Preparación y notas de diseño mobile) vive en el addendum del brief: `briefs/brief-tradekiu-2026-06-12/addendum.md`. El PRD la incorpora por referencia en FR-1.
