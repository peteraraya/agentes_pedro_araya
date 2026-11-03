# AGENTS.md — Punto de entrada de Rompe la Racha

Este directorio contiene el framework de trabajo del producto Rompe la Racha. La aplicación está en la raíz del workspace; lee la documentación de contexto antes de modificar código.

## Orden de lectura

1. [`README.md`](./README.md) — mapa del framework y del workspace.
2. [`context/project-context.md`](./context/project-context.md) — producto, stack y reglas de privacidad.
3. [`roles/roles-matrix.md`](./roles/roles-matrix.md) — responsabilidades y secuencia.
4. [`orchestration/orchestrator.md`](./orchestration/orchestrator.md) — workflow para tareas.
5. [`context/definition-of-done.md`](./context/definition-of-done.md) — validación de cierre.
6. La spec activa en [`specs/`](./specs/) y la skill activada por el tipo de tarea.

## Mapa físico

- `orchestration/`: coordinador de flujo y runbook de incidentes.
- `roles/`: matriz de responsabilidades del equipo conceptual.
- `skills/`: conocimiento activado por tipo de trabajo.
- `context/`: fuente de verdad del producto, tokens, handoffs y DoD.
- `specs/`: especificaciones de features y contratos.
- `apps/web/`, `apps/api/`, `packages/shared-types/`: implementación.

No existe una carpeta `agents/` con subagentes ejecutables en este workspace. Los roles del flujo son responsabilidades de trabajo; cuando no hay un ejecutor dedicado disponible, el agente actual completa las etapas en secuencia y registra los handoffs concretos. No afirmes que se invocó un agente que no existe.

## Reglas no negociables

- `context/project-context.md` prevalece sobre ejemplos genéricos de skills.
- Privacidad opt-in: público y ranking son consentimientos independientes; perfiles privados no aparecen en ninguna superficie pública.
- El contrato NestJS se publica en `specs/api/` antes de consumirse; las respuestas del navegador se validan con Zod.
- Los datos públicos nunca incluyen correo, identificadores internos ni credenciales.
- Mantén estados completos, accesibilidad y lenguaje empático; ver tokens y DoD.
- Usa el flujo de `orchestration/orchestrator.md`; omite una etapa solo si no aplica y decláralo.
