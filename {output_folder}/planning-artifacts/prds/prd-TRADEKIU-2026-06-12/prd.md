---
title: "PRD: TRADEKIU"
status: draft
created: 2026-06-12
updated: 2026-06-12
---

# PRD: TRADEKIU

> **Fuente:** Brief `briefs/brief-tradekiu-2026-06-12/` (status: ready-for-prd).
> **Usuario único:** el creador. Proyecto personal — rigor calibrado a ese nivel.
> Las etiquetas `[SUPUESTO]` marcan decisiones inferidas por el PM pendientes de validación del usuario.

## 1. Visión y Contexto

TRADEKIU es una aplicación web personal de trading journal con IA para un único trader. Captura el contexto de vida diario (check-in con botones), los trades (importación CSV en v1), y la voz del trader (audio post-sesión transcrito y analizado por IA), y devuelve dos cosas: un dashboard de performance con calendario mensual de P&L, y un reporte semanal de psicología de trading disponible el lunes antes del market open, con hallazgos, tendencias y malos hábitos.

El objetivo de fondo: ayudar a un trader que nunca ha journaleado a construir el hábito y descubrir su propia metodología con datos reales, sin pagar suscripciones.

**Plan de adopción (define el roadmap):**
| Mes | Cuenta | Importación |
|---|---|---|
| 1 | Paper trading (TradingView) | CSV manual |
| 2 | Live (Vantage, MT4/MT5) | MetaApi automática (v2) |
| 3 | Cuenta fondeada (prop firm) | MetaApi o CSV según plataforma |

**Contexto operativo (confirmado por el usuario):** zona horaria America/Caracas (UTC-4). La sesión arranca con el NY open (9:30 AM ET). La semana de trading es lunes–viernes; los fines de semana no se opera.

## 2. El Ritual Diario (journey de referencia)

Todo el diseño sirve a este ciclo. Si una feature no aparece en este ritual, probablemente sobra.

1. **Mañana, antes de abrir los gráficos** — Imontbrun abre TRADEKIU (computadora o teléfono) y completa el check-in: 11 campos, solo botones y sliders, <3 minutos. La app deja claro que el check-in va *antes* de ver el mercado.
2. **Sesión de trading** — fuera de TRADEKIU (TradingView/MT4-5).
3. **Cierre de sesión** — graba un audio libre: qué pasó, qué sintió, por qué entró o por qué no tomó trades. Un toque para grabar, listo. La IA transcribe y extrae los datos del día; el audio no se vuelve a escuchar.
4. **Después del cierre** — exporta el Trade Log de TradingView y lo sube a TRADEKIU (CSV). El dashboard y el calendario se actualizan.
5. **Lunes en la mañana, antes del NY open** — abre el reporte semanal de la semana anterior: hallazgos, tendencias, malos hábitos. Lo lee como parte de su preparación, hace su check-in, y arranca la semana.

## 3. Funcionalidades y Requisitos

### F1 — Check-in pre-sesión

- **FR-1** El usuario puede completar un check-in diario con los 11 campos confirmados en el brief: horas de sueño (botones 4h–9h+), calidad del sueño (slider 1–100), workout (sí/no), desayuno (sí/no), café (sí/no), estado de ánimo (slider 1–5), estrés (slider 1–5), enfoque (sí/no/más o menos), lugar (casa/otro), dispositivo (computadora/teléfono/tablet), calendario económico revisado (sí/no).
- **FR-2** El check-in registra automáticamente fecha y hora de completado — la hora sirve como dato de análisis ("tradear tarde/temprano") sin pedir un campo extra.
- **FR-3** Solo existe un check-in por día; puede editarse hasta el fin del día. No se permite llenar check-ins de días pasados — un día sin check-in queda como "sin contexto" para proteger la honestidad del dato. *(Confirmado por el usuario.)*
- **FR-4** El check-in es usable desde el teléfono (web responsive, botones touch-friendly), porque la mañana no siempre ocurre frente a la computadora.
- **FR-21** Recordatorio y advertencia: los días de semana (lun–vie), si el check-in del día no está completo a las 9:00 AM (hora de Caracas), el sistema envía un recordatorio; además, la app muestra siempre una advertencia visible mientras falte el check-in del día. El canal del recordatorio (notificación del navegador, correo u otro) se decide en arquitectura — el requisito es que llegue sin tener la app abierta.

