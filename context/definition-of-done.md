# Definition of Done

Checklist único que el `orchestrator` usa para cerrar una feature o tarea como completa. Ninguna feature se marca `completa` en `/specs/[feature].md` si no cumple lo aplicable a su alcance. No todo ítem aplica a toda tarea (un fix puntual no pasa por `designer`), pero cuando aplica, es obligatorio — no una sugerencia.

> Adaptado a La Racha: frontend SPA con Vite + React + TanStack y backend NestJS/PostgreSQL con API propia (ver `/context/project-context.md`). Aplica las secciones que correspondan al alcance.

## Datos y servicios externos (si aplica)

- [ ] Toda respuesta externa se valida con un schema Zod propio antes de usarse — nunca se confía en el shape devuelto sin parsear
- [ ] Casos de fallo contemplados explícitamente: errores HTTP, timeout y respuesta malformada
- [ ] Las solicitudes del cliente web usan TanStack Query y el cliente común
- [ ] Sin llamadas reales a servicios externos en pruebas automatizadas

## Backend propio — API NestJS (si la tarea tocó backend)

- [ ] DTOs con validación estricta de entrada (`whitelist: true`, `forbidNonWhitelisted: true`) — nunca se confía en el shape de entrada sin validar
- [ ] Autenticación y autorización separadas (guards de identidad vs. rol/ownership); rate limiting en endpoints sensibles
- [ ] Manejo de errores que no filtra detalles internos (stack traces, mensajes del motor de DB) al cliente
- [ ] Queries parametrizadas — sin concatenación de strings SQL; transacciones explícitas en operaciones multi-paso atómicas
- [ ] Migraciones versionadas y revisadas; cambios destructivos con plan de reversión
- [ ] Sin `any` implícito ni explícito sin justificación documentada
- [ ] Contrato de la API documentado/actualizado en `/specs/api/` y sincronizado con `packages/shared-types`
- [ ] Tests de integración/e2e con Supertest de los endpoints nuevos, con dependencias externas mockeadas (sin red real)

## Frontend (si la tarea tocó UI)

- [ ] Estados completos implementados: loading, error, vacío, éxito (no solo el happy path)
- [ ] Code splitting aplicado a rutas/componentes pesados cuando corresponda
- [ ] Sin `any` implícito ni explícito sin justificación documentada
- [ ] Textos y estados están en español y no afirman internacionalización aún no implementada
- [ ] Datos del servidor no se duplican innecesariamente en estado local

## Diseño (si la tarea tocó UX/UI)

- [ ] Contraste WCAG AA verificado, no asumido
- [ ] Todo elemento interactivo alcanzable por teclado con foco visible
- [ ] Ningún estado/información crítica comunicada solo por color
- [ ] Se usan los tokens de La Racha definidos en `/context/design-tokens.md`; ningún dato crítico depende solo del color
- [ ] Especificación con valores exactos (no "un poco más de espacio") entregada a `frontend`

## Testing (siempre, salvo excepción explícita del usuario)

- [ ] Casos negativos cubiertos: errores de API, respuestas inválidas, permisos, timeout y límites de input
- [ ] Test de regresión si la tarea se originó en un bug reportado
- [ ] Sin tests flaky introducidos (determinismo verificado, sin `setTimeout` real, sin llamadas reales a red)
- [ ] Cobertura de diff razonable en el código nuevo/modificado — no perseguir 100% global
- [ ] E2E (Playwright) solo si la tarea toca uno de los flujos críticos del producto — no se exige por defecto en cada cambio de UI

## CI/CD y build (si la tarea afecta despliegue)

- [ ] Pipeline pasa lint → typecheck → tests → build sin pasos salteados
- [ ] Build de web (`npm run build --workspace @antiracha/web`) completa sin warnings de bundle size no revisados
- [ ] Sin secretos en texto plano en el repo (tokens de API, variables de entorno)
- [ ] Preview deployment revisado si existe un entorno de despliegue configurado

## Contexto y documentación

- [ ] Decisiones reutilizables registradas en `/context/project-context.md` (sección "Decisiones de arquitectura registradas")
- [ ] Si la tarea introdujo una convención nueva (nomenclatura, patrón adoptado), documentada donde el resto del equipo la va a encontrar, no solo mencionada en la conversación
- [ ] Handoffs entre agentes documentados según `/context/handoff-protocol.md` si la tarea fue multi-agente

## Cierre

- [ ] El `orchestrator` confirma que cada agente involucrado dio su artefacto por completo (no "a medias, se termina después" sin que quede declarado explícitamente como pendiente)
- [ ] Si algún ítem aplicable no se cumplió, se declara explícitamente como deuda técnica conocida — nunca se omite en silencio
