---
name: frontend-design
description: Guía de dirección visual para react-base-app — el portafolio SPA de Pedro Araya (Vite + React 19 + Tailwind). Úsala siempre que la tarea implique tipografía, paleta de color, layout, jerarquía visual o estilo de interfaz, o cuando el usuario pida que algo "se vea profesional", "no genérico" o "con dirección propia". También aplica al revisar si un componente nuevo respeta la identidad visual ya establecida (azul de marca, tipografía Newsreader/IBM Plex Mono) antes de introducir un patrón visual distinto.
---

# Dirección visual — react-base-app (2026)

Guía de estilo para que cualquier pantalla o componente nuevo se sienta parte del mismo producto, no una plantilla genérica resuelta pantalla por pantalla. Complementa a `ui-design-system` (que define los tokens exactos): esta skill decide *cuándo y por qué* usar cada uno, no solo cuáles existen.

Principio rector: **cada decisión visual tiene una razón funcional (jerarquía, legibilidad, consistencia), nunca "porque se ve bien" sin más**. Si no puedes explicar por qué un elemento es más grande, más oscuro o está más separado que otro, probablemente esa decisión todavía no está tomada.

## 1. Identidad de marca — no negociable

- **Color de acento único**: `blue` de `tailwind.config.js` (`blue-600` light / `blue-400` dark). Ningún acento decorativo nuevo (íconos destacados, bordes activos, CTAs) usa otro matiz — purple/indigo/emerald/amber quedaron descartados explícitamente (ver `/context/project-context.md` §5, decisión "Sistema de diseño blue unificado").
- **Colores semánticos aparte**: `green` (éxito/online/completado), `red` (error/peligro), `amber`/`orange` (advertencia/en progreso). Estos no son "acentos decorativos" — comunican estado, así que no se reemplazan por blue.
- **Tipografía**: `Newsreader` para display/títulos (con carácter editorial, no una sans genérica de plantilla), `IBM Plex Mono` para código/datos técnicos. No introduzcas una tercera familia sin razón — cada fuente nueva es una decisión de identidad, no un ajuste de gusto puntual.
- **Fondos y superficies**: escala `gray` fría neutra (`gray-50`/`white` en light, `gray-900`/`gray-800` en dark) — nunca un gris con subtono cálido, rompe la coherencia con el azul de marca.

## 2. Jerarquía visual antes que decoración

- Tamaño, peso, color y espaciado comunican importancia relativa de forma deliberada. Si dos elementos compiten por la misma atención (dos títulos del mismo tamaño en la misma vista, tres CTAs igual de destacados), la jerarquía todavía no está resuelta.
- Un componente nuevo primero pregunta "¿ya existe una variante de esto en `ui-design-system`?" antes de crear un patrón visual paralelo — dos formas de resolver lo mismo (dos estilos de card, dos formas de botón primario) es una regresión, no una opción más.
- Escala tipográfica limitada (no más de 5-7 tamaños activos) — si hace falta un tamaño nuevo, primero revisa si un tamaño existente ya resuelve el caso antes de agregar uno.

## 3. Layout y densidad

- Este es un portafolio (secciones de CV, proyectos, widgets técnicos) consumido tanto por reclutadores en modo rápido (`recruiterMode`/TL;DR) como por desarrolladores explorando en detalle (`vscodeMode`) — el layout de una sección nueva se piensa para ambos modos, no solo el que el autor tuvo en mente al escribirla.
- Espaciado en escala consistente (múltiplos de 4/8px vía las utilidades de Tailwind) — nunca un valor arbitrario (`mt-[13px]`) que rompe el ritmo vertical del resto de la página.
- Diseña para el rango completo de viewports relevante (mobile-first, breakpoints de Tailwind), no solo para el ancho de escritorio en el que se probó primero — la navegación mobile y desktop de `router.tsx` ya establece el patrón a seguir para nuevas secciones.
- Elementos decorativos (3D con react-three-fiber, animaciones de Framer Motion) refuerzan la jerarquía, nunca compiten con el contenido real (texto de CV, datos de proyectos) por atención — si una animación hace más difícil leer el contenido, la animación pierde.

## 4. Evitar el look genérico de plantilla

- Sin paletas ni tipografías "default de librería" sin dirección propia — el objetivo declarado del proyecto es un CV memorable, no un template de Tailwind UI sin adaptar.
- Los widgets técnicos (`ROICalculator`, `HireMeKanban`, `AIChatWidget`, `CommandPalette`) son parte de la narrativa de portafolio ("demostraciones de skill técnicas embebidas") — su estilo visual debe sentirse curado, no un componente de ejemplo de una librería de UI pegado sin ajuste.
- Antes de aceptar un ícono, imagen o layout "porque fue lo primero que apareció", pregúntate si refuerza la identidad del producto o es intercambiable con cualquier otro sitio — si es intercambiable, probablemente no está terminado.

## 5. Relación con la implementación real

- Antes de proponer un patrón visual que termine en código, revisa `vite-tanstack-tailwind` para confirmar que es viable en una SPA sin SSR (ej. nada que dependa de renderizado en servidor) y `ui-design-system` para los valores exactos de token que corresponde usar.
- Si una dirección visual implica un estado (hover, error, vacío) no contemplado, se especifica ahí mismo — un mockup o descripción que solo cubre el happy path no es una especificación completa (ver checklist de `qa-qc-react-vite` para los casos que van a testear ese estado).

## 6. Checklist rápido al proponer una dirección visual

- [ ] ¿El único acento decorativo es `blue`? ¿Los colores semánticos (`green`/`red`/`amber`) se usan solo para estado, no como decoración?
- [ ] ¿La tipografía usada es `Newsreader` (display) o `IBM Plex Mono` (técnico/código), sin una tercera familia sin justificar?
- [ ] ¿Hay una jerarquía clara (no dos elementos del mismo peso compitiendo por atención)?
- [ ] ¿Ya existe una variante de este componente en el sistema de diseño antes de crear una nueva?
- [ ] ¿El layout funciona en mobile y desktop, no solo en el viewport donde se diseñó primero?
- [ ] ¿Una animación o elemento 3D refuerza el contenido en vez de competir con él?
- [ ] ¿Esto se ve como parte de este portafolio específico, o sería intercambiable con cualquier plantilla genérica?
