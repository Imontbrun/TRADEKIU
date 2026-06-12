---
title: "Product Brief: TRADEKIU"
status: ready-for-prd
created: 2026-06-12
updated: 2026-06-12
---

# Product Brief: TRADEKIU

## Executive Summary

TRADEKIU es una aplicación personal de trading journal con inteligencia artificial, construida para un único trader: su creador. Combina el registro de operaciones (importación por CSV en v1, automática en v2) con el análisis de contexto de vida diario —sueño, ejercicio, entorno, estado de ánimo— para descubrir cómo los hábitos fuera del mercado afectan los resultados dentro de él.

A diferencia de las plataformas genéricas de suscripción, TRADEKIU es un sistema construido a medida que crece con el trader. Su corazón es una capa de inteligencia artificial que procesa notas de audio post-sesión, extrae los datos relevantes del día, y genera cada semana —el domingo, antes del open del lunes— un reporte de psicología de trading con hallazgos, tendencias y malos hábitos identificados. El objetivo no es guardar un diario: es construir, con datos reales propios, la metodología personal de un trader que recién empieza su camino.

## El Problema

Un trader que está comenzando enfrenta tres problemas simultáneos:

**1. El journaling es valioso pero casi nadie lo sostiene.** En las comunidades de trading se repite la misma idea: llevar un journal sistemático ayuda a mejorar más rápido. Pero en la práctica casi todos lo abandonan, porque es tedioso, consume tiempo, y no devuelve retroalimentación clara.

**2. Las herramientas existentes son genéricas y caras.** Apps como Tradezilla o TraderSync cuestan entre $25 y $50/mes y están diseñadas para miles de usuarios distintos. No se adaptan al estilo de vida particular ni a la metodología personal de un trader individual.

**3. La metodología personal no existe aún — hay que descubrirla.** Un trader nuevo no sabe todavía cuáles son sus patrones, sus fortalezas, sus peores hábitos. No puede "implementar su metodología" porque aún no la conoce. Solo los datos propios —acumulados con disciplina y analizados con rigor— pueden revelarla.

El costo del status quo: seguir tradeando sin reflexión estructurada, perdiendo dinero real (o tiempo en paper trading) sin extraer aprendizaje del proceso.

## La Solución

TRADEKIU resuelve el problema en tres capas integradas:

### Capa 1 — Registro diario de contexto (inputs)
Antes de abrir los gráficos, el trader completa en menos de 5 minutos un check-in rápido con botones y sliders — sin texto libre:

- **Sueño:** horas dormidas + calidad (1-100)
- **Cuerpo:** ¿workout? / ¿desayunó? / ¿tomó café?
- **Mente:** estado de ánimo (1-5) / nivel de estrés (1-5) / ¿se siente enfocado?
- **Entorno:** lugar (casa / otro) + dispositivo (computadora / teléfono / tablet)
- **Preparación:** ¿revisó el calendario económico?

Al cerrar la sesión de trading —o al final del día si no se tomaron trades— graba una nota de audio libre: qué pasó, cómo se sintió en las operaciones, por qué no tomó trades si fue el caso. El audio no se archiva para escuchar después: la IA lo transcribe y extrae los datos relevantes del día.

Los trades se importan vía CSV (v1) o API automatizada con MetaApi/MT4/MT5 (v2), evitando teclear cada operación a mano.

### Capa 2 — Dashboard de performance
Una vista limpia con los indicadores que importan:
- P&L por sesión y acumulado
- Ratio riesgo/beneficio (R:R) y su promedio móvil
- Win rate
- Número de trades por sesión
- **Vista de calendario mensual**: cada día del mes muestra el resultado de esa sesión de un vistazo — verde/rojo, magnitud de la ganancia o pérdida. Un mes entero en una pantalla.

### Capa 3 — Reporte semanal de psicología (el diferenciador)
Cada domingo, antes del open del lunes, TRADEKIU genera automáticamente un reporte de psicología de trading con:
- **Hallazgos de la semana**: correlaciones detectadas entre hábitos de vida y resultados (ej. "tus 3 mejores sesiones fueron días en que dormiste +7h y entrenaste")
- **Tendencias**: qué está mejorando, qué está empeorando, qué se repite semana a semana
- **Malos hábitos identificados**: patrones negativos que se repiten (ej. "tradeaste 4 veces desde el teléfono esta semana — tu R:R en esas sesiones fue 0.4 vs 1.8 desde computadora")

