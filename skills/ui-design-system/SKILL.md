---
name: ui-design-system
description: Criterio de sistema de diseño UI del proyecto actual — reglas de tokens de color, tipografía, espaciado, componentes base, estados y contraste WCAG AA. Los valores exactos viven en `/context/design-tokens.md`, referenciado en los Parámetros del proyecto. Úsala siempre al definir o revisar tokens de color/espaciado/tipografía, crear o modificar un componente de UI base (botón, input, card, badge), verificar accesibilidad de contraste, o cuando se proponga un acento decorativo distinto del de marca.
---

# Sistema de diseño — reglas de uso de UI base

Los valores exactos (hex, escala, nombres de token) viven en `/context/design-tokens.md` — **fuente de verdad**, no se reinventan acá. Los valores de identidad específicos del proyecto se listan en los **Parámetros del proyecto** (abajo). Esta skill documenta el criterio de uso: qué token corresponde a qué situación, y las reglas que no son negociables al construir o revisar un componente de UI base.

## Parámetros del proyecto — Rompe la Racha

> Los únicos valores específicos de esta skill. Al adaptar a otro proyecto se editan estos parámetros (y `/context/design-tokens.md`), no el cuerpo de la skill.

| Parámetro | Valor actual (`Rompe la Racha`) |
|---|---|
| Paleta | Naranja principal, azul secundario, amarillo y menta según `/context/design-tokens.md` |
| Colores semánticos | Usa los tokens de estado definidos en `/context/design-tokens.md` |
| Tipografía | `Fraunces` y `DM Sans`, según los roles del sistema |
| Superficies | Marfil cálido, lavanda pálido y superficies con contraste accesible |
| Fuente de valores | `/context/design-tokens.md` |
| Skills relacionadas | Stack: `vite-tanstack-tailwind` (ver tabla "Skills activas" en `/skills/skills-README.md`) |

## 1. Regla de oro: roles de color consistentes

- Usa cada color según su rol definido en los tokens. El naranja es el acento principal; el azul, amarillo y menta son colores de apoyo, no acentos intercambiables.
- **Excepción, no violación**: los colores semánticos (ver Parámetros) comunican estado, no decoran — se usan exclusivamente para eso, nunca como acento estético alternativo al de marca.
- No agregues colores fuera de la paleta sin una decisión explícita y una revisión de contraste.

## 2. Componentes base — variantes antes que duplicados

- Un componente de UI base (botón, input, badge, card) tiene sus variantes definidas por prop (`variant`, `size`), no por copias del componente con estilos ligeramente distintos. Si necesitas "un botón un poco distinto", primero revisa si es una variante nueva del componente existente.
- Estados relevantes en todo componente interactivo: `default`, `hover`, `focus`, `active`, `disabled`, `loading` y `error` (si aplica).
- El estado `focus` es siempre visible (ring/outline con el color de acento de marca, ver Parámetros) y sigue un orden de tabulación lógico — nunca `outline: none` sin un reemplazo visible equivalente.

## 3. Contraste — verificado, no asumido

- WCAG AA como mínimo: 4.5:1 para texto normal, 3:1 para texto grande y elementos gráficos (incluye gráficos de `recharts-charts`). Se verifica el par real light/dark que se va a usar, no se asume que "el color de marca siempre contrasta bien" — algunos tonos del acento no cumplen AA sobre ciertos fondos; el token correcto por contexto está documentado en `/context/design-tokens.md`.
- Ningún estado o información crítica se comunica solo por color — el error de un input, por ejemplo, lleva también texto/ícono, no solo un borde rojo.
- Tamaños de objetivo táctil cómodos en cualquier control interactivo pensado para touch (mobile).

## 4. Tipografía y espaciado

- Escala tipográfica limitada (definida en `/context/design-tokens.md`) — no se introduce un tamaño de fuente nuevo "a ojo" cuando la escala ya cubre el caso.
- Las familias por rol definidas en Parámetros (display/técnico) — un tercer tipo de letra es una decisión de identidad, no un ajuste de componente puntual (ver `frontend-design`).
- Espaciado según los tokens CSS del proyecto; evita valores arbitrarios que rompan el ritmo visual.

## 5. Dark mode como ciudadano de primera clase

- Cada token de color tiene su par definido para light y dark (ver `/context/design-tokens.md`) — un componente nuevo no se entrega solo probado en light mode.
- Fondos/superficies usan la escala definida en Parámetros en ambos temas — nunca subtonos cálidos que desentonen con la marca.

## 6. Relación con otras skills

- `frontend-design` decide la dirección visual (cuándo y por qué usar cada elemento); esta skill define los valores y componentes concretos que la implementan.
- La skill del stack del proyecto (ver Parámetros) traduce estos tokens a clases reales en componentes; la skill de testing del stack verifica accesibilidad básica en tests de UI (`getByRole`, presencia de indicadores no-color).
- Las skills de gráficos disponibles heredan estas mismas reglas de color/contraste para gráficos, no un sistema de color aparte.

## 7. Checklist rápido al crear/revisar un componente de UI base

- [ ] ¿Se usan los colores según los roles establecidos en Parámetros y los colores semánticos solo para estados?
- [ ] ¿Están definidos los estados default/hover/focus/active/disabled (+ loading/error/vacío si aplica)?
- [ ] ¿El foco es visible y sigue un orden de tabulación lógico?
- [ ] ¿El contraste (texto, iconografía, bordes de estado) cumple WCAG AA en light y dark, verificado contra `/context/design-tokens.md`?
- [ ] ¿Ningún estado se comunica solo por color?
- [ ] ¿La tipografía y el espaciado usan la escala del sistema, no valores arbitrarios?
- [ ] ¿El componente está resuelto en dark mode, no solo en light?
