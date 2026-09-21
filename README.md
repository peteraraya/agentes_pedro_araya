# Agentes de Pedro Araya — equipo para `react-base-app`

Este repositorio define un equipo de agentes de IA especializados (orquestador, frontend, diseño, QA) para trabajar sobre **`react-base-app`**: el portafolio SPA de Pedro Araya, construido con Vite + React 19 + TanStack (Router/Query/Form) + Tailwind, sin backend propio, que consume la API pública de GitHub.

No es una plantilla genérica: cada agente, skill y documento de contexto está adaptado a este proyecto específico. Si adaptas este repo a otro proyecto, revisa primero la sección [Al adaptar este repo a otro proyecto](#al-adaptar-este-repo-a-otro-proyecto).

## Estructura del repositorio

| Carpeta | Contenido |
|---|---|
| [`/agents`](./agents/agents-README.md) | Identidad, tono, dominio técnico y autonomía de cada agente especializado (`frontend`, `designer`, `qa-tester`) |
| [`/orchestration`](./orchestration/orchestrator.md) | El agente `orchestrator` (clasifica solicitudes y coordina el flujo entre agentes) y el runbook de incidentes |
| [`/roles`](./roles/roles-matrix.md) | Matriz de responsabilidades — qué agente resuelve qué tipo de solicitud |
| [`/skills`](./skills/skills-README.md) | Conocimiento especializado por dominio (`.skill`, paquetes con `SKILL.md`) que los agentes consultan de forma autónoma |
| [`/specs`](./specs/feature-spec-template.md) | Plantillas y especificaciones reales de features, con criterios de aceptación e historial de handoffs |
| `/context` (`files.zip`) | Contexto vivo del proyecto: convenciones, tokens de diseño, protocolo de handoff, Definition of Done y convención de PR/commits |

## Cómo se relacionan las piezas

1. El usuario le pide algo a `orchestrator`, quien clasifica la solicitud y decide qué agente(s) de `/agents` la resuelven, en qué secuencia (ver `/roles/roles-matrix.md` para la matriz de decisión rápida).
2. Cada agente consulta de forma autónoma las skills de `/skills` que su tarea activa (cada agente declara su propia tabla de activación) y siempre revisa `/context` antes de empezar — el contexto del proyecto tiene prioridad sobre cualquier guía genérica.
3. El trabajo entre agentes se pasa mediante un handoff explícito (formato en `/context/handoff-protocol.md`), nunca de forma implícita o resumida.
4. Una feature se marca `completa` en `/specs` solo cuando cumple el checklist de `/context/definition-of-done.md`.
5. Un incidente en producción (sitio caído, vulnerabilidad activa) suspende el flujo normal y sigue `/orchestration/incident-runbook.md`.

## Empezar a usar los agentes

Este repositorio se consume desde un cliente/orquestador de agentes de IA (ej. Claude Code) que apunte al directorio del repo. La convención esperada es:

1. Clona este repositorio junto al proyecto (`react-base-app`) que va a trabajar.
2. Configura tu cliente de agentes para que cargue los archivos de `/agents` como definiciones de agente, y para que cada uno tenga acceso de lectura a `/skills`, `/context` y `/specs`.
3. Dirige las solicitudes a `orchestrator.md` cuando no estén claramente acotadas a un solo dominio; invoca un agente de `/agents` directamente cuando el dominio es obvio (ver la tabla en `/agents/agents-README.md`).

## Al adaptar este repo a otro proyecto

Este equipo está fuertemente acoplado a `react-base-app` (stack, paleta de color, ausencia de backend). Si lo adaptas a otro proyecto:

- Actualiza `/context` (project-context, design-tokens, handoff-protocol, definition-of-done, pr-convention) con el stack y convenciones reales del nuevo proyecto — no dejes referencias a TanStack/Tailwind/GitHub API si no aplican.
- Revisa `/skills`: elimina las skills que no correspondan al nuevo stack y agrega las que falten, siguiendo la convención documentada en `skills/skills-README.md`. No dejes una skill listada como "activa" que no exista físicamente en la carpeta, ni una skill presente que ningún agente use — ambas son la fuente de inconsistencia más común en este tipo de repos.
- Si el nuevo proyecto sí tiene backend propio, agrega `agents/backend.md` siguiendo la convención de `agents/agents-README.md`, y revierte las notas de adaptación en `/specs` y `/context/handoff-protocol.md` que asumen que no existe.
- Actualiza este README con la descripción real del nuevo proyecto.

## Licencia

Todos los archivos en este repositorio están bajo la licencia [MIT](https://opensource.org/licenses/MIT).

## Autor y contacto

El autor del repositorio es [Pedro Araya Gálvez](https://github.com/peteraraya).
