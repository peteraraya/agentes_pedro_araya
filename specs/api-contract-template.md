# Plantilla de Contrato de API

Formaliza como artefacto publicado y versionado el contrato de datos externos que `frontend` define al consumir una API (ver `/context/handoff-protocol.md`, handoff 2 y 3), en vez de dejarlo solo como un bloque de código Zod pegado en la conversación. Copia esta plantilla a `/specs/api/[recurso].md` por cada recurso/endpoint externo nuevo o modificado.

> Adaptado a `react-base-app`: el equipo actual es `designer`, `frontend`, `qa-tester` — **no hay agente `backend`** ni API propia (SPA que consume la API pública de GitHub, ver `/context/project-context.md` §4). Donde el flujo genérico de abajo asignaría el contrato a `backend`, acá lo define `frontend` a partir de la respuesta real (documentada o no) de la API externa consumida — el "agente dueño" de cada recurso documentado con esta plantilla es `frontend`. Si el proyecto incorpora un backend propio en el futuro, esta plantilla vuelve a usarse con `backend` como dueño del recurso, tal como está redactada más abajo.

## Por qué existe esto además del schema Zod

El schema Zod en el código es la fuente de verdad en tiempo de ejecución, pero **no es documentación consultable** sin leer el código fuente. Este archivo es la vista legible del contrato — para que `qa-tester` (y cualquiera del equipo) lo consulte sin depender de releer el código cada vez, y para generar/mantener alineado el spec OpenAPI si el proyecto expone documentación pública o usa `@nestjs/swagger` (no aplica hoy a `react-base-app`, ver nota arriba).

---

## [Recurso] — ej. `Users`

**Base path**: `/api/v1/users`
**Agente dueño**: `backend` (o `frontend` si el recurso es una API externa consumida sin backend propio — ver nota de adaptación arriba)
**Consumido por**: `frontend` (especificar página/feature), `qa-tester`

### `POST /api/v1/users`

**Descripción**: Crea un nuevo usuario.

**Autenticación requerida**: Sí / No — tipo (Bearer JWT, etc.)
**Permisos**: (ej. rol `admin`, o público)
**Rate limit**: (ej. 5 req/min)

**Request body**:
```ts
{
  email: string;      // formato email válido, requerido
  password: string;   // mínimo 8 caracteres, requerido
  name: string;        // 1-100 caracteres, requerido
}
```

**Response 201 (éxito)**:
```ts
{
  id: string;          // UUID
  email: string;
  name: string;
  createdAt: string;   // ISO 8601
  // nunca se expone password/hash
}
```

**Response 400 (validación fallida)**:
```ts
{
  statusCode: 400;
  message: string[];   // lista de errores de validación, uno por campo
  timestamp: string;
}
```

**Response 409 (email ya existe)**:
```ts
{
  statusCode: 409;
  message: string;
  timestamp: string;
}
```

**Casos límite conocidos** (para `qa-tester`):
- Email con mayúsculas/minúsculas mixtas — ¿se trata como duplicado del mismo email?
- Password exactamente en el límite mínimo/máximo de longitud
- Campos con solo espacios en blanco

---

## Convención general para cada endpoint documentado aquí

- **Un bloque por endpoint**, con método + path como encabezado.
- **Todo código de estado HTTP posible** documentado, no solo el camino feliz — si el endpoint puede devolver 401/403/404/409/422/500 en circunstancias reales, se listan todos.
- **Forma exacta del body**, no una descripción en prosa ("devuelve el usuario creado") — el tipo TypeScript/schema real.
- **Casos límite conocidos** explícitos: esto alimenta directamente los criterios de aceptación en `/specs/feature-spec-template.md` y el trabajo de `qa-tester`.
- Cuando el contrato cambia, se actualiza este archivo en el mismo PR que el código — un contrato desactualizado es peor que no tener contrato documentado, porque genera falsa confianza.

## Relación con OpenAPI/Swagger

Si el proyecto expone `@nestjs/swagger`, este archivo es la referencia de diseño antes de anotar los decorators (`@ApiProperty`, `@ApiResponse`) en el código — evita que la documentación autogenerada sea la única fuente y quede acoplada a que alguien recuerde anotar correctamente cada DTO. Para proyectos sin Swagger expuesto, este archivo *es* la documentación de contrato.
