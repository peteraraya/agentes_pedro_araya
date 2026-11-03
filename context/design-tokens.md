# Design Tokens — Rompe la Racha

Valores de identidad de marca definidos por el plan de producto. Todo componente nuevo debe usar estos tokens y mantener accesibilidad WCAG 2.2 AA.

## 1. Color

| Token | Valor | Uso |
|---|---|---|
| `cream` | `#FFF8EC` | Fondo principal |
| `flame` | `#FF6B35` | CTA principal, racha y marca |
| `ultramarine` | `#3A4DFF` | Acento secundario, enlaces y foco |
| `sunshine` | `#FFC83D` | Acentos de celebración |
| `mint` | `#2EC4A6` | Éxito y estados positivos |
| `ink` | `#1B1B2F` | Texto principal |
| `surface` | `#FFFFFF` | Tarjetas y formularios |
| `muted` | `#6B6B7A` | Texto secundario |
| `danger` | `#B42318` | Error/destructivo, siempre acompañado de texto |

El naranja, amarillo y menta deben reservarse para los usos indicados; no comunicar información crítica solo por color. Verificar contraste real de texto y controles en sus fondos antes de cerrar UI.

## 2. Tipografía y escala

- Titulares: Bricolage Grotesque o fallback sans-serif.
- Cuerpo: DM Sans o fallback sans-serif.
- Cifras de racha: cifras tabulares (`font-variant-numeric: tabular-nums`).
- Escala sugerida: 12, 14, 16, 18, 24, 36 y 48 px.

## 3. Espaciado, forma y elevación

- Espaciado sobre escala de 4 px: 4, 8, 12, 16, 24, 32, 48, 64.
- Bordes redondeados generosos (12–24 px), bordes definidos y sombras sólidas suaves para el estilo soft-pop.
- Objetivos táctiles de al menos 44 × 44 px.

## 4. Estados accesibles

- Foco de teclado visible y de alto contraste en todos los controles.
- Loading: skeleton que conserva el espacio final.
- Error: mensaje accionable y asociado al control afectado.
- Empty: explicar qué ocurrió y ofrecer el siguiente paso.
- Respetar `prefers-reduced-motion`; animaciones nunca son requisito para comprender el estado.
- En el modo oscuro, mantener los mismos roles de color con contraste verificado.
