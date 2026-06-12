# Reconciliación PRD ↔ Brief — TRADEKIU

**Fecha de auditoría:** 2026-06-12
**Documentos comparados:**

| Rol | Archivo |
|---|---|
| PRD | `planning-artifacts/prds/prd-TRADEKIU-2026-06-12/prd.md` |
| Addendum PRD | `planning-artifacts/prds/prd-TRADEKIU-2026-06-12/addendum.md` |
| Brief fuente | `planning-artifacts/briefs/brief-tradekiu-2026-06-12/brief.md` |
| Addendum brief | `planning-artifacts/briefs/brief-tradekiu-2026-06-12/addendum.md` |

**Divergencia conocida (excluida de esta auditoría):** el brief dice "reporte el domingo antes del open del lunes" y el PRD dice "lunes 8:00 AM hora de Caracas". Corrección intencional del usuario durante el PRD — el PRD manda. No se reporta como gap. Esto también cubre la frase de la Visión del brief ("la última cosa que revisa el domingo") y el nombre de la métrica "Ritual dominical", que quedan superseded por la misma decisión.

---

## Veredicto general

La traducción brief → PRD es de alta fidelidad. Los 11 campos del check-in del addendum del brief están completos en FR-1 (opciones y tipos incluidos), las notas de diseño (agrupación por categorías, orden del flujo matinal, mobile touch-friendly) se incorporan por referencia explícita en el addendum del PRD, las decisiones con racional (audio-no-archivo, no-retroactividad, lenguaje de observación) se preservaron deliberadamente, y los 5 criterios de éxito tienen traducción a la tabla de métricas con contra-métricas añadidas (mejora neta sobre el brief). Aun así, se detectaron **5 pérdidas**: 1 de alcance funcional, 2 de detalle de requisito y 2 cualitativas/de contexto.

---

## GAPS DETECTADOS

### GAP 1 — "Botones configurables" del check-in: compromiso de alcance v1 perdido **(el más relevante)**

- **Brief, sección Alcance v1 ("Dentro"):** «Check-in pre-sesión con botones **configurables**».
- **Brief, "Lo Que Hace a TRADEKIU Diferente":** «Es 100% personal… **cada campo, cada métrica, cada pregunta del check-in está calibrada** para una sola persona y su forma de vivir y tradear» — implica que el check-in puede evolucionar con el usuario.
- **PRD:** FR-1 fija exactamente 11 campos cerrados. No existe ningún FR que permita añadir, quitar o modificar campos del check-in, ni en v1 ni en v2. El tema tampoco aparece en "Explícitamente fuera de v1" ni en Preguntas Abiertas.
- **Diagnóstico:** ítem de alcance v1 del brief que desapareció silenciosamente, sin decisión registrada. Puede ser una simplificación razonable (los 11 campos fueron "confirmados"), pero debería ser una decisión explícita: o se agrega un FR de configurabilidad, o se mueve formalmente a "fuera de v1 / v2" con su racional.
- **Acción sugerida:** preguntar al usuario y registrar la decisión en el PRD (FR nuevo o exclusión explícita).

### GAP 2 — R:R "y su promedio móvil" degradado a "R:R promedio"

- **Brief, Capa 2 (Dashboard):** «Ratio riesgo/beneficio (R:R) **y su promedio móvil**».
- **PRD:** FR-14 dice solo «R:R promedio» para el período seleccionado.
- **Diagnóstico:** no es lo mismo. El promedio móvil es una serie temporal (cómo evoluciona el R:R sesión a sesión), no un número agregado del período. La intención del brief —ver la *tendencia* del R:R— se perdió en la compresión.
- **Acción sugerida:** precisar FR-14: «R:R promedio del período **y su evolución como promedio móvil** (ventana a definir en diseño)».

### GAP 3 — Compromiso "open-source" del diferenciador "Sin costo de licencia"

- **Brief:** «Construido para uso personal **con herramientas open-source** y APIs con tier gratuito».
- **PRD:** NFR-2 cubre fielmente el costo (tiers gratuitos, costo ~cero, IA como único variable, estimación previa) pero **omite la preferencia por herramientas open-source** como restricción/guía para arquitectura.
- **Diagnóstico:** pérdida menor pero real — "gratis" y "open-source" no son equivalentes (un tier gratuito propietario cumple NFR-2 pero traiciona el espíritu del brief: evitar dependencia de plataformas cerradas tipo Tradezilla/TraderSync).
- **Acción sugerida:** añadir a NFR-2 una línea: preferencia por stack open-source donde sea viable.

### GAP 4 — Contexto de mercado "forex/CFD" ausente del PRD

