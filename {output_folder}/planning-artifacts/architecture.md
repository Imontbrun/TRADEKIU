---
stepsCompleted: [1, 2]
inputDocuments:
  - '{output_folder}/planning-artifacts/prds/prd-TRADEKIU-2026-06-12/prd.md'
  - '{output_folder}/planning-artifacts/prds/prd-TRADEKIU-2026-06-12/addendum.md'
  - '{output_folder}/planning-artifacts/prds/prd-TRADEKIU-2026-06-12/.decision-log.md'
  - '{output_folder}/planning-artifacts/briefs/brief-tradekiu-2026-06-12/brief.md'
  - '{output_folder}/planning-artifacts/briefs/brief-tradekiu-2026-06-12/addendum.md'
workflowType: 'architecture'
project_name: 'TRADEKIU'
user_name: 'Imontbrun'
date: '2026-06-12'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

---

## Project Context Analysis

> Enriquecido con revisión multi-perspectiva (John PM, Sally UX, Amelia Dev, Mary Analyst) y respuestas del usuario.
> Decisiones tomadas en esta fase marcadas con ✅.

### Requirements Overview

**Functional Requirements — arquitectónicamente agrupados:**

TRADEKIU tiene 21 FRs en 5 features que arquitectónicamente forman tres capas:

- **Captura (F1, F2, F3):** tres flujos de entrada que convergen en la entidad central "día de trading":
  - Check-in pre-sesión: formulario de 11 campos fijos (botones/sliders), editable hasta las 23:59 hora Caracas de ese mismo día, sin retroactivos. La ausencia de check-in es dato explícito (`no_checkin`), no NULL — es dato psicológico para el reporte.
  - Audio post-sesión: grabación via MediaRecorder API → upload → transcripción IA → extracción estructurada → borrado del audio. El usuario **no revisará la transcripción** — el pipeline es completamente automático. El sistema muestra estados visibles pero sin tarea obligatoria.
  - Importación CSV: idempotente, con `source` por trade desde v1 para recibir MetaApi en v2 sin migración.

- **Lectura (F4):** dashboard con métricas agregadas (P&L, win rate, R:R con promedio móvil), calendario mensual de P&L, y vista de día que une las tres capas por fecha Caracas.

- **Síntesis (F5):** reporte semanal generado por IA, disponible lunes 8:00 AM Caracas, que profundiza con la historia acumulada. No es plantilla estática.

- **Procesos desatendidos:** recordatorio 9:00 AM lun–vie (FR-21) vía **Telegram Bot** ✅ (canal elegido — usuario tiene Telegram, es gratis, confiable, sin dependencia de PWA ni permisos de push) y generación del reporte lunes (FR-20). Ambos requieren jobs programados server-side.

**Non-Functional Requirements:**

- **NFR-1 Privacidad:** datos íntimos; solo salen hacia APIs de IA. Sin analytics ni telemetría.
- **NFR-2 Costo:** tiers gratuitos, open-source. Estimación de IA resuelta en esta fase (ver Restricciones): **< $1/mes** — aprobado.
- **NFR-3 Fricción:** check-in < 3 min, audio en 1 toque, ritual total < 10 min. Incluye tiempo de carga en frío: la app debe ser usable en iPhone 15 con red móvil en segundos (app shell cacheado, skeleton screens).
- **NFR-4 Durabilidad:** el NFR dominante. BD gestionada con backups + **backup automático semanal fuera del proveedor** (no solo export manual) + exportación completa en 1 clic.
- **NFR-5 Single-user:** sin gestión de usuarios; protección simple si se expone a internet.

**Scale & Complexity:**

- Primary domain: full-stack web, SPA responsive + backend ligero + cron jobs + integraciones IA
- Complexity level: media-baja en infraestructura; media en el pipeline de IA y el reporte evolutivo
- Estimated architectural components: ~8 (frontend, API/BFF, BD, storage temporal audio, pipeline IA como máquina de estados, scheduler/cron, Telegram bot, módulo import CSV)

### Technical Constraints & Dependencies

**Zona horaria:** America/Caracas (UTC-4). Toda lógica de "día" (frontera 23:59 Caracas para FR-3, crons 9:00/8:00 AM Caracas) se resuelve server-side en un único punto canónico. La validación del cutoff de check-in ocurre en Postgres/RPC del servidor, nunca en el cliente.

**Dispositivos del usuario:**
- Computadora: desktop-first
- **iPhone 15 / Safari iOS** ✅ — MediaRecorder emite `audio/mp4` (AAC), no `audio/webm`. El `mimeType` se detecta con `MediaRecorder.isTypeSupported()` y se persiste junto al blob. Riesgo conocido: en iOS la grabación se corta si la pantalla se bloquea durante los 5 min máx. — requiere test manual en Safari iOS.

**Pipeline de audio — diseñado como máquina de estados persistida en BD:**

```
uploaded → transcribing → transcribed → extracting → extracted → audio_deleted
                ↓                              ↓
         transcription_failed          extraction_failed (transcript intacto)
```

