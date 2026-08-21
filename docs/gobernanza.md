# Gobierno de decisiones del ecosistema

Este documento define cómo se razonan, registran y propagan decisiones que afectan a
uno o varios repositorios Vicunav. El hub coordina la decisión, pero no reemplaza la
fuente de verdad técnica de cada paquete.

## Autoridad y alcance

- El usuario conserva la decisión final de producto y de cualquier acción destructiva,
  irreversible o con impacto externo.
- El hub registra arquitectura, prioridades, dependencias y decisiones transversales.
- Cada repositorio es dueño de su código, contratos públicos, pruebas y documentación
  específica.
- `vicunav-standards` es la única fuente normativa para reglas compartidas.
- `vicunav-gutenberg` es un proyecto relacionado pero independiente y no participa en
  el backlog de este ecosistema.

## Flujo de una decisión

1. **Explorar:** describir el problema, las restricciones, las alternativas y sus
   consecuencias. Esta etapa no modifica archivos por sí sola.
2. **Decidir:** registrar la elección en un ADR cuando afecte arquitectura, límites o
   contratos; usar el estado o el backlog cuando sea una prioridad operativa.
3. **Asignar fuente de verdad:** determinar qué repositorio posee el valor, contrato o
   comportamiento resultante.
4. **Propagar:** crear un issue y un pull request atómicos en cada repositorio que deba
   cambiar. No mezclar varios repositorios en un mismo commit.
5. **Verificar:** comprobar el resultado en cada consumidor y actualizar el estado y el
   backlog del hub cuando todos los cambios requeridos estén cerrados.

Los estados recomendados son `borrador`, `decidida`, `planificada`, `aplicada` y
`verificada`. Una decisión no se considera propagada mientras algún consumidor
obligatorio siga pendiente.

## Cambios derivados de un diseño aprobado

Cuando un producto parte de un baseline visual, los estados funcional y visual se
registran por separado. Una implementación puede tener runtime verificado y continuar
pendiente como producto integrado.

La verificación visual exige:

- fuente y commit inmutables;
- inventario de páginas, componentes, estados, viewports, fuentes y assets;
- mapa de propiedad entre theme, plugin vertical y demo;
- comparación del render fuente y WordPress con entorno equivalente;
- evidencia lado a lado y overlay por página, estado y viewport;
- diferencias deliberadas registradas y aprobación humana del checkpoint.

No se acepta como sustituto la presencia de copy, bloques válidos, rutas 200, ausencia
de overflow, Lighthouse ni una auditoría de accesibilidad. Todas esas pruebas siguen
siendo obligatorias, pero verifican dimensiones diferentes del producto.

La presencia de configuración en archivos o base de datos tampoco acredita su efecto.
El consumidor debe demostrar la configuración aplicada en el CSS y el render finales.
El [ADR 0010](adr/0010-fidelidad-visual-bloqueante.md) contiene el contrato completo.

## Fuente de verdad por tipo de cambio

| Cambio | Fuente de verdad | Registro coordinador |
| --- | --- | --- |
| Arquitectura o límite entre paquetes | ADR del hub | Estado y backlog del hub |
| Prioridad, nuevo producto o demo | Hub | Backlog del hub |
| Convención transversal | `vicunav-standards` | ADR si cambia arquitectura |
| Contrato público de un paquete | Repositorio propietario | ADR y enlaces desde el hub |
| Token visual, color o tipografía | `vicunav-theme-core` | Issue del theme y consumidores |
| Markup y estado funcional de un bloque de dominio | Plugin vertical propietario | Issue del vertical y demo consumidor |
| Lógica de negocio | Plugin o vertical propietario | Issue y pruebas del repositorio |
| Composición de una demo | Repositorio de la demo | Backlog del hub |

No se copia el mismo Markdown entre repositorios para simular propagación. Se prefieren
enlaces, submódulos, dependencias versionadas y contratos públicos. Cuando un archivo
local sea obligatorio, cada consumidor actualiza explícitamente su referencia y valida
el cambio.

## Contenido mínimo de una decisión

- Contexto y problema observable.
- Alternativas consideradas.
- Decisión y responsable final.
- Consecuencias y riesgos.
- Fuente de verdad.
- Repositorios afectados.
- Plan de propagación y validación.
- Decisión anterior que reemplaza, cuando corresponda.

## Uso de los documentos canónicos

- [`docs/adr/`](adr/): decisiones arquitectónicas aceptadas y su historia.
- [`docs/handoff/estado-ecosistema.md`](handoff/estado-ecosistema.md): fotografía actual
  y hechos vigentes.
- [`docs/handoff/backlog-ecosistema.md`](handoff/backlog-ecosistema.md): trabajo
  pendiente, orden y dependencias.
- [`docs/ia/modelos-tokens-qwen.md`](ia/modelos-tokens-qwen.md): política operativa de
  modelos y herramientas auxiliares.
