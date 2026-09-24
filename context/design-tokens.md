# Design Tokens — react-base-app

Fuente de verdad de valores exactos de color, tipografía, espaciado y estados para todo el equipo (`designer`, `frontend`, `qa-tester`). Referenciado desde `/context/project-context.md` §2 y §4, y desde las skills `frontend-design`, `ui-design-system`, `vite-tanstack-tailwind` y `recharts-charts`. Cualquier valor nuevo de color/tipografía/espaciado que se use en el producto se agrega acá primero — nunca se elige "a ojo" en un componente y se documenta después.

## 1. Color de marca — escala `blue`

Única escala usada para acentos decorativos (botón primario, links, foco, bordes activos, ícono destacado, serie principal de gráficos). Corresponde a la escala `blue` estándar de Tailwind, sin variantes custom.

| Token | Uso principal (light) | Uso principal (dark) |
|---|---|---|
| `blue-50` / `blue-100` | Fondos sutiles de estado activo/hover en superficies claras | — |
| `blue-500` | Acento secundario, iconografía sobre fondo claro | Texto de acento sobre fondo oscuro cuando `blue-400` no alcanza contraste |
| `blue-600` | **Acento primario** — botón primario, link, borde activo, foco (light mode) | — |
| `blue-400` | — | **Acento primario** — botón primario, link, borde activo, foco (dark mode) |
| `blue-700` / `blue-800` | Estado `active`/`pressed` de un elemento `blue-600` | Estado `active`/`pressed` de un elemento `blue-400` |

Regla: `blue-600` en light y `blue-400` en dark son los pares por defecto para "el" azul de marca — no se mezclan indistintamente dentro del mismo modo.

## 2. Neutros — escala `gray` (fría)

Fondos, superficies, texto y bordes. Nunca un gris con subtono cálido (nada de `stone`/`neutral`/`zinc` mezclado con `gray`).

| Token | Uso |
|---|---|
| `white` / `gray-50` | Fondo base y superficies elevadas (cards) en light |
| `gray-100` / `gray-200` | Bordes y divisores en light; fondo de controles inactivos |
| `gray-500` / `gray-600` | Texto secundario en light |
| `gray-900` | Texto principal en light; fondo base en dark |
| `gray-800` | Superficies elevadas (cards) en dark |
| `gray-700` | Bordes y divisores en dark |
| `gray-100` / `gray-300` | Texto principal / secundario en dark |

## 3. Colores semánticos — solo para estado, nunca decorativos

| Token | Significado | Notas |
|---|---|---|
| `green-500`/`green-600` | Éxito, online, completado | No se usa como acento estético alternativo al azul |
| `red-500`/`red-600` | Error, peligro, destructivo | Todo estado de error va acompañado de texto/ícono, nunca solo el color |
| `amber-500`/`orange-500` | Advertencia, en progreso | Distinguible de `red` también por ícono, no solo por tono |

Los bloques de código/sintaxis conservan su resaltado nativo del tema de highlighting elegido — no se fuerzan a la paleta `blue`.

## 4. Tipografía

| Token | Familia | Uso |
|---|---|---|
| `font-display` | Newsreader | Títulos, headings, momentos editoriales del CV |
| `font-mono` | IBM Plex Mono | Código, datos técnicos, widgets tipo terminal/IDE (`vscodeMode`) |
| `font-sans` (base) | Fuente sans del sistema/Tailwind por defecto | Cuerpo de texto general |

### Escala tipográfica (máximo 6 tamaños activos)

| Token | Tamaño | Uso |
|---|---|---|
| `text-xs` | 12px | Metadatos, labels auxiliares |
| `text-sm` | 14px | Texto secundario, controles |
| `text-base` | 16px | Cuerpo de texto |
| `text-lg` | 18px | Subtítulos |
| `text-2xl` | 24px | Títulos de sección |
| `text-4xl`/`text-5xl` | 36px/48px | Título principal (hero) |

## 5. Espaciado

Escala de Tailwind en múltiplos de 4px (`p-1` = 4px, `p-2` = 8px, `p-4` = 16px, `p-6` = 24px, `p-8` = 32px, `p-12` = 48px, `p-16` = 64px). Ningún valor arbitrario (`p-[13px]`) sin que primero se descarte que un token de la escala ya resuelve el caso.

## 6. Radios y sombras

| Token | Uso |
|---|---|
| `rounded-lg` (8px) | Cards, inputs, botones — radio por defecto del sistema |
| `rounded-full` | Elementos circulares (avatares, nodos de timeline, badges de estado) |
| `shadow-sm` | Elevación sutil (cards en reposo) |
| `shadow-md` | Elevación en hover/foco de elementos interactivos elevados |

## 7. Estados de componentes interactivos

Todo componente interactivo (botón, input, card clicable, filtro) define explícitamente:

| Estado | Regla |
|---|---|
| `default` | Color base según §1/§2 |
| `hover` | Un paso más oscuro en la escala del color base (ej. `blue-600` → `blue-700` en light) |
| `focus` | Ring visible en `blue-600`/`blue-400` (2px mínimo), nunca `outline: none` sin reemplazo |
| `active` | Un paso adicional más oscuro/`pressed` respecto a `hover` |
| `disabled` | `gray-300`/`gray-600` con opacidad reducida (~50%), cursor `not-allowed` |
| `loading` | Skeleton (`animate-pulse` sobre `gray-200`/`gray-700`) del tamaño real del contenido final, nunca un spinner que colapsa el layout |
| `error` | Borde/texto `red-600`/`red-400` + ícono/mensaje — nunca solo el color |
| `vacío` (empty) | Ícono + texto explicando qué pasó y qué puede hacer el usuario a continuación |

## 8. Contraste — mínimos verificados

- Texto normal sobre fondo: **4.5:1** (WCAG AA).
- Texto grande (≥18px o ≥14px bold) y elementos gráficos/iconografía: **3:1**.
- Pares verificados como conformes en este sistema: `blue-600` sobre `white`/`gray-50`; `blue-400` sobre `gray-900`/`gray-800`; `gray-900` sobre `white`/`gray-50`; `gray-100` sobre `gray-900`/`gray-800`.
- Un tono de la escala `blue` que no cumpla AA en un fondo específico (ej. `blue-500` sobre `gray-100` en algunos casos límite) no se usa para texto en ese fondo — se sube a `blue-600`/`blue-700` o se usa solo como acento no textual (borde, ícono grande).

## 9. Cómo se actualiza este archivo

- Cambios de token siguen el mismo flujo que cualquier decisión de arquitectura: se registran también en `/context/project-context.md` §5 ("Decisiones de arquitectura registradas") cuando el cambio afecta a todo el sistema (ej. el paso a paleta `blue` unificada documentado ahí).
- `designer` es quien propone un token nuevo o modificado; `frontend` confirma la implementación real en `tailwind.config.js`; ambos deben quedar sincronizados en el mismo PR — un token documentado acá que no existe en el config (o viceversa) es una inconsistencia a corregir de inmediato.
