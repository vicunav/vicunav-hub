# Política de modelos, tokens y Qwen

## Objetivo

Usar el modelo menos costoso que complete correctamente la tarea, sin provocar
retrabajo. La configuración global de Codex ya usa `gpt-5.6-terra` con razonamiento
medio; es la base para trabajo cotidiano, no una obligación de mantener la misma
selección ante cualquier riesgo.

## Enrutamiento de modelos

| Tipo de trabajo | Selección inicial | Cuándo escalar |
| --- | --- | --- |
| Consulta breve, inventario o transformación mecánica | Terra, bajo | Si aparecen ambigüedades o varias fuentes en conflicto |
| Implementación cotidiana y depuración acotada | Terra, medio | Si el cambio cruza contratos o el fallo no es local |
| Arquitectura, seguridad o cambio amplio de alto impacto | Sol, medio | A alto solo si la primera pasada identifica decisiones difíciles |
| Problema excepcional con múltiples tradeoffs | Sol, alto | Max únicamente con una razón concreta y comunicada |
| Trabajo divisible en subtareas independientes | Modelo según cada subtarea | Ultra/subagentes solo con autorización y ahorro claro de tiempo |

Una tarea principal ya iniciada no siempre permite que el agente cambie el modelo por
sí mismo. En ese caso Codex debe recomendar una sola vez la selección exacta en la
interfaz y continuar con el mejor esfuerzo disponible, salvo que la calidad requiera
detenerse.

## Control de contexto

- Una tarea corresponde a un issue coherente; cambiar de issue permite iniciar una
  tarea limpia cuando el contexto anterior ya no ayuda.
- Leer primero instrucciones, issue, contrato y archivos directamente afectados.
- Preferir búsquedas dirigidas, diffs y salidas acotadas a volcados completos.
- Mantener decisiones, aceptación y resultados en issues, PRs, ADRs y este hub, no
  solo en chats.
- Ejecutar checks dirigidos durante la edición y la validación completa una vez antes
  de publicar.
- No volver a explicar contexto estable en cada turno; enlazar la fuente canónica.

## Avisos de consumo

Codex avisa únicamente cuando la recomendación sea accionable:

- al detectar que el modelo o esfuerzo activo es excesivo o insuficiente;
- antes de usar una modalidad costosa o delegación autorizada;
- cuando una salida, reintento o carga de contexto se pueda evitar;
- al cerrar un lote, si existe una mejora concreta para la siguiente tarea.

No se repite el mismo aviso en cada mensaje: esa repetición también consume tokens.

## MCP `qwen-bridge`

### Naturaleza global y disponibilidad

- `qwen-bridge` es una herramienta global de Codex para ahorrar tokens en tareas
  mecánicas, repetitivas y objetivamente verificables. No es un agente responsable de
  un repositorio, una dependencia del producto ni una capacidad que cada proyecto deba
  configurar para funcionar.
- El modelo corre mediante Ollama en otra laptop. Esa laptop puede estar apagada o no
  disponible, por lo que Qwen se usa como una optimización oportunista y nunca como un
  requisito para completar una tarea.
- Codex conserva la responsabilidad de delimitar el trabajo, revisar el resultado y
  ejecutar las validaciones aplicables. Si Qwen no está disponible, continúa con el
  modelo principal.
- Las allowlists de escritura pueden restringirse por proyecto como control de
  seguridad. Esas allowlists no convierten a Qwen en una configuración funcional ni en
  parte de la arquitectura de ese repositorio.

### Estado técnico verificado el 2026-08-03

- El servidor está registrado globalmente y expone `ask_qwen_review`,
  `ask_qwen_apply` y `qwen_stats`.
- Usa Ollama mediante un endpoint privado fuera de esta Mac con
  `qwen2.5-coder:7b`; no se publica la dirección de infraestructura.
- Una prueba anterior devolvió `fetch failed`, resultado esperable cuando la laptop
  remota está apagada o inaccesible. No se hicieron reintentos.
- La escritura directa se deniega si el proyecto no define
  `QWEN_APPLY_ALLOWED_PATHS`. En `vicunav-gutenberg` existe una allowlist limitada a
  borradores y evidencia generada; esto es solo una frontera de seguridad para
  `ask_qwen_apply`.
- El registro consultado acumulaba 11 llamadas: cuatro archivos escritos, cuatro
  fallos de ruta/validación y tres borradores. El ahorro estimado de escritura directa
  era de unos 153 tokens; los borradores no garantizan ahorro porque el modelo
  principal puede tener que reemitirlos.

### Uso autorizado

- `ask_qwen_review`: borradores pequeños y verificables que necesitan inspección y
  cuyo coste de revisión sea menor que redactarlos con el modelo principal.
- `ask_qwen_apply`: lotes mecánicos dentro de una allowlist de áreas no ejecutables,
  con validador objetivo y revisión posterior del diff.
- Si Qwen falla una vez por conexión, Codex sigue directamente y no vuelve a probarlo
  en esa tarea.
- Nunca se envían secretos, datos personales, formularios reales ni contenido de
  producción.

### Límites técnicos observados

- El validador Gutenberg actual cuenta aperturas y cierres, pero no demuestra que el
  anidamiento, los tipos de bloque o los atributos sean válidos.
- La comprobación de rutas usa rutas resueltas léxicamente y no documenta una defensa
  contra symlinks dentro de una carpeta permitida.
- La escritura reemplaza el archivo de destino; no es atómica ni crea respaldo.
- El log de estadísticas predeterminado vive junto al bridge y agrega actividad de
  varios proyectos salvo que se configure `QWEN_STATS_PATH` por proyecto.

Por estos límites, Qwen no escribe directamente runtime, contratos, configuración,
migraciones, autenticación, seguridad, lógica de negocio ni archivos de GitHub. Si en
el futuro se amplía su alcance, primero se deben endurecer el aislamiento de rutas,
los validadores y la escritura atómica.

## Fuentes

- [Selección de modelos en Codex](https://learn.chatgpt.com/docs/models)
- [Guía oficial de GPT-5.6](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-5.6)
- [Estado canónico del ecosistema](../handoff/estado-ecosistema.md)
- [Backlog multirrepositorio](../handoff/backlog-ecosistema.md)
