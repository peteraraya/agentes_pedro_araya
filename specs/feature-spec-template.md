# Plantilla de Especificación de Feature

Usada por el `orchestrator` al iniciar una feature multi-agente, y por `designer`/`frontend` al definir el punto de partida de un handoff (ver `/context/handoff-protocol.md`). Copia esta plantilla a un nuevo archivo por feature dentro de `/specs` (ej. `/specs/checkout-flow.md`) y complétala antes de asignar trabajo a los agentes.

> Para La Racha, consulta `/context/project-context.md`. Cuando una feature añade o modifica datos propios, `backend` define el contrato de `/api/v1` y lo documenta en `/specs/api/`; `frontend` consume los schemas compartidos de `packages/shared-types`. Si no hay datos/API, indícalo expresamente.

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

### 4. Contrato de datos (`backend` cuando aplique)

```ts
// API propia: DTOs con validación, endpoints, request/response tipados.
// Especificación completa en /specs/api/.

// Response (éxito)
// (schema/type)

// Response (error)
// (forma del error y códigos HTTP aplicables)
```

- **Reglas de negocio/validación**:
- **Casos de fallo de la API o dependencias a contemplar** (errores HTTP, timeout, respuesta inválida):

### 5. Implementación de UI (`frontend`)

- **Componentes nuevos o modificados**:
- **Estado cliente necesario** vs. **estado servidor** (TanStack Query):
- **Consumo del contrato**: si la feature usa la API propia, `frontend` consume el contrato que `backend` publicó (handoff `backend → frontend`), no inventa un shape distinto.
- **Dependencias de visualización**, si aplica:

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
