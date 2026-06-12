# PRD Quality Review — TRADEKIU

> Revisión contra la rúbrica `.claude/skills/bmad-prd/assets/prd-validation-checklist.md`.
> Calibración: proyecto hobby/personal, usuario único (el creador), rigor objetivo ~2–4 páginas.
> Documentos evaluados: `prd.md` + `addendum.md` (y `.decision-log.md` como fuente de trazabilidad).
> El PRD alimenta bmad-ux, bmad-create-architecture y bmad-create-epics-and-stories — la usabilidad downstream pesa más que en un PRD standalone.

## Overall verdict

PRD sólido y honesto para su nivel de stakes: tiene tesis clara (construir el hábito de journaling y descubrir la metodología propia con datos reales), un journey de referencia que disciplina el alcance ("Si una feature no aparece en este ritual, probablemente sobra"), métricas con contra-métricas, y supuestos etiquetados con su estado de validación trazable en el decision log. Lo que está en riesgo es menor pero real: el esquema de extracción de FR-8 es la pieza más difusa que heredará arquitectura/stories, el addendum arrastra un residuo de la decisión superada del "domingo 18:00", y un par de ambigüedades pequeñas (períodos del dashboard, "fin del día") obligarían a un agente posterior a inventar detalles. Ningún hallazgo es bloqueante; con tres correcciones puntuales el documento queda listo para la cadena downstream.

## Decision-readiness — strong

Las decisiones están escritas como decisiones, no como "consideraciones". FR-3 (sin check-ins retroactivos), FR-9 (el audio se borra tras transcripción, "sin opción de conservar audios"), FR-17 (lunes 8:00 AM, corrigiendo explícitamente el "domingo" del brief) — todas con su racional en el addendum (§ "Decisiones de producto con racional"). Las Preguntas Abiertas (§7) son genuinamente abiertas: formato real del CSV y costo de IA, ambas con owner asignado (arquitectura/primer sprint), y la #1 marcada como resuelta en vez de borrada — buena trazabilidad. El diferimiento de NFR-2 ("el usuario no fijó un tope — la fase de arquitectura debe estimar... y presentárselo antes de construir") es un diferimiento honesto con guía concreta ("un dígito de dólares al mes"), no una evasión.

Sin hallazgos: la dimensión sostiene el nivel exigible y más.

## Substance over theater — strong

Cero teatro de personas (un solo usuario, nombrado "Imontbrun" en el ritual). Los NFR tienen umbrales propios del producto, no boilerplate: check-in <3 minutos, audio en 1 toque, ritual total <10 minutos como criterio de fallo (NFR-3); exportación en un clic (NFR-4); "sin analytics de terceros, sin telemetría" (NFR-1). La tabla de métricas (§5) incluye contra-métricas específicas y no genéricas ("Completar el check-in con datos falsos solo por mantener la racha") — eso es contenido ganado, no mobiliario. La visión (§1) es específica a este producto (plan de adopción paper→live→fondeada, reporte de psicología el lunes pre-open); no es intercambiable con cualquier trading journal.

Sin hallazgos.

## Strategic coherence — strong

El PRD tiene tesis y la usa: el §2 (Ritual Diario) funciona como filtro de alcance explícito y las cinco features mapean 1:1 sobre los pasos del ritual. Las métricas de éxito validan la tesis (hábito, primer insight, claridad, continuidad), no actividad genérica — no hay DAU/MAU de relleno. El alcance v1/v2 (§6) sigue el plan de adopción de §1 (mes 1 paper/CSV, mes 2 live/MetaApi), y FR-13 protege la transición sin sobre-construir ("el modelo de datos debe poder recibir trades de ambas fuentes sin migración").

Sin hallazgos.

## Done-ness clarity — adequate

La mayoría de los FRs son verificables: FR-1 enumera los 11 campos con tipos y opciones inline; FR-15 define el calendario con precisión (verde/rojo/vacío, fines de semana neutros, navegable); FR-17/FR-18 fijan hora exacta, secciones del reporte, umbral de 3 días y formato de evidencia con ejemplo. Pero hay tres puntos donde un agente de stories tendría que inventar:

