# Contexto del Proyecto

Este archivo es la **fuente única de verdad** que todos los agentes (`/agents`) consultan antes de trabajar, y donde el orquestador (`/orchestration`) registra decisiones reutilizables. Se actualiza a medida que el proyecto evoluciona — no es un documento estático de kickoff.

## 1. Visión general del producto

- **Nombre del producto**: react-base-app (portafolio personal)
- **Descripción en una línea**: Portafolio SPA de Pedro Araya — desarrollador full-stack — con CV interactivo, contacto, proyectos, recursos y utilidades (analizador de vacantes, API docs, calc. de ROI).
- **Usuario objetivo**: Reclutadores, desarrolladores y personas que quieren evaluar el perfil y experiencia de Pedro.
- **Problema que resuelve**: Presentar un CV/carrera de forma interactiva y memorable, con demostraciones de skill técnicas reales embebidas.

## 2. Stack técnico confirmado

| Capa | Tecnología | Notas |
|---|---|---|
| Build | Vite 6 + esbuild | SPA estática, deploy en Vercel/PWA |
| Frontend | React 19 + TypeScript estricto | Sin Next.js, sin Server Components |
| Router | TanStack Router (`src/app/router.tsx`) | Rutas `createRoute`/`createRootRoute` — ver skill `vite-tanstack-tailwind` |
| Datos cliente | TanStack Query (+ API de GitHub) | Estado servidor en cliente; sin backend propio |
| Estado UI | Zustand (`src/stores/uiStore`) | Solo estado de UI puro |
| Formularios | TanStack Form + @tanstack/zod-form-adapter | Validación con Zod |
| Validación | Zod | Contrato de datos, incluida la respuesta de la API de GitHub |
| Estilos | Tailwind CSS 3 | Tokens en `/context/design-tokens.md` — ver skill `ui-design-system` |
| Tipografía | Newsreader (display), IBM Plex Mono (mono) | Fuentes via fontsource |
| Animaciones | Framer Motion + CSS | `motion.div`, `AnimatePresence` |
| Visualización | recharts, react-github-calendar | Ver skill `recharts-charts` |
| 3D | react-three-fiber + drei + three | Elementos decorativos |
| Testing | Vitest + RTL + MSW + jsdom + Playwright (E2E acotado) | Ver skill `qa-qc-react-vite` |
| i18n | i18next + react-i18next | es/en |
| CI/CD | GitHub Actions / Vercel | Ver skill `cicd-expert-pipelines` |

## 3. Convenciones del equipo

- **Nomenclatura de ramas**: `main` protegida; `feature/<slug>`, `fix/<slug>`, `chore/<slug>`. Ver `/context/pr-convention.md`.
- **Formato de commits**: Conventional Commits. Ver `/context/pr-convention.md`.
- **Definition of Done**: checklist de cierre por tarea. Ver `/context/definition-of-done.md`.
- **Estructura de carpetas del repo**:
  - `src/app/` — router y layout raíz
  - `src/pages/` — páginas por ruta (una `*.tsx` por ruta)
  - `src/components/` — `animations/`, `cv/`, `feedback/`, `navigation/`, `ui/`
  - `src/lib/` — utilidades, api client, i18n
  - `src/data/`, `src/stores/`, `src/types/`, `src/hooks/`, `src/styles/`
- **Idioma del código**: inglés (variables, funciones, commits) / español (comunicación con el usuario y documentación de negocio).

## 4. Restricciones de negocio conocidas

- El azul es el color de marca único. Todo acento decorativo usa la paleta `blue` (ver `/context/design-tokens.md`). Colores semánticos solo para: `green` (éxito/online/completado), `red` (error/peligro), `amber/orange` (advertencia/en-progreso), y los mockups de código/sintaxis conservan su resaltado nativo.
- No hay backend propio; el único consumo externo es la API pública de GitHub (perfil/stars). No inventar endpoints propios — el equipo de agentes es `designer`, `frontend`, `qa-tester` (sin agente `backend`, ver `/agents/README.md`).

## 5. Decisiones de arquitectura registradas

El orquestador agrega una entrada aquí cada vez que una tarea produce una decisión reutilizable (patrón adoptado, convención nueva, restricción técnica descubierta). Formato:

```
### [Fecha] Título de la decisión
**Contexto**: por qué surgió
**Decisión**: qué se resolvió
**Agentes afectados**: cuáles deben respetarla
```

### [2026-08-30] Sistema de diseño blue unificado
**Contexto**: el portafolio mostraba acentos multi-color (purple, indigo, emerald, amber) y el fondo gris tenía subtonos cálidos; se pidió un azul profesional coherente en toda la app.
**Decisión**: el color de marca es la paleta `blue` de `tailwind.config.js` en toda la UI. Los fondos/cards usan la escala `gray` fría neutra. Se conservan los colores semánticos y de sintaxis de código. Documentado en `/context/design-tokens.md`.
**Agentes afectados**: frontend, designer, qa-tester.

### [2026-08-30] Feature Career Timeline (`/career`)
**Contexto**: se pidió una línea de tiempo interactiva de la trayectoria siguiendo el flujo `.opencode`.
**Decisión**: nueva ruta `/career` (`CareerPage`) + componente `CareerTimeline` + helper puro `careerTimeline.ts` (deriva `TimelineEntry` desde `cv.experience`/`cv.education`, ordena por heurística de año y filtra por categoría). Se usa estado local `useState` para el filtro (sin Zustand ni TanStack Query: datos locales síncronos).
**Decisiones de detalle**:
- Nodos de educación usan variante neutra azulada (`slate/gray`) con ícono `GraduationCap`; los de experiencia usan `blue` + `Briefcase`. Se mantiene la regla "acentos decorativos solo blue", diferenciando por forma de ícono.
- Deuda técnica: `period` en `cv.ts` es texto libre; el ordenamiento usa el primer `\d{4}` como heurística (fallback 0). Si en el futuro se estructura la fecha, `buildTimeline`/`sortByPeriodDesc` deben actualizarse en un solo lugar.
- Convención de prueba: tests de JSX usan el docblock `// @vitest-environment jsdom` y mockean `react-i18next` (evita depender del init async de i18next en tests).
**Agentes afectados**: designer, frontend, qa-tester.

## 6. Glosario del dominio

Términos específicos del negocio/producto que no son obvios desde el código. Evita que cada agente interprete un concepto de forma distinta.

- **CV Sections**: secciones del portafolio que renderizan contenido del CV (`HeroSection`, `SkillsSection`, `ImpactMetrics`, etc.) dentro de `src/components/cv/`.
- **Widgets**: componentes de utilidad (`ROICalculator`, `HireMeKanban`, `AIChatWidget`, `CommandPalette`...).
- **recruiterMode / vscodeMode**: modos de presentación togglables desde la UI (modo TL;DR reclutador, modo IDE VS Code).
- **Easter eggs**: comportamientos lúdicos en consola/UI (`useEasterEgg.ts`).

## 7. Proyectos activos

- `react-base-app` — este portafolio (repo actual).

Referencias a otros proyectos de Pedro que aparecen como contenido/CV se manejan como datos en `src/data/cv.ts`, no como repos a modificar.
