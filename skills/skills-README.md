# `/skills` — Índice de habilidades especializadas

Cada archivo `SKILL.md` en esta carpeta documenta el estándar del equipo para un dominio técnico específico: cuándo usar qué herramienta, errores comunes reales (no solo "cómo se usa" superficial), y un checklist rápido al generar código. Los agentes en `/agents` las consultan de forma autónoma cuando una tarea las activa — no hace falta pedirlo explícitamente.

## Nota de realineación (2026-09-21)

Este proyecto **react-base-app** es un SPA (Vite + React + TanStack Router + Tailwind), **no** un proyecto Next.js/NestJS. Las skills de la plantilla original orientadas a un stack distinto (NestJS/Next.js/Nivo/Plotly/Leaflet/Kubernetes) se **eliminaron** de esta carpeta — no basta con marcarlas "no aplica" y dejarlas presentes, porque un agente puede terminar consultando un `.skill` que sigue ahí. Las 6 skills de la tabla siguiente son las únicas que existen físicamente en `/skills` y las únicas que los agentes deben consultar.

## Skills disponibles (activas en este proyecto)

| Skill | Dominio | Consumida principalmente por |
|---|---|---|
| `frontend-design` | Dirección visual, tipografía y estilo intencional; sistema blue de marca (ver `/context/design-tokens.md`) | `frontend`, `designer` |
| `vite-tanstack-tailwind` | Vite + React 19 + TanStack Router/Query/Form + Tailwind | `frontend`, `designer` (restricciones de implementación) |
| `qa-qc-react-vite` | Estrategia de testing (Vitest/RTL/MSW + jsdom) | `qa-tester`, `frontend` |
| `cicd-expert-pipelines` | Pipelines de CI/CD (GitHub Actions / Vercel), PWA build | `frontend`, `orchestrator`, `qa-tester` |
| `recharts-charts` | Gráficos con recharts y react-github-calendar — tema, accesibilidad | `frontend`, `designer`, `qa-tester` |
| `ui-design-system` | Tokens de color blue, componentes UI, estados y contraste WCAG AA | `designer`, `frontend`, `qa-tester` |

Cada una es un paquete `.skill` (zip con `<nombre>/SKILL.md` dentro) en esta misma carpeta — si una tabla de activación en `/agents` menciona una skill que no está en esta lista, es un error a corregir, no una skill "implícita".

## Si el proyecto cambia de arquitectura

Si en el futuro el proyecto incorpora un backend propio, Next.js, u otras librerías de gráficos/mapas, la skill correspondiente se agrega de nuevo siguiendo la convención de abajo — no se reintroducen las skills eliminadas "por si acaso" sin una necesidad real del proyecto.

## Cómo se activan

Cada agente en `/agents` tiene su propia tabla de activación que mapea skills a disparadores concretos. Esta tabla es la vista global; para el detalle de activación de cada agente, ver su archivo correspondiente en `/agents`.

## Convención al agregar una nueva skill

1. Formato `SKILL.md` estándar (o paquete `.skill` con `SKILL.md` dentro): frontmatter con `name` y `description` (la descripción debe listar disparadores concretos — frases/acciones que activan la skill, no solo el nombre del dominio).
2. Contenido orientado a decisiones reales y errores comunes, no a documentación genérica de la librería/framework.
3. Cierra siempre con un checklist rápido aplicable al generar código.
4. Agrégala a esta tabla y a la tabla de activación de cada agente que deba consumirla en `/agents`.
5. Si la skill introduce una convención que otras skills ya cubrían parcialmente, revisa solapamiento antes de publicarla.