### F2 — Registro post-sesión (audio → datos)

- **FR-5** El usuario puede grabar una nota de audio al cierre de su sesión, en un toque, desde computadora o teléfono. Duración máxima 5 minutos; puede regrabarse antes de enviar a procesamiento. *(Confirmado.)*
- **FR-6** Hay registro post-sesión también en días sin trades — el audio de "por qué no tradeé hoy" es un dato de primera clase.
- **FR-7** El sistema transcribe el audio con IA. `[SUPUESTO]` El audio será en español, con vocabulario de trading en inglés mezclado (stop loss, break even, etc.); la transcripción debe tolerarlo.
- **FR-8** De cada transcripción la IA extrae datos estructurados del día: estado emocional percibido, eventos clave (errores, aciertos, desviaciones del plan), y razones de acción/inacción. El extracto queda asociado al día; la transcripción completa se conserva como respaldo.
- **FR-9** El audio original se elimina automáticamente tras una transcripción exitosa — lo importante es el contenido; la fuente de verdad es la transcripción + el extracto. *(Confirmado: sin opción de conservar audios.)*

### F3 — Importación de trades

- **FR-10** El usuario puede importar sus trades subiendo el CSV exportado del Trade Log de paper trading de TradingView. `[SUPUESTO]` v1 soporta únicamente ese formato; otros formatos llegan con v2.
- **FR-11** La importación es idempotente: re-subir un CSV con trades ya registrados no crea duplicados.
- **FR-12** Cada trade importado queda asociado a su día (y por tanto a su check-in y audio). Campos mínimos: instrumento, dirección, precio de entrada/salida, hora de entrada/salida, tamaño, P&L. El R:R se calcula como aproximado a partir del historial (R realizado desde P&L); si el CSV trae stop/target se usa el R:R planificado real. *(Confirmado: R:R aproximado del historial es aceptable.)*
- **FR-13** v2 (Mes 2+): integración con MetaApi para importación automática desde Vantage (MT4/MT5) y la cuenta fondeada. Queda fuera del v1 pero el modelo de datos debe poder recibir trades de ambas fuentes sin migración.

### F4 — Dashboard de performance

- **FR-14** El dashboard muestra, para el período seleccionado: P&L (por sesión y acumulado), win rate, R:R promedio, número de trades por sesión.
- **FR-15** Calendario mensual de P&L (diseño validado con referencia visual): cada día muestra P&L y número de trades; verde = positivo, rojo = negativo, vacío = sin trades. Sábados y domingos se muestran neutros (no se opera los fines de semana). Navegable entre meses.
- **FR-16** Al hacer clic en un día se abre la **vista de día**: trades de ese día, check-in de esa mañana, y extracto del audio de esa tarde — las tres capas juntas, que es donde el producto cobra sentido.

### F5 — Reporte semanal de psicología

- **FR-17** El sistema genera un reporte semanal que cruza check-ins + extractos de audio + resultados de trades de la semana de trading anterior (lun–vie), con tres secciones: hallazgos (correlaciones observadas), tendencias (qué mejora/empeora semana a semana), y malos hábitos (patrones negativos recurrentes). Disponible **el lunes a las 8:00 AM (hora de Caracas)**, antes del NY open de 9:30 AM ET — el usuario lo lee como parte de su preparación del lunes. *(Confirmado: lunes pre-open, no domingo.)*
- **FR-18** El reporte presenta correlaciones como observaciones, no como causalidad, y con su evidencia ("3 de tus 4 días rojos empezaron con <6h de sueño"), nunca cifras infladas. Con pocos datos (primeras 1–2 semanas) el reporte lo dice honestamente y se limita a describir, no a concluir. `[SUPUESTO]` Umbral mínimo: 3 días con datos completos para emitir correlaciones.
- **FR-19** Los reportes quedan archivados y navegables — la serie de reportes ES la historia de la evolución del trader.
- **FR-20** `[SUPUESTO]` La generación corre automáticamente (job programado) si la app está desplegada en un servidor; si corre localmente, el reporte se genera al abrir la app la mañana del lunes (lazy generation, lista en segundos). En ambos casos el usuario lo encuentra disponible antes de las 8:00 AM de Caracas o al primer acceso del lunes.