- El audio se borra **tras transcripción exitosa**, no tras extracción. Si la extracción falla, el transcript es el respaldo.
- El usuario no revisa la transcripción ✅ → el pipeline corre automáticamente. La UI muestra el estado pero no exige acción.
- Cada paso es idempotente y reanudable (resuelve el límite de wall-clock ~150s de Edge Functions free).
- El audio crudo persiste en el dispositivo hasta confirmación de `uploaded` (resiliencia a red móvil/internet inestable).
- En estado `extraction_failed`: el transcript se conserva, la extracción se reintenta automáticamente 1 vez con el error de validación en el prompt. Si falla de nuevo: estado visible + opción de reintento manual desde UI, nunca día silenciosamente vacío.

**Estado visible del sistema (implicación arquitectónica de NFR-3 y hábito):**

Cada artefacto del día expone su estado al usuario. Un día en blanco en el calendario que el usuario sabe que registró destruye la confianza en todo el journal. Estados mínimos requeridos:
- Check-in: pendiente / completo / sin-contexto (día pasado sin check-in)
- Audio: sin-grabar / grabando / subiendo / procesando / listo / fallido-recuperable
- CSV: sin-importar / importado (N trades)
- Reporte semanal: generándose / disponible / fallback-datos-crudos (si cron falló)

**Estimación de costo IA (NFR-2, cumple condición "antes de construir"):**

| Concepto | Modelo | Costo/mes |
|---|---|---|
| Transcripción: 5 min/día × 22 días | Whisper ($0.006/min) | ~$0.66 |
| Extracción estructurada diaria | GPT-4o-mini o Gemini Flash | ~$0.10 |
| Reporte semanal × 4 (crece con historial) | GPT-4o-mini | ~$0.20 |
| **Total mes 1** | | **< $1/mes** ✅ |
| Total mes 3 (reporte con 90 días de historial) | GPT-4o o similar | **< $3/mes** (estimado) |

El reporte evolutivo (FR-19) puede escalar en tokens al mes 3 — diseñar con contexto comprimido/resumido de historia, no historial raw completo.

**Dependencias externas y sus riesgos:**

- **Supabase (candidato plataforma):** free tier pausa proyectos tras ~7 días de inactividad → mitigación: GitHub Actions cron como keep-alive ping gratuito. Riesgo de eliminación de free tier (precedente Heroku 2022) → NFR-4 exige backup automático semanal a destino fuera de Supabase (GitHub privado o similar).
- **API de transcripción (Whisper/OpenAI o alternativa):** external, sin SLA en free. Fallo no debe bloquear el día — el estado `transcription_failed` preserva el audio hasta reintento.
- **MetaApi (v2):** free tier 1 cuenta personal MT4/MT5. Riesgo de cambio de tier similar al de Supabase — evaluar en fase v2. El esquema de trades incluye `source` desde v1.
- **Telegram Bot:** canal del recordatorio FR-21 ✅. Si el proyecto Supabase está pausado, el cron no corre y el bot no envía — el keep-alive de GitHub Actions mitiga esto también.

**CSV TradingView:** formato de columnas **no validado aún** (Pregunta Abierta #2 del PRD). Primer sprint debe incluir un export real antes de diseñar el schema de trades. Parser versionado que falla explícito ante columnas desconocidas. Idempotencia por hash de fila normalizada. El archivo CSV original se preserva en Storage (durabilidad + re-import).

### Cross-Cutting Concerns Identificados

1. **La entidad "día de trading" como agregado central:** check-in, extracto de audio y trades se asocian por fecha (fecha-Caracas). El día existe aunque falte alguna capa — la ausencia se representa explícitamente. Sobrevive el cambio de fuente CSV → MetaApi v2 gracias al campo `source` y al modelo agnóstico.

2. **Zona horaria canónica:** un único módulo server-side gestiona todas las conversiones a/desde America/Caracas. Crons, fronteras de día, y validaciones de FR-3 pasan por él.

3. **Pipeline de IA resiliente y observable:** máquina de estados en BD, pasos idempotentes, borrado de audio solo tras transcripción exitosa, extracción con schema validation (zod o equivalente), output nunca "responde en JSON" sino structured outputs/tool-use. Tests de contrato con fixtures, sin LLM real en CI.

4. **Hábito como primera-clase en el diseño:** el monitoreo del hábito tiene su propia lógica — si el check-in no ocurrió Y el recordatorio Telegram no salió, eso es visible en la app. El reporte semanal nota ausencias explícitas. El ritual del lunes tiene fallback a datos crudos si el reporte no está listo.

5. **Idempotencia:** import CSV re-subible sin duplicados; generación de reporte re-ejecutable sin duplicar reportes; pipeline de audio reanudable por estado.

6. **Durabilidad activa:** backup automático semanal a destino externo (no solo export manual); CSV original preservado; transcript preservado aunque falle la extracción.

7. **Costo de IA medible:** registrar consumo por llamada (modelo, tokens, costo estimado) desde el día 1 para validar NFR-2 y detectar escalada del reporte evolutivo.

8. **Tiempo de carga en frío (NFR-3 en móvil):** app shell cacheado, skeleton screens, PWA opcional — el check-in debe estar visible en < 3 segundos en iPhone 15 con red móvil.
