# Framework de desarrollo

Guías de orquestación, contexto, skills y contratos para implementar Rompe la Racha. La aplicación vive en `../apps/`; el producto es una SPA Vite/React + TanStack y una API NestJS/PostgreSQL.

## Fuente de verdad

- [`context/project-context.md`](./context/project-context.md): producto, stack, reglas de privacidad y decisiones.
- [`context/design-tokens.md`](./context/design-tokens.md): valores de identidad visual y accesibilidad.
- [`orchestration/orchestrator.md`](./orchestration/orchestrator.md): clasificación y secuencia de trabajo.
- [`roles/roles-matrix.md`](./roles/roles-matrix.md): responsabilidades de diseño, frontend, backend y QA.
- [`context/handoff-protocol.md`](./context/handoff-protocol.md): contratos y artefactos entre etapas.
- [`context/definition-of-done.md`](./context/definition-of-done.md): criterios de cierre.
- [`specs/la-racha-mvp.md`](./specs/la-racha-mvp.md): alcance vigente.
- [`specs/api/la-racha.md`](./specs/api/la-racha.md): contrato backend → frontend.

## Flujo de trabajo

Para features de producto con datos: diseño → contrato/backend → frontend → QA. El backend es la fuente de verdad de endpoints y payloads; el frontend valida respuestas con los schemas compartidos de Zod. Las tareas pequeñas pueden omitir etapas que no apliquen, dejando explícito el motivo.

## Skills

Las skills están en [`skills/`](./skills/skills-README.md). Consultar solo las que active el stack/tarea; los valores del producto prevalecen sobre ejemplos genéricos.

## Desarrollo local

Desde la raíz del workspace, seguir las instrucciones de [`../README.md`](../README.md).

## Adaptación a otro proyecto

Este framework está adaptado a Rompe la Racha. Antes de reutilizarlo en otro producto, seguir [`ADAPTING.md`](./ADAPTING.md) y actualizar contexto, tokens, contratos y parámetros de skills en conjunto.

Licencia MIT. Autor: [Pedro Araya Gálvez](https://github.com/peteraraya).
