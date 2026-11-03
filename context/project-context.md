# Contexto del proyecto

Fuente única de verdad para el producto Rompe la Racha. Si otra guía del equipo contradice este archivo, prevalece este contexto.

## 1. Producto

- **Nombre**: Rompe la Racha.
- **Propuesta**: ayudar a que el talento en búsqueda activa sea visible y acompañar el proceso personal sin prometer empleo.
- **Audiencia**: candidatos y reclutadores en Chile y Latinoamérica.
- **Principio central**: celebrar la constancia sin estigmatizar el desempleo. Usar “días de búsqueda activa”; presentar cada dato público como autodeclarado.
- **Privacidad**: perfiles privados por defecto, consentimiento explícito y granular para publicar y aparecer en el ranking, seudónimo disponible, sin publicar correo ni teléfono. Ocultar o eliminar la cuenta debe ser una acción clara.
- **Edad mínima**: 18 años, declarativa.

## 2. Alcance actual y stack

El alcance de implementación inicial es la Fase 1 (MVP) del plan de producto Rompe la Racha.

| Capa | Tecnología | Convención |
|---|---|---|
| Workspace | npm workspaces | `apps/web`, `apps/api`, `packages/shared-types` |
| Web | Vite + React + TypeScript + TanStack Router/Query | SPA mobile-first; directorio público y dashboard |
| Estilos | CSS responsivo con tokens del producto | Crema, naranja llama, azul ultramar, menta y tinta oscura; detalles en `design-tokens.md` |
| Validación compartida | Zod | Validar respuestas de API en el navegador; `class-validator` en DTOs Nest |
| API | NestJS + TypeScript | REST versionada en `/api/v1`; módulos por dominio |
| Persistencia | PostgreSQL + Prisma | Migraciones versionadas; no exponer entidades ni credenciales |
| Autenticación | JWT + Argon2id | Secretos por entorno; tokens nunca en logs |
| Testing | Vitest/RTL en web; Jest/Supertest en API | Sin llamadas de red reales; servicios externos mockeados |
| Entorno local | Docker Compose | PostgreSQL para desarrollo; configuración desde `.env` |

No usar Next.js, GitHub API, Zustand, PWA, Kubernetes ni servicios externos que no estén en el alcance explícito de la feature.

## 3. Estructura esperada

- `apps/web/`: interfaz y cliente HTTP tipado.
- `apps/api/`: módulos NestJS, DTOs, servicios y persistencia Prisma.
- `packages/shared-types/`: schemas Zod y contratos compartidos cuando la dependencia entre apps lo permita.
- `.opencode/`: framework de orquestación, contexto, skills y especificaciones.

## 4. Reglas de negocio y seguridad

- Un perfil solo se publica con consentimiento explícito; el ranking requiere consentimiento independiente.
- Perfiles privados nunca aparecen en directorio, búsqueda, estadísticas de perfiles públicos ni ranking.
- No incluir email, hash de contraseña, tokens, fecha de nacimiento ni datos de contacto privados en respuestas públicas.
- La fecha del último día trabajado es autodeclarada, no puede ser futura y no debe presentarse como verificada.
- La cantidad de días se deriva de la fecha de inicio; no se almacena como contador mutable.
- La duración se expresa como días de búsqueda activa. El usuario puede ocultar la cifra.
- Validar todo input en servidor con whitelist estricta y límites de longitud; limitar abuso en registro e inicio de sesión.
- No confiar en identificadores del cliente para autorizar cambios. Las mutaciones del perfil/racha son del usuario autenticado.
- Errores HTTP no deben incluir stack traces, SQL ni secretos.
- No habilitar servicios externos de email/OAuth con credenciales ficticias; declarar configuración requerida y fallar explícitamente.

## 5. Identidad visual y contenido

- Tono cálido y empático. Nunca llamar a una persona “fracasada” ni usar el tiempo sin empleo como una competición.
- Frase de marca: “Tu talento sigue en movimiento. Que te encuentren.”
- El Hall de la Resiliencia es una selección opt-in de perfiles que rota semanalmente; no asigna puestos ni ordena por días.
- Los hitos son referencias personales opcionales; no medir valor, éxito ni empleabilidad.
- La pausa de la vitrina es voluntaria; no se despublica un perfil por falta de respuesta a recordatorios.
- La moderación de reportes y las métricas de exposición son privadas para administración; las métricas son agregadas y no identifican visitantes.
- La duración y cualquier badge se acompañan de texto; no depender solo del color.
- Usar los tokens documentados en `design-tokens.md`; diseñar estados de carga, error, vacío y éxito, además de teclado y foco visible.

## 6. Decisiones de arquitectura

### 2026-11-03 — Adaptación de `.opencode` a La Racha
**Contexto**: el framework describía un portafolio distinto y el producto no tenía aplicación en `antiracha/`.
**Decisión**: seguir la Fase 1 del plan con monorepo npm, Vite/React y NestJS/PostgreSQL/Prisma; mantener la privacidad opt-in y el tono empático como requisitos de producto.
**Agentes afectados**: orquestador, diseño, frontend, backend y QA.

### 2026-11-03 — Contrato de API versionado por el backend
**Contexto**: directorio, perfiles, rachas y autenticación requieren datos propios.
**Decisión**: backend define y documenta `/api/v1`; frontend valida y consume exactamente ese contrato. Directorio/ranking solo proyectan campos públicos.
**Agentes afectados**: backend, frontend y QA.

### 2026-03-11 — Hitos de racha (Fase 2)
**Contexto**: los usuarios necesitan hitos visuales para celebrar su constancia en la búsqueda activa (30, 60, 90 días).
**Decisión**: endpoint `GET /me/streak/milestones` que calcula `currentDays` desde `startDate` con `getStreakDays` existente (sin tablas nuevas). Hitos fijos definidos como constantes `MILESTONES` en `packages/shared-types` para que backend y frontend compartan textos empáticos. Sección "Tus hitos" en el dashboard con estados loading/error/vacío/éxito.
**Agentes afectados**: backend, frontend, qa-tester.

## 7. Glosario

- **Racha**: días consecutivos desde el último día trabajado, calculados desde una fecha autodeclarada.
- **Perfil público**: perfil publicado con consentimiento afirmativo; privado por defecto.
- **Hall de la Resiliencia**: selección voluntaria rotativa, separada del consentimiento de publicación y sin posiciones competitivas.
- **Reclutador**: rol previsto para una fase posterior; mensajería y verificación de empresa no forman parte del MVP inicial.