## 4. Requisitos No Funcionales

- **NFR-1 — Privacidad:** los datos (audios, transcripciones, estados emocionales) son íntimos. Solo salen del sistema hacia las APIs de IA necesarias para transcripción/análisis. Sin analytics de terceros, sin telemetría.
- **NFR-2 — Costo:** sin suscripciones. Infraestructura en tiers gratuitos o costo cercano a cero; el único costo variable aceptado es el consumo de API de IA. El usuario no fijó un tope — la fase de arquitectura debe estimar el costo mensual real de IA (transcripción diaria + reporte semanal) y presentárselo antes de construir. Guía de referencia: mantenerlo en un dígito de dólares al mes.
- **NFR-3 — Fricción mínima:** check-in completable en <3 minutos; audio iniciable en 1 toque. Si el ritual diario supera los 10 minutos totales, el producto está fallando su criterio de éxito #1.
- **NFR-4 — Durabilidad de los datos:** la historia acumulada es el valor del producto. Exportación/backup completo de todos los datos en un clic. La pérdida de datos es el peor fallo posible.
- **NFR-5 — Single-user:** sin registro ni gestión de usuarios. `[SUPUESTO]` Si se despliega accesible por internet, basta una protección simple (clave única o acceso privado de red).

## 5. Métricas de Éxito

Heredadas del brief, con sus contra-métricas:

| Métrica | Objetivo | Contra-métrica (lo que NO debe pasar) |
|---|---|---|
| Hábito | Check-in diario sostenido durante el mes 1 | Completar el check-in con datos falsos solo por mantener la racha |
| Primer insight | ≥1 correlación verificable en las semanas 2–3 | Reportar correlaciones espurias por forzar hallazgos con pocos datos |
| Claridad | Al fin del mes 1, nombrar 2–3 condiciones donde tradea mejor/peor | — |
| Continuidad | Transición a live (mes 2) sin fricción de migración | — |
| Ritual dominical | El reporte semanal se vuelve parte de la preparación del lunes | Reportes tan largos o genéricos que dejan de leerse |

## 6. Alcance

**v1 (Mes 1 — paper trading):** F1, F2, F3 (solo CSV TradingView), F4, F5. Web app desktop-first responsive.

**Explícitamente fuera de v1:** importación automática por API (v2), app móvil nativa, análisis conversacional ("pregúntale a la app"), multi-usuario/multi-cuenta, integración con prop firms. *(Nota: el recordatorio de check-in de FR-21 es la única "notificación" dentro del v1; no hay más alertas push.)*

**v2 (Mes 2+):** MetaApi (Vantage MT4/MT5 y cuenta fondeada), refinamiento del análisis de IA con 30+ días de data real.

## 7. Preguntas Abiertas

1. ~~Zona horaria / semana de trading~~ — **Resuelta:** Caracas (UTC-4), sesión desde el NY open (9:30 AM ET), semana lun–vie, reporte el lunes 8:00 AM.
2. **Formato real del CSV de TradingView:** hay que validar con un export real del paper trading del usuario qué columnas trae (en particular stop/target para el R:R planificado de FR-12). *(Tarea para arquitectura/primer sprint.)*
3. **Costo mensual de IA:** estimación pendiente en arquitectura; presentar al usuario antes de construir (ver NFR-2).
