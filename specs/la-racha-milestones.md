# Hitos de racha — La Racha

**Estado**: `completa`  
**Agentes involucrados**: backend, frontend, qa-tester

## 1. Problema y objetivo

- **Problema que resuelve**: los usuarios no tienen una forma visual de celebrar hitos de constancia en su búsqueda activa (30, 60, 90 días). La racha solo se muestra como un número de días sin hitos intermedios.
- **Usuario objetivo**: cualquier candidato con una racha activa que quiera ver su progreso marcado por hitos significativos.
- **Criterio de éxito**: el dashboard muestra tarjetas de hito con estado "alcanzado" o "pendiente", días restantes, y un tono empático que nunca convierte el desempleo en competición.

## 2. Alcance

**Incluye**:
- Endpoint `GET /me/streak/milestones` (Bearer JWT) que devuelve `currentDays` y los 3 hitos (30, 60, 90 días) con `reached` y `daysRemaining`.
- Schemas Zod `MilestoneSchema` y `MilestoneListSchema` en `packages/shared-types`.
- Sección "Tus hitos" en `DashboardPage.tsx` con estados loading, error, vacío y éxito.
- Estilos CSS con tokens del producto (flame, mint, sunshine, ink, muted, surface).
- Tests de unidad del servicio backend y de validación Zod en shared-types.

**No incluye (fuera de alcance para esta iteración)**:
- Notificaciones push/email al alcanzar un hito.
- Hitos personalizados por el usuario.
- Compartir hitos en redes sociales.

## 3. Especificación UX/UI (designer)

- **Flujo de usuario**: el usuario abre su dashboard → ve su número actual de días → ve 3 tarjetas de hito (30/60/90 días) con estado alcanzado o pendiente.
- **Estados a diseñar**: default, loading, error, vacío (sin racha activa), éxito.
- **Casos de error a contemplar**: API no disponible, token expirado, sin racha activa.
- **Requisitos de accesibilidad**: foco visible en todos los controles, contraste WCAG AA verificado, información crítica no depende solo del color (se usa texto + color), estados announciados para lectores de pantalla.

## 4. Contrato de datos (backend)

Endpoint: `GET /me/streak/milestones`
- Autenticación: Bearer JWT requerido
- Response 200: `{ currentDays: number, items: Milestone[] }`
- `Milestone = { days: number, title: string, description: string, reached: boolean, daysRemaining: number }`
- Response 401: token faltante o expirado
- Documentado en `/specs/api/la-racha.md`

## 5. Implementación de UI (frontend)

- **Componentes nuevos o modificados**: `DashboardPage.tsx` (sección de hitos), `styles.css` (estilos de `.milestone-*`).
- **Estado cliente necesario**: ninguno adicional (TanStack Query para estado servidor).
- **Consumo del contrato**: `api.getMilestones()` valida con `MilestoneListSchema` de `@antiracha/shared-types`.
- **Dependencias de visualización**: ninguna (usar tokens existentes).

## 6. Criterios de aceptación (qa-tester)

- [ ] `GET /me/streak/milestones` devuelve 200 con `currentDays` y 3 items cuando hay racha activa.
- [ ] `GET /me/streak/milestones` devuelve `currentDays: 0` y items con `reached: false` cuando no hay racha activa.
- [ ] `GET /me/streak/milestones` sin token devuelve 401.
- [ ] `MilestoneSchema` rechaza `days` negativo y campos requeridos faltantes.
- [ ] `MilestoneListSchema` requiere `currentDays`.
- [ ] Dashboard renderiza sección de hitos con estados loading, error, vacío y éxito.
- [ ] Los hitos alcanzados se muestran con estilo visual distinto (mint) y los pendientes con sunshine.
- [ ] El texto nunca comunica el desempleo como competición (tono empático verificado).

**Casos negativos/límite a cubrir explícitamente**:
- [ ] Usuario sin racha activa (ver tabla de arriba).
- [ ] Días exactamente en el límite (30, 60, 90) → `reached: true`.
- [ ] `daysRemaining` nunca negativo (usa `Math.max(0, ...)`).

## 7. Decisiones registradas

- Los hitos son fijos (30, 60, 90 días) y definidos como constantes compartidas en `packages/shared-types` (`MILESTONES`) para que backend y frontend usen los mismos textos.
- `currentDays` se calcula en backend con `getStreakDays` existente; no se almacena como contador mutable.

## 8. Historial de handoffs

| Fecha | De → A | Artefacto | Notas |
|---|---|---|---|
| 2026-03-11 | backend → frontend | Contrato `GET /me/streak/milestones` + schemas Zod | Endpoint documentado en `specs/api/la-racha.md` |
| 2026-03-11 | frontend → qa-tester | UI en `DashboardPage.tsx` + estilos CSS | Tests de unidad y validación Zod incluidos |