Este reporte se construye combinando los datos del dashboard, los extractos de los audios analizados, y el historial de contexto diario.

## Lo Que Hace a TRADEKIU Diferente

**Es 100% personal.** No hay concesiones para miles de usuarios distintos. Cada campo, cada métrica, cada pregunta del check-in está calibrada para una sola persona y su forma de vivir y tradear.

**El audio es un input de datos, no un archivo.** Hablar es más honesto y rápido que escribir, y el trader nunca tiene que volver a escuchar sus grabaciones para obtener valor — la IA convierte esa honestidad en datos estructurados.

**Construye metodología desde cero.** TRADEKIU no asume que el trader ya sabe cómo opera. Lo ayuda a descubrirlo. Con suficientes semanas de datos, el reporte deja de ser un resumen y empieza a funcionar como un espejo: "esto es quién eres como trader, con tus números reales."

**Sin costo de licencia.** Construido para uso personal con herramientas open-source y APIs con tier gratuito (MetaApi free para 1 cuenta). El único costo variable es el uso de IA para transcripción y análisis.

## A Quién Sirve

**Usuario único: el creador de la app.**

Un trader en las etapas tempranas de su carrera. Actualmente en paper trading (TradingView), con live account en Vantage (MT4/MT5), con meta de conseguir una cuenta fondeada en el tercer mes. Opera en los mercados de forex/CFD.

No tiene experiencia previa con journaling de trading. Entiende que el aspecto psicológico es determinante para la rentabilidad a largo plazo, y quiere construir el hábito y la metodología desde el comienzo, antes de que las pérdidas reales sean significativas.

Su definición de éxito: después de 30-60 días de uso, que TRADEKIU le revele patrones sobre sí mismo que no habría descubierto de otra manera.

## Criterios de Éxito

1. **Uso sostenido:** el check-in diario se completa en menos de 5 minutos y se mantiene como hábito durante el mes de paper trading.
2. **Primer insight real:** en las primeras 2-3 semanas, el reporte semanal identifica al menos una correlación verificable entre un hábito de vida y el rendimiento en los gráficos.
3. **Claridad de patrones:** al finalizar el mes 1, el usuario puede nombrar 2-3 condiciones bajo las cuales tradea mejor y 2-3 hábitos que correlacionan negativamente con su rendimiento.
4. **Adopción en live:** el sistema sigue en uso cuando se hace la transición de paper a live account, sin fricción de migración.
5. **Utilidad del reporte dominical:** el reporte semanal de psicología se convierte en parte del ritual de preparación del lunes.

## Alcance

### v1 — Paper trading month (Mes 1)
**Dentro:**
- Check-in pre-sesión con botones configurables
- Grabación de audio post-sesión + transcripción + extracción de datos con IA
- Importación de trades vía CSV (TradingView Trade Log export)
- Dashboard de performance: P&L, R:R, win rate, número de trades
- Vista de calendario mensual con resultados por día
- Reporte semanal de psicología generado automáticamente (domingo)
- Una sola cuenta/usuario — sin sistema de autenticación complejo
- Web app (desktop-first, dado que la mayoría de sesiones son desde computadora)

**Fuera:**
- Importación automática por API (v2)
- App móvil nativa (el check-in desde teléfono puede ser web responsive)
- Análisis conversacional ("pregúntale a la app")
- Soporte para múltiples usuarios o cuentas
- Integración con prop firms

### v2 — Live account (Mes 2+)
- Integración MetaApi para importación automática desde Vantage (MT4/MT5)
- Soporte para cuenta fondeada (MT4/MT5 vía MetaApi)
- Refinamiento del modelo de análisis de IA basado en 30+ días de data real

## Visión

En 6 meses, TRADEKIU es el centro de la rutina del trader: la primera cosa que abre en el día (check-in) y la última que revisa el domingo (reporte). Ha acumulado suficiente historia para que los patrones empiecen a ser confiables — y el trader puede tomar decisiones sobre su rutina de vida con la misma rigurosidad con la que analiza sus entradas al mercado.

En 2 años, si los resultados son positivos y el sistema ha probado su valor, existe la posibilidad de abrirlo a otros traders — pero solo si la propuesta de valor personal está completamente validada primero. El producto público, si llega a existir, sería la versión destilada de lo que funcionó para un trader real durante dos años de uso honesto.