### Findings
- **medium** Esquema de extracción de FR-8 sin estructura (§3 F2, FR-8) — "estado emocional percibido, eventos clave (errores, aciertos, desviaciones del plan), y razones de acción/inacción" nombra categorías pero no define el esquema: ¿campos fijos o lista libre? ¿el estado emocional es escala, enum o texto? Es el FR del que dependen la vista de día (FR-16) y el reporte semanal (FR-17), y el más difuso del documento. *Fix:* definir un esquema mínimo del extracto (3–5 campos con tipo) o delegarlo explícitamente a arquitectura con un `[NOTA]` como se hizo con el canal de FR-21.
- **medium** "Período seleccionado" del dashboard sin enumerar (§3 F4, FR-14) — no se dice qué períodos puede seleccionar el usuario (¿día/semana/mes/rango libre?). bmad-ux tendrá que decidirlo solo. *Fix:* enumerar los períodos del v1 (p. ej. semana, mes, desde el inicio).
- **low** "Fin del día" de FR-3 sin frontera precisa (§3 F1, FR-3) — "puede editarse hasta el fin del día" presumiblemente es medianoche America/Caracas, pero no está dicho; para la lógica de bloqueo de edición hace falta el corte exacto. *Fix:* añadir "(23:59 hora de Caracas)" o el corte que el usuario prefiera.
- **low** Clave de deduplicación de FR-11 indefinida (§3 F3, FR-11) — "idempotente" es el requisito correcto, pero la clave de duplicado depende de las columnas reales del CSV (Pregunta Abierta #2). Está bien diferirlo, pero conviene ligarlo explícitamente. *Fix:* añadir en FR-11 una referencia a la Pregunta Abierta #2 ("clave de deduplicación se define al validar el CSV real").

## Scope honesty — strong

La sección de alcance (§6) hace trabajo real: "Explícitamente fuera de v1" lista cinco exclusiones concretas, incluida la aclaración fina de que FR-21 es la única notificación del v1. Los `[SUPUESTO]` marcan inferencias reales (FR-7, FR-10, FR-18, FR-20, NFR-5) y el decision log registra cuáles fueron confirmados, corregidos o diferidos — la corrección de FR-17 (domingo→lunes) quedó documentada con la divergencia respecto al brief señalada. Densidad de items abiertos (2 preguntas + 5 supuestos vigentes) apropiada para los stakes.

### Findings
- **low** Sin índice de supuestos al final del PRD (§7) — los 5 `[SUPUESTO]` vigentes solo existen inline; su estado de validación vive en `.decision-log.md`, que es un dotfile que un agente downstream no leerá por defecto. *Fix:* añadir una mini-lista al final de §7 con los supuestos aún no confirmados por el usuario.

## Downstream usability — adequate

Los IDs son únicos y contiguos (FR-1..FR-21, NFR-1..5) y las referencias cruzadas internas resuelven (FR-12↔Pregunta #2, NFR-2↔Pregunta #3, FR-20↔addendum). La referencia externa de FR-1 al addendum del brief (`briefs/brief-tradekiu-2026-06-12/addendum.md`) existe y resuelve — verificado — y el PRD mitiga el riesgo enumerando los 11 campos inline de todos modos. No hay glosario, pero los sustantivos de dominio (check-in, extracto, reporte semanal, vista de día) se usan de forma consistente; para este tamaño es aceptable.

### Findings
- **medium** Residuo "domingo 18:00" en el addendum (addendum § Notas técnicas, "Generación del reporte (FR-20)") — dice "lazy generation al abrir la app después del domingo 18:00", que es el supuesto #7 superado; el PRD (FR-17/FR-20) manda lunes 8:00 AM Caracas. No es contradicción dura (generar tras el domingo 18:00 cumpliría el plazo del lunes) pero el addendum es insumo directo de arquitectura y arrastra la decisión vieja. *Fix:* reescribir la nota alineada a FR-20: "disponible antes de las 8:00 AM del lunes o al primer acceso del lunes".

## Shape fit — strong

La forma es exactamente la correcta para hobby/solo encadenado a UX→arquitectura→stories: spec de capacidades con un único journey de referencia (§2) en lugar de una batería de UJs, métricas operativas del propio usuario, sin secciones de go-to-market/stakeholders/compliance que aquí serían mobiliario. ~3 páginas, en español, consistente con el brief. Ni sobre-formalizado ni infra-formalizado.

Sin hallazgos.

## Mechanical notes

- **Etiqueta "Ritual dominical" desactualizada** (§5, tabla de métricas): la fila dice "Ritual dominical" pero su contenido habla de "la preparación del lunes" — residuo del brief previo a la corrección domingo→lunes. Renombrar a "Ritual del lunes".
- **FR-21 fuera de secuencia numérica**: aparece en F1 después de FR-4 (fue añadido en la validación de supuestos). IDs únicos y sin huecos; solo el orden visual no es numérico. Aceptable, pero conviene saberlo al extraer.
- **Decision log vs PRD**: el log inicial lista "FR-9 ... (configurable)" y "FR-17 ... domingo 18:00"; ambos fueron corregidos en la validación y el PRD refleja la versión final — consistente. El log es la fuente de qué supuestos siguen pendientes (FR-7, FR-10, FR-18, FR-20, NFR-5).
- **Referencia al brief** (`briefs/brief-tradekiu-2026-06-12/`): resuelve dentro de `planning-artifacts/`; brief, addendum y decision log del brief existen.
- **Sin glosario**: tolerable a este tamaño; los términos clave se usan sin deriva (singular/plural consistente en "extracto(s) de audio").
- **Roundtrip de supuestos**: los 5 `[SUPUESTO]` inline no tienen índice en el documento (ver hallazgo low en Scope honesty); el roundtrip completo solo es posible vía `.decision-log.md`.