- **Brief, "A Quién Sirve":** «Opera en los mercados de **forex/CFD**».
- **PRD:** ni la sección de visión, ni F3 (importación), ni el modelo de campos de FR-12 mencionan el tipo de mercado.
- **Diagnóstico:** no es decorativo — condiciona el modelo de datos de trades (lotes vs acciones, pips, apalancamiento, instrumentos tipo EURUSD/XAUUSD) y el parsing del CSV/futura integración MetaApi. La Pregunta Abierta #2 (formato del CSV) se beneficiaría de este contexto explícito.
- **Acción sugerida:** una línea en §1 (Visión y Contexto) o en FR-12: el dominio es forex/CFD.

### GAP 5 — La idea del "espejo": el reporte debe *evolucionar*, no solo repetirse (cualitativo)

- **Brief, diferenciador "Construye metodología desde cero":** «Con suficientes semanas de datos, **el reporte deja de ser un resumen y empieza a funcionar como un espejo**: "esto es quién eres como trader, con tus números reales"». Reforzado por la definición de éxito del usuario: «que TRADEKIU le revele patrones sobre sí mismo que no habría descubierto de otra manera» (horizonte 30–60 días).
- **PRD:** FR-18 cubre bien el extremo de *pocos* datos (honestidad, describir sin concluir) y FR-19 archiva la serie. El v2 menciona «refinamiento del análisis de IA con 30+ días de data real». Pero **ningún requisito captura la progresión inversa**: que con más datos el reporte profundice (de observaciones semanales → síntesis de identidad/metodología del trader).
- **Diagnóstico:** gap cualitativo. El riesgo es que el reporte se implemente como plantilla estática semanal y nunca llegue al "espejo" que el brief promete como diferenciador central. Esto también conecta con la contra-métrica del propio PRD ("reportes tan genéricos que dejan de leerse").
- **Acción sugerida:** una frase en FR-17 o en v2: el reporte debe escalar su profundidad con el volumen de datos acumulado (síntesis de metodología/perfil a partir de N semanas).

---

## Observaciones menores (no llegan a gap; registrar y seguir)

1. **Criterio de éxito #3 comprimido.** Brief: «2-3 condiciones bajo las cuales tradea **mejor** *y* 2-3 hábitos que correlacionan **negativamente**» (dos listas). PRD: «nombrar 2–3 condiciones donde tradea mejor/peor» (una sola). La intención sobrevive; la doble exigencia se diluyó ligeramente.
2. **"Magnitud" en el calendario.** Brief: el calendario muestra «verde/rojo, **magnitud** de la ganancia o pérdida». FR-15 muestra la cifra de P&L, lo que técnicamente comunica magnitud; si la intención era codificación visual de intensidad (gradiente de color), es decisión de UX a validar con la referencia visual ya confirmada.
3. **"Una vista limpia".** El tono del dashboard del brief (limpieza, "los indicadores que importan") no tiene NFR, aunque NFR-3 (fricción mínima) y el principio de §2 ("si una feature no aparece en este ritual, probablemente sobra") capturan el espíritu razonablemente.
4. **Visión a 2 años (posible producto público).** No está en el PRD; correcto que no genere requisitos, pero conviene que arquitectura la conozca como "no cerrar puertas innecesariamente" (ya parcialmente cubierto por el campo `source` agnóstico del addendum del PRD).

## Cobertura verificada (sin pérdida)

- 11/11 campos del check-in con tipos y opciones exactas (FR-1 + referencia al addendum del brief).
- Notas de diseño del check-in (orden de categorías, antes-de-los-gráficos, mobile) — incorporadas por referencia y reforzadas en §2 y FR-4; el racional anti-sesgo de resultado se preservó en el addendum del PRD y en FR-3.
- Audio post-sesión completo: días sin trades (FR-6), audio-como-input-no-archivo (FR-9 + racional), extractos alimentan el reporte (FR-17).
- Importación CSV TradingView, sin tipeo manual, idempotencia añadida (FR-10/11 — mejora sobre el brief).
- Dashboard: P&L por sesión y acumulado, win rate, nº de trades por sesión (FR-14); calendario P&L con clic-a-día de tres capas (FR-15/16, fiel a la referencia visual confirmada).
- Reporte: tres secciones exactas (hallazgos/tendencias/malos hábitos), lenguaje de observación con evidencia (FR-17/18).
- Plan de adopción 3 meses, MetaApi v2 con tier gratuito, alcance dentro/fuera de v1 — todo trasladado.
- Los 5 criterios de éxito → tabla de métricas con contra-métricas (adición valiosa del PRD).
- Single-user sin auth compleja (NFR-5), privacidad (NFR-1, refuerzo del PRD coherente con la intimidad del dato del brief).

---

*Auditoría generada por reconciliación documental. Los gaps 1–5 requieren decisión del usuario o ajuste editorial del PRD; ninguno invalida el PRD como base para arquitectura.*
