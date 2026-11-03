---
name: qa-qc-react-vite
description: Estrategia de testing del monorepo Rompe la Racha: Vitest, React Testing Library para Vite/React y tests unitarios/integración de NestJS y contratos compartidos. Úsala al escribir o revisar tests, definir cobertura o estrategia de testing, o cuando se pida evitar regresiones.
---

# Testing — Web Vite/React (Vitest + RTL)

Estrategia de testing para el monorepo Rompe la Racha: web Vite/React, API propia NestJS y contratos Zod compartidos. Prioriza pruebas unitarias e integración deterministas; usa E2E para flujos críticos cuando la infraestructura esté disponible.

## Parámetros del proyecto — Rompe la Racha

> Los únicos valores específicos de esta skill. Al adaptar a otro proyecto se editan estos parámetros (y los archivos de `/context` que referencian), no el cuerpo de la skill.

| Parámetro | Valor actual (`Rompe la Racha`) |
|---|---|
| Aplicación | Monorepo npm: Vite/React, NestJS, Prisma y PostgreSQL |
| Contrato | Schemas Zod de `packages/shared-types` |
| Flujos críticos | Registro/verificación, autenticación, privacidad, edición de perfil y rachas |
| Dependencia externa | Servicio de email; debe mockearse en pruebas |

## 1. Pirámide de testing de este proyecto

```
       E2E (Playwright)         ← acotado a los flujos críticos (ver Parámetros)
   Integración (RTL + mocks)    ← la mayoría de la cobertura útil vive acá
Unitarios (Vitest, funciones puras)  ← helpers, schemas Zod, lógica de derivación
```

- E2E se justifica en el puñado de flujos donde el costo de un fallo real (frente a la audiencia real del producto) es alto — no como cobertura por defecto de cada pantalla.
- Un helper puro (ver ejemplos en Parámetros) se testea unitariamente sin montar ningún componente — es más rápido y señala exactamente dónde está el bug si falla.

## 2. Pruebas de API y servicios

- No accedas a servicios externos en pruebas unitarias/integración; mockea email y dependencias de infraestructura con los patrones Nest existentes.
- Comprueba permisos, validación, respuestas 4xx/5xx y filtrado de datos privados, además del caso feliz.
- Los mocks de contrato deben ajustarse a `packages/shared-types`; evita mantener formas incompatibles por app.

## 3. Qué testear (comportamiento observable)

- Testea lo que el usuario/consumidor experimenta (texto renderizado, estado del DOM accesible por rol), no detalles de implementación interna (nombre de variable de estado, estructura interna de un hook) — un test acoplado a implementación se rompe con cualquier refactor sin proteger nada real.
- Selectores: prioriza `getByRole`/`getByLabelText` sobre `getByTestId` — si un componente solo es testeable con `data-testid`, es una señal de que tampoco es accesible por lectores de pantalla/teclado.
- Estados de UI completos en cada componente relevante: loading, error, vacío, éxito — nunca solo el estado por defecto.

## 4. Setup técnico (Vitest + RTL + jsdom)

- Entorno `jsdom` explícito en tests de componentes (`// @vitest-environment jsdom` si el proyecto no lo fija global).
- Mockea las dependencias externas y mantén fixtures alineadas con los contratos compartidos.
- Al testear componentes que usan TanStack Router/Query, usa el setup de render con providers reales (`createRouter` de test, `QueryClientProvider` con un `QueryClient` nuevo por test) — no asumas comportamiento de router/query sin el provider correspondiente.
- Sin `setTimeout` real, sin fechas/horas sin mockear, sin estado compartido entre tests sin limpiar (`afterEach` con cleanup) — estas son las causas más comunes de flaky tests en este stack.

## 5. Un test flaky es un bug

- Si un test falla intermitentemente sin cambios de código, se investiga la causa raíz (timing, orden de tests, mock de red mal configurado) — nunca se resuelve reintentando ni se silencia con `skip`.
- La causa más común: mocks no limpiados entre tests o dependencias asíncronas que introducen timing real.

## 6. Cobertura con criterio

- La cobertura es una guía, no una meta — `expect(result).toBeDefined()` no prueba nada útil; verifica el valor real esperado.
- Cobertura de diff razonable en código nuevo/modificado, no perseguir 100% global.
- Regresión: todo bug reportado y corregido lleva un test que fallaba antes del fix y pasa después.

## 7. E2E con Playwright — cuándo sí

- Reservado a los flujos donde el fallo real importa (ver Parámetros). No se exige por defecto en cada cambio de UI — ver `/context/definition-of-done.md`.

## 8. Checklist rápido al escribir/revisar tests

- [ ] ¿Están cubiertos errores, permisos y datos inválidos además del caso feliz?
- [ ] ¿Los datos de prueba respetan los schemas compartidos?
- [ ] ¿Se testea comportamiento observable (`getByRole`) y no implementación interna?
- [ ] ¿Están cubiertos loading/error/vacío/éxito en componentes con fetch?
- [ ] ¿Hay determinismo — sin `setTimeout` real, sin fechas sin mockear, sin estado compartido entre tests?
- [ ] ¿Un bug reportado tiene su test de regresión correspondiente?
- [ ] ¿El E2E (si existe) está reservado a un flujo crítico real, no a cobertura genérica de pantalla?
