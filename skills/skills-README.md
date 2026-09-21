# `/skills` — Índice de habilidades especializadas

Cada archivo `SKILL.md` en esta carpeta documenta el estándar de un dominio técnico específico: cuándo usar qué herramienta, errores comunes reales (no solo "cómo se usa" superficial), y un checklist rápido al generar código. Los agentes en `/agents` las consultan de forma autónoma cuando una tarea las activa — no hace falta pedirlo explícitamente.

## Nota de sincronización (2026-09-21)

Esta carpeta contiene **dos grupos de skills**, y es importante no confundirlos:

1. **Activas para este equipo** (6): las que los agentes de `/agents` (`frontend`, `designer`, `qa-tester`, `orchestrator`) consultan realmente para trabajar en `react-base-app`. Ver tabla §1.
2. **Sincronizadas desde la cuenta de Claude del usuario, presentes pero no consumidas por ningún agente de este equipo** (24): el resto de las skills de marketplace de la cuenta (`anthropic-skills:*`), copiadas acá a pedido explícito para tener el inventario completo en el repo. Ningún agente de `/agents` las referencia en su tabla de activación — si una tarea futura las necesita, se agregan a la tabla de activación del agente correspondiente recién en ese momento, no antes.

Que una skill esté en el grupo 2 **no es un error** — a diferencia de la inconsistencia anterior (skills listadas como "activas" que no existían como archivo), acá el archivo sí existe, solo que ningún agente lo consume todavía. La inconsistencia a evitar sigue siendo la misma: nunca declarar una skill "activa"/consumida en una tabla de `/agents` sin que el archivo exista, y nunca dejar un archivo huérfano que un agente cita como si fuera propio sin estarlo.

## 1. Skills activas en este proyecto (consumidas por `/agents`)

| Skill | Dominio | Consumida principalmente por |
|---|---|---|
| `frontend-design` | Dirección visual, tipografía y estilo intencional; sistema blue de marca (ver `/context/design-tokens.md`) | `frontend`, `designer` |
| `vite-tanstack-tailwind` | Vite + React 19 + TanStack Router/Query/Form + Tailwind | `frontend`, `designer` (restricciones de implementación) |
| `qa-qc-react-vite` | Estrategia de testing (Vitest/RTL/MSW + jsdom) | `qa-tester`, `frontend` |
| `cicd-expert-pipelines` | Pipelines de CI/CD (GitHub Actions / Vercel), PWA build | `frontend`, `orchestrator`, `qa-tester` |
| `recharts-charts` | Gráficos con recharts y react-github-calendar — tema, accesibilidad | `frontend`, `designer`, `qa-tester` |
| `ui-design-system` | Tokens de color blue, componentes UI, estados y contraste WCAG AA | `designer`, `frontend`, `qa-tester` |

Si una tabla de activación en `/agents` menciona una skill que no está en esta lista como archivo real de esta carpeta, es un error a corregir.

## 2. Skills de la cuenta, sincronizadas pero no usadas por este equipo (24)

Copiadas tal cual desde la cuenta de Claude del usuario (paquete `anthropic-skills`) para tener el inventario completo en el repo. Ninguna tiene fila en la tabla de activación de `frontend`/`designer`/`qa-tester`/`orchestrator` — no aplican al stack de `react-base-app` (SPA sin backend propio) o cubren un dominio que este equipo no atiende hoy (Word/Excel/PDF, Kubernetes, NestJS, mapas, otras librerías de gráficos, etc.).

