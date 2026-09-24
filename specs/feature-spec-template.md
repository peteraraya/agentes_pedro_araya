# Plantilla de Especificación de Feature

Usada por el `orchestrator` al iniciar una feature multi-agente, y por `designer`/`frontend` al definir el punto de partida de un handoff (ver `/context/handoff-protocol.md`). Copia esta plantilla a un nuevo archivo por feature dentro de `/specs` (ej. `/specs/checkout-flow.md`) y complétala antes de asignar trabajo a los agentes.

> Adaptado a `react-base-app`: el equipo actual es `designer`, `frontend`, `qa-tester` — **no hay agente `backend`** (SPA sin servidor propio, ver `/context/project-context.md` §4 y `/agents/agents-README.md`). La sección 4 ("Contrato de datos") es la única que cambia de dueño: en este proyecto no hay DTOs propios que definir, así que la completa `frontend` a partir del schema Zod que valida la respuesta de la API externa consumida (ver `/context/handoff-protocol.md`, handoff 2) — no se deja vacía ni se asigna a un agente que no existe en el equipo actual. Si el proyecto incorpora un backend propio en el futuro, esta sección vuelve a ser responsabilidad de `backend`.

---

## [Nombre de la feature]

**Estado**: `borrador` / `en diseño` / `en desarrollo` / `en QA` / `completa`
**Agentes involucrados**: (ej. designer, backend, frontend, qa-tester)

### 1. Problema y objetivo

- **Problema que resuelve**:
- **Usuario objetivo**:
- **Criterio de éxito** (¿cómo se sabe que funcionó?):

### 2. Alcance

**Incluye**:
-

**No incluye (fuera de alcance para esta iteración)**:
-

### 3. Especificación UX/UI (`designer`)

- **Flujo de usuario** (pasos, en orden):
- **Estados a diseñar**: default / hover / focus / active / disabled / loading / error / vacío
- **Casos de error a contemplar**:
- **Requisitos de accesibilidad específicos** (si hay algo más allá del estándar base):

### 4. Contrato de datos (`frontend` — sin backend propio, ver nota arriba)

```ts
// Schema Zod que valida la respuesta de la API externa consumida (ej. API de GitHub)
// (o "no aplica" si la feature no consume/modifica datos externos)

// Response (éxito)
// (schema Zod)

// Response (error)
// (forma del error, códigos HTTP reales de la API externa: 404, 403 rate limit, etc.)
```

- **Reglas de negocio/validación**:
- **Casos de fallo de la API externa a contemplar** (404, 403/rate limit, timeout, respuesta malformada):

### 5. Implementación de UI (`frontend`)

- **Componentes nuevos o modificados**:
- **Estado cliente necesario** (Zustand) vs. **estado servidor** (TanStack Query):
- **Dependencias de visualización** (recharts / react-github-calendar — ver skill `recharts-charts`), si aplica:

### 6. Criterios de aceptación (`qa-tester`)

Lista de condiciones verificables, no ambiguas — cada una debe poder convertirse directamente en un test:

- [ ]
- [ ]
- [ ]

**Casos negativos/límite a cubrir explícitamente**:
- [ ]
- [ ]

### 7. Decisiones registradas

Cualquier decisión tomada durante esta feature que deba persistir en `/context/project-context.md`:

-

### 8. Historial de handoffs

| Fecha | De → A | Artefacto | Notas |
|---|---|---|---|
| | | | |
