---
name: vite-tanstack-tailwind
description: Guía técnica del stack Vite + React + TanStack Router/Query y API NestJS del proyecto La Racha. Úsala siempre que la tarea implique ruteo, consultas al servidor, formularios, configuración de build, o estilos del cliente web.
---

# Stack de implementación — Vite + React + TanStack

Referencia técnica del stack actual del monorepo La Racha.

## Parámetros del proyecto — La Racha

> Mantén estos valores sincronizados con el proyecto y sus archivos de contexto.

| Parámetro | Valor actual (`La Racha`) |
|---|---|
| Arquitectura | SPA Vite/React y API propia NestJS (ver `/context/project-context.md`) |
| Deploy | Pendiente; no asumir proveedor |
| Datos de servidor | TanStack Query consumiendo `/api/v1` y schemas Zod compartidos |
| Estado UI | Estado local React mientras no exista necesidad real de store global |
| Ruteo | `apps/web/src/router.tsx` con TanStack Router |
| i18n | Español base; internacionalización pendiente |
| Estilos | CSS responsivo y tokens en `/context/design-tokens.md`; preferencia del sistema |
| SEO | SPA sin SSR; no prometer indexación dinámica hasta incorporar prerender/SSR |

## 1. Build — Vite

- `npm run build --workspace @antiracha/web` produce `apps/web/dist/`. La SPA consume la API NestJS; no implementa middleware de servidor/SSR.
- No se ha configurado PWA; no asumir que hay manifest o service worker.
- No hay React Compiler; aplica memoización solo si es necesaria y justificada.
- SEO dinámico queda limitado por ser SPA sin SSR/prerender.

## 2. Ruteo — TanStack Router (`apps/web/src/router.tsx`)

- Sigue la configuración y patrones existentes de TanStack Router.
- Añade rutas al layout/navegación cuando corresponda y comprueba su acceso directo al recargar la SPA.

## 3. Datos servidor — TanStack Query y API propia

- Las solicitudes remotas usan TanStack Query y el cliente común de `apps/web/src/lib/api.ts`.
- Valida respuestas con schemas compartidos de `packages/shared-types`; no redefinas contratos en cada vista.
- Maneja errores HTTP, desconexión, respuesta inválida, loading y resultados vacíos con estados observables para el usuario.
- Cambios del contrato se coordinan entre API y web mediante `/context/handoff-protocol.md`.

## 4. Estado de interfaz

- Prefiere estado local React. No se ha adoptado Zustand ni otro store global.
- No dupliques en estado local datos cuya fuente de verdad sea TanStack Query.

## 5. Formularios — React y Zod

- Sigue los patrones de formularios existentes; valida entradas con los schemas compartidos o schemas locales cuando sean exclusivos de UI.
- Los errores deben ser accesibles y no revelar detalles internos de la API.

## 6. Estilos — CSS

- Usa las variables/tokens CSS de `apps/web/src/styles.css` y `/context/design-tokens.md`.
- Mantén layouts accesibles y responsivos; respeta `prefers-reduced-motion` y preferencias de tema existentes.

## 7. Arquitectura por capas — dónde va cada cosa

```
Ruteo (TanStack Router)
  → TanStack Query + cliente común (API NestJS)
    → schemas Zod compartidos
      → componentes y formularios accesibles
```

- La obtención de datos vive en hooks de TanStack Query, no dispersa en `useEffect` dentro de componentes de UI.
- Componentes de presentación separados del hook/contenedor que trae los datos — permite testear cada capa por separado (ver `qa-qc-react-vite`).

## 8. Checklist rápido al escribir código de este stack

- [ ] ¿Las solicitudes usan el cliente común y TanStack Query?
- [ ] ¿La respuesta se valida con el contrato compartido?
- [ ] ¿Están cubiertos los estados loading/error/vacío/éxito, no solo el happy path?
- [ ] ¿La ruta nueva está registrada en `apps/web/src/router.tsx` y navega correctamente?
- [ ] ¿Los estilos usan tokens y mantienen accesibilidad/responsividad?
- [ ] ¿Hay algún supuesto de SSR que no aplica a esta SPA?
