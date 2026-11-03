# API La Racha — MVP

**Base path**: `/api/v1`  
**Dueño**: backend  
**Consumidor**: `apps/web`; las respuestas públicas se validan con Zod.

## Respuestas de error

Las rutas devuelven `{ statusCode, message, error? }`; errores de validación son `400`, falta de sesión `401`, recurso privado/no encontrado `404`, conflicto de email `409`, limitación `429` y error interno `500` sin detalles del servidor.

## Directorio público

### `GET /public/profiles?q=&country=&cursor=&limit=`

Sin autenticación. `200 { items: PublicProfile[], nextCursor: string | null }`. Solo perfiles con visibilidad `PUBLIC`, consentimiento de publicación concedido, usuario activo y racha activa. `limit` máximo 24.

### `GET /public/profiles/:slug`

Sin autenticación. `200 PublicProfile`; `404` para slug inexistente, privado o eliminado. Nunca devuelve correo, identificadores internos ni tokens.

### `GET /public/ranking?limit=10`

Sin autenticación. `200 { items: RankedProfile[] }`. Solo perfiles públicos con `showInRanking=true`; límite máximo 10. Cifra de días omitida si `showStreakCount=false`.

### `GET /public/stats`

Sin autenticación. `200 { publicProfiles: number }`; solo cuenta perfiles visibles con consentimiento vigente.

## Autenticación

### `POST /auth/register`

Body: `{ email, password, lastWorkDay, displayName, displayMode, publicProfileConsent, rankingConsent }`. Password: 12–128 caracteres; `lastWorkDay` fecha ISO no futura; consentimientos booleanos y términos aceptados. Crea usuario, perfil, racha y eventos de consentimiento atómicamente. `201 { accessToken, user, profile }`, `400`, `409`.

### `POST /auth/login`

Body `{ email, password }`. `200 { accessToken, user, profile }`; `401` para cualquier combinación incorrecta; `429` por límite de intentos.

### `POST /auth/verify-email`

Body `{ token }`. `200 { emailVerified: true }`; `400` para token inválido o expirado.

### `GET /auth/me`

Bearer JWT requerido. `200 { user, profile, streak }`; `401` si falta o expiró el token.

## Cuenta y racha (Bearer JWT)

### `GET/PATCH /me/profile`

Devuelve el perfil privado. PATCH acepta solo `displayName`, `displayMode`, `headline`, `bio`, `country`, `city`, `linkedinUrl`, `visibility`, `showInRanking`, `showStreakCount`. `visibility=PUBLIC` requiere consentimiento concedido; `200` devuelve el perfil propio sin credenciales.

### `PUT /me/streak`

Body `{ lastWorkDay }`; fecha ISO no futura. `200 { startDate, days, isActive }`; cambios a fecha inicial se limitan y auditan.

### `POST /me/streak/close`

Cierra la racha activa con razón `EMPLOYED` o `MANUAL`. `200 { isActive: false, endDate, endReason }`; `404` sin racha activa.

### `DELETE /me`

Elimina la cuenta autenticada y sus datos asociados; `204`. No acepta IDs de usuario suministrados por el cliente.

## Forma pública

`PublicProfile = { slug, displayName, displayMode, headline, bio, country, city, avatarUrl, industry, linkedinUrl, showStreakCount, streak?: { days, startDate, selfDeclared: true } }`.

`RankedProfile` es `PublicProfile` sin enlaces externos opcionales. No incluir email, `userId`, fecha de nacimiento o estado de verificación.
