# Protocolo de Handoff entre Agentes

Este documento define **cómo se pasa trabajo de un agente a otro** dentro de un flujo coordinado por el orquestador (`/orchestration/orchestrator.md`). Un handoff mal definido es la causa más común de que dos agentes produzcan resultados incompatibles. Todo handoff sigue este formato — no se pasa trabajo de forma implícita o resumida en prosa.

> Adaptado a `react-base-app`: el equipo son 3 agentes (`designer`, `frontend`, `qa-tester`) — no hay agente `backend`, porque el proyecto no tiene servidor propio (ver `/context/project-context.md`). Donde el flujo genérico tendría un handoff `backend → frontend` con un contrato DTO, acá el equivalente es el **schema Zod que `frontend` define para validar la respuesta de la API externa de GitHub** — ese schema es la fuente de verdad, y lo define `frontend` porque no hay un agente de servidor que lo emita.

## Estructura de un handoff

```markdown
### Handoff: [Agente origen] → [Agente destino]

**Artefacto entregado**: (qué se produjo — spec, schema de datos, componente, suite de tests)

**Contenido**:
(el artefacto completo o la referencia exacta a él — nunca un resumen que omita detalles accionables)

**Restricciones que el agente destino debe respetar**:
- (ej. "el schema Zod que valida la respuesta de GitHub no debe reinterpretarse sin actualizarlo primero")
- (ej. "el estado de error diseñado incluye estos 3 casos, todos deben cubrirse en la UI")

**Pendiente de confirmar** (si aplica):
- (cualquier ambigüedad que el agente destino debe resolver o escalar, no asumir en silencio)
```

## Handoffs típicos del flujo estándar

### 1. `designer` → `frontend`

**Entrega**: especificación de UX/UI — flujo de usuario, wireframe/mockup (o descripción visual detallada si no hay render), estados completos del componente (default/hover/focus/loading/error/vacío), valores exactos de diseño (espaciado, color de la paleta `blue`, tipografía).

**Restricción clave**: `frontend` no reinterpreta ni simplifica los estados definidos por `designer` sin señalarlo — si un estado es técnicamente costoso de implementar, se reporta como conflicto al orquestador, no se omite silenciosamente.

### 2. `designer` → `frontend` (cuando el diseño implica datos de la API de GitHub)

**Entrega**: qué datos de GitHub necesita la interfaz (perfil, repos, stars, lenguajes, calendario de contribuciones), en qué formato se muestran, y qué pasa visualmente si el dato no está disponible.

**Restricción clave**: `frontend` es quien traduce esto a un schema Zod real que valida la respuesta de la API — el diseño informa qué datos hacen falta, pero la especificación técnica del contrato (campos exactos, opcionalidad, manejo de nulos) la define `frontend`, no `designer`.

### 3. `frontend` → `qa-tester` (contrato de datos externos)

**Entrega**: el schema Zod exacto que valida la respuesta de la API de GitHub para el recurso en cuestión, más los casos de fallo ya conocidos (rate limit, 404, campos ocasionalmente ausentes en la respuesta real de GitHub).

**Restricción clave**: este schema es la fuente de verdad para los mocks de MSW que `qa-tester` escribe — `qa-tester` no debe inventar un shape de respuesta distinto al que `frontend` realmente valida en runtime, o los tests certifican un contrato que el código no respeta.

### 4. `frontend` → `qa-tester` (implementación)

**Entrega**: el código implementado (componentes, hooks de TanStack Query), más los casos límite ya identificados por `frontend` durante la implementación (si ya sabe que un campo puede venir vacío en ciertos perfiles, se lo dice a `qa-tester` explícitamente, no se asume que lo va a descubrir solo).

**Restricción clave**: `qa-tester` no se limita a los casos que `frontend` señaló — los toma como punto de partida, no como el universo completo de casos a cubrir.

### 5. `qa-tester` → `frontend` o `designer` (hallazgo de bug o gap)

**Entrega**: reporte estructurado (pasos para reproducir, esperado vs. actual, severidad) dirigido específicamente al agente responsable del artefacto donde está el gap — `frontend` si es de implementación, `designer` si el gap es que un estado nunca fue especificado.

**Restricción clave**: el hallazgo no se cierra hasta que el agente responsable confirma el fix y `qa-tester` valida con un test de regresión — no se da por resuelto solo porque se corrigió el código.

## Reglas generales

- **El artefacto de un handoff es siempre concreto y completo** — código real, schema real, especificación real. Nunca "el componente debería tener algo como..." sin la definición exacta.
- **Toda restricción no respetada se reporta como conflicto al orquestador**, no se resuelve unilateralmente entre dos agentes sin que quede registrado.
- **Un handoff que introduce una convención reutilizable** (ej. un patrón de manejo de errores de la API de GitHub adoptado en el proceso) se registra en `/context/project-context.md`, sección "Decisiones de arquitectura registradas" — para que el próximo flujo no tenga que redescubrirlo.
- Si el proyecto llegara a incorporar un backend propio en el futuro, este documento debe extenderse con los handoffs `designer → backend` y `backend → frontend` del flujo genérico — no se agregan de forma anticipada mientras no exista ese agente.