| Skill | Dominio |
|---|---|
| `devops-docker-kubernetes` | Docker/Kubernetes de producción |
| `docs` | Documentos vivos (Claude Docs) |
| `docx` | Crear/editar Word (.docx) |
| `import-memory` | Importar memoria de otro asistente |
| `kubernetes-cluster-admin` | Administración de clusters K8s |
| `leaflet-maps-integration` | Mapas interactivos con Leaflet/react-leaflet |
| `mongodb-expert-database` | Modelado, índices y agregaciones en MongoDB |
| `morning` | Brief matutino en artifact HTML |
| `nestjs-clean-code-expert` | Clean code aplicado a NestJS |
| `nestjs-secure-backend` | Arquitectura y seguridad de backends NestJS |
| `nextjs-2026-best-practices` | Next.js 16 / App Router / Cache Components |
| `nginx-expert-config` | Reverse proxy, TLS y caching con Nginx |
| `nivo-professional-charts` | Gráficos con Nivo (React/Next.js) |
| `pdf` | Leer, crear y editar PDF |
| `plotly-expert-charts` | Gráficos interactivos con Plotly.js |
| `pptx` | Crear/editar PowerPoint (.pptx) |
| `project-guidelines` | Lineamientos de `react-base-app` y `my-forge-app` — **ver advertencia abajo** |
| `prueba-tecnica-react` | Ejercicios de prueba técnica React/Full Stack |
| `qa-qc-react-nestjs` | Testing para stacks React + NestJS |
| `security-modularization-architect` | Threat modeling y límites de módulo en NestJS |
| `skill-creator` | Crear/mejorar/evaluar skills |
| `sonarqube-code-quality` | SonarQube/SonarCloud, Quality Gates |
| `xlsx` | Crear/editar hojas de cálculo (.xlsx/.csv) |

### ⚠️ Advertencia sobre `project-guidelines`

Esta skill trae una referencia (`references/react-base-app.md`) que describe **una arquitectura de `react-base-app` distinta e incompatible** con la documentada en `/context/project-context.md` de este repo: menciona Axios como HTTP client, autenticación con `authStore`/rutas protegidas, y una carpeta `src/features/` con CRUD de "products" — nada de eso existe en el proyecto real que este equipo de agentes atiende (SPA de portafolio, sin backend propio, sin auth, solo consumo de la API pública de GitHub). Es una contradicción real sin resolver, no un error de este repo: **no se consume esta skill para trabajar en `react-base-app`** hasta que el usuario confirme cuál de las dos versiones es la vigente y se actualice la que quedó desactualizada.

## 3. Skills nativas de Claude Code que no se pudieron sincronizar como archivo

Además de las anteriores, la cuenta tiene skills nativas del propio Claude Code (`dataviz`, `artifact-design`, `artifact-diagramming`, `artifact-capabilities`, `update-config`, `keybindings-help`, `code-review`, `simplify`, `fewer-permission-prompts`, `loop`, `claude-api`, `run`, `init`, `security-review`). Son capacidades del producto/CLI, no paquetes `SKILL.md` con un archivo fuente accesible en disco — no se pueden copiar a esta carpeta como `.skill`. `session-start-hook` es la única skill nativa con archivo fuente real (`/root/.claude/skills/session-start-hook/SKILL.md`), por eso sí está sincronizada acá.

## Cómo se activan

Cada agente en `/agents` tiene su propia tabla de activación que mapea skills a disparadores concretos — solo las 6 skills de §1 aparecen en esas tablas hoy. Para el detalle de activación de cada agente, ver su archivo correspondiente en `/agents`.

## Convención al agregar una nueva skill activa

1. Formato `SKILL.md` estándar (o paquete `.skill` con `SKILL.md` dentro): frontmatter con `name` y `description` (la descripción debe listar disparadores concretos — frases/acciones que activan la skill, no solo el nombre del dominio).
2. Contenido orientado a decisiones reales y errores comunes, no a documentación genérica de la librería/framework.
3. Cierra siempre con un checklist rápido aplicable al generar código.
4. Agrégala a la tabla §1 de este archivo **y** a la tabla de activación de cada agente que deba consumirla en `/agents` — una skill que solo está en una de las dos tablas es la inconsistencia más común de esta carpeta.
5. Si la skill introduce una convención que otra ya cubría parcialmente, revisa solapamiento antes de publicarla.
6. Si viene de la cuenta de Claude (§2) y pasa a ser activa, muévela de la tabla §2 a la §1 en este mismo commit.
