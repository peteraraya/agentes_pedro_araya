# `/skills` — Índice de habilidades especializadas

Cada archivo `SKILL.md` en esta carpeta documenta el estándar del equipo para un dominio técnico específico: cuándo usar qué herramienta, errores comunes reales (no solo "cómo se usa" superficial), y un checklist rápido al generar código. Los agentes en `/agents` las consultan de forma autónoma cuando una tarea las activa — no hace falta pedirlo explícitamente.

Este equipo es **full-stack** (`react-base-app`): frontend SPA (Vite + React + TanStack Router + Tailwind) + backend NestJS. El catálogo de skills cubre ambos lados, más las librerías alternativas disponibles para otros proyectos del portafolio.

## Skills activas en `react-base-app`

| Skill | Dominio | Consumida principalmente por |
|---|---|---|
| `frontend-design` | Dirección visual, tipografía y estilo intencional; sistema blue de marca (ver `/context/design-tokens.md`) | `frontend`, `designer` |
| `vite-tanstack-tailwind` | Vite + React 19 + TanStack Router/Query/Form + Tailwind | `frontend`, `designer` (restricciones de implementación) |
| `nestjs-secure-backend` | Arquitectura, seguridad y testing de backends NestJS; DTOs, guards, manejo de errores, TypeORM/Prisma | `backend` |
| `qa-qc-react-nestjs` | Estrategia de testing full-stack (React + NestJS): pirámide, contract testing, Jest/Supertest, Vitest/MSW/Playwright | `qa-tester`, `backend`, `frontend` |
| `qa-qc-react-vite` | Setup de testing del frontend Vite en particular (Vitest, RTL, jsdom, MSW) | `qa-tester`, `frontend` |
| `cicd-expert-pipelines` | Pipelines de CI/CD (GitHub Actions / Vercel), PWA build | `frontend`, `backend`, `orchestrator`, `qa-tester` |
| `devops-docker-kubernetes` | Dockerfile de producción, Kubernetes, despliegue y orquestación de contenedores | `backend` |
| `recharts-charts` | Gráficos con recharts y react-github-calendar — tema, accesibilidad | `frontend`, `designer`, `qa-tester` |
| `ui-design-system` | Tokens de color blue, componentes UI, estados y contraste WCAG AA | `designer`, `frontend`, `qa-tester` |

## Skills disponibles (stack full-stack ampliado, se activan a pedido explícito)

Estas skills existen físicamente en `/skills` y las consumen los agentes del equipo, pero **no se activan por defecto en `react-base-app`**: aplican cuando el usuario las pide explícitamente o cuando se trabaja sobre otro proyecto del portafolio de Pedro (el frontend de `react-base-app` es Vite/recharts, no Next.js/Nivo/Plotly, y no tiene mapas).

| Skill | Dominio | Cuándo se activa |
|---|---|---|
| `nextjs-2026-best-practices` | Arquitectura y buenas prácticas Next.js 16 (App Router, RSC, Turbopack) | `frontend` — solo para proyectos Next.js distintos de `react-base-app` |
| `nivo-professional-charts` | Gráficos con Nivo (`@nivo/*`) | `frontend`, `designer`, `qa-tester` — a pedido explícito del usuario |
| `plotly-expert-charts` | Gráficos interactivos/científicos con react-plotly.js | `frontend`, `designer`, `qa-tester` — a pedido explícito del usuario |
| `leaflet-maps-integration` | Mapas interactivos con react-leaflet | `frontend`, `designer`, `qa-tester` — a pedido explícito del usuario |

Cada skill es un paquete `.skill` (zip con `<nombre>/SKILL.md` dentro) en esta misma carpeta — si una tabla de activación en `/agents` menciona una skill que no está en estas tablas, es un error a corregir, no una skill "implícita".

## Cómo se activan

Cada agente en `/agents` tiene su propia tabla de activación que mapea skills a disparadores concretos. Esta tabla es la vista global; para el detalle de activación de cada agente, ver su archivo correspondiente en `/agents`.

## Cómo evoluciona este catálogo

- Si una skill deja de aplicarse a `react-base-app` (ej. por cambio de stack), se mueve a la tabla de "disponibles" o se elimina físicamente — no se deja en "activas" sin uso real.
- Si el proyecto agrega un dominio nuevo (otra base de datos, otra librería de gráficos, etc.), la skill correspondiente se agrega siguiendo la convención de abajo.

## Convención al agregar una nueva skill

1. Formato `SKILL.md` estándar (o paquete `.skill` con `SKILL.md` dentro): frontmatter con `name` y `description` (la descripción debe listar disparadores concretos — frases/acciones que activan la skill, no solo el nombre del dominio).
2. Contenido orientado a decisiones reales y errores comunes, no a documentación genérica de la librería/framework.
3. Cierra siempre con un checklist rápido aplicable al generar código.
4. Agrégala a la tabla correspondiente (activas o disponibles) y a la tabla de activación de cada agente que deba consumirla en `/agents`.
5. Si la skill introduce una convención que otras skills ya cubrían parcialmente, revisa solapamiento antes de publicarla.