# MVP — La Racha

**Estado**: en desarrollo  
**Flujo**: diseño → contrato/backend → frontend → QA

## Objetivo y alcance

Permitir que una persona cree una cuenta y una racha autodeclarada, controle su perfil público y sea descubierta solo cuando da consentimiento explícito.

Incluye autenticación por email/contraseña, verificación de email, perfil y racha editables, directorio público con búsqueda básica, perfil público SEO-friendly en el cliente y ranking opt-in. Excluye mensajería, reclutadores, Google OAuth, pagos, ranking con reacciones, notificaciones, exportación de datos y despliegue CI/CD.

## UX y reglas

- Perfil privado por defecto; la publicación y participación en ranking tienen controles independientes.
- Fecha de fin de empleo obligatoria en el registro, no futura y descrita como autodeclarada.
- Puede usarse seudónimo; correo nunca aparece en vistas públicas.
- Estados de carga, error, vacío y éxito; navegación por teclado y textos sin depender solo del color.
- Celebrar la búsqueda activa sin promover la duración del desempleo como competición.

## Handoff de contrato → frontend

El contrato publicado está en [`api/la-racha.md`](./api/la-racha.md). El backend define rutas y shapes; el frontend valida respuestas con los schemas compartidos de `packages/shared-types`.

## Criterios de aceptación

- Registro crea cuenta, perfil privado, racha activa y consentimientos versionados en una transacción.
- Fechas futuras, payloads desconocidos, email duplicado y credenciales incorrectas se rechazan.
- Solo un perfil público con consentimiento concedido aparece en directorio y estadísticas públicas.
- Solo un usuario que opta por ranking aparece en el top 10.
- Perfil público omite email y otros campos privados.
- Usuario puede cambiar su perfil/visibilidad/racha propia o solicitar eliminación de su cuenta.
- Los días se calculan desde la fecha de inicio y nunca se guardan como contador.
- Tests cubren auth, fecha y filtros de privacidad en API, así como estados accesibles de la interfaz.

## Decisiones y deuda explícita

- Google OAuth y refresh token rotativo quedan fuera del MVP de este incremento.
- La eliminación inmediata se implementa en lugar de la purga programada a 30 días; el producto deberá definir retención legal antes de lanzamiento.
- El email de verificación usa Resend en producción y un enlace de consola solo en desarrollo local.
