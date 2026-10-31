CARTE es la versión publicada de **Cartesian DS**, el sistema de diseño de Andreani para plataformas logísticas: back-office, tracking, portales de clientes y apps de operación. Todo lo que está acá sale de la especificación de Cartesian (tokens, componentes, reglas de gobernanza y metadata de comportamiento). Si algo no está documentado, no se inventa: se consulta al equipo de DS.

## Principios

1. **Semántica antes que color.** Los componentes solo consumen tokens semánticos (`c-color-*`). La paleta global (`palette-*`) es referencia y nunca se usa directo.
2. **Un estado, un namespace.** Si un componente tiene un estado activo (error, success, warning, info, caution), sus capas surface + border + text + icon salen del mismo namespace. Mezclarlos es violación `[NAMESPACE_ROTO]`.
3. **Capas con rol fijo.** `bg` = layout de la página. `surface` = contenedores (card, modal, input). `fill` = elementos internos con estados completos (hover, pressed, selected). No se intercambian.
4. **Densidad operativa.** Las pantallas operativas priorizan escaneo rápido: tablas de 40px, spacing `c-spacing-2`/`c-spacing-3` en paneles densos y un máximo de 7 elementos visuales sin agrupar.
5. **Diseñar el estado degradado.** Todo componente con datos en tiempo real contempla sin conexión, datos desactualizados (stale) y reintento.

## Tokens: cómo se nombran

La especificación nombra los tokens con punto: `color.surface.error.subtle`. En este sistema cada token se publica como variable CSS con prefijo `c-` y guiones: `--c-color-surface-error-subtle`. La nota de cada token incluye su nombre con punto para mapear 1:1 con Figma y con el código.

- ✅ `var(--c-color-surface-error-subtle)`
- ❌ `#df474c` (hex directo, `[HEX_HARDCODEADO]`)
- ❌ `var(--palette-gray-500)` (paleta global, `[PALETA_GLOBAL]`)

## Color

**Marca.** El rojo Andreani es `c-color-fill-primary` (#d71920). Hover `c-color-fill-primary-hover`, pressed `c-color-fill-primary-pressed`. Se usa en la acción principal, el ítem activo y el foco de los inputs. No es un color decorativo.

**Texto.** El texto corriente va en `c-color-text-content-base` sobre `c-color-surface` o `c-color-bg`. Para títulos, `c-color-text-content-title`. Los links usan `c-color-text-link` con sus estados `-hover` y `-visited`. Sobre fondos oscuros o con fill de marca, `c-color-text-neutral-inverse`.

**Estados.** Hay cinco namespaces: success (verde), info (azul), caution (amarillo), warning (naranja) y error (rojo). Cada uno tiene surface, surface-subtle, fill con hover/pressed/subtle, border default/strong/subtle, text/strong/subtle e icon. Sobre un fondo `-subtle`, el texto va en `-strong`.

**Neutros.** `c-color-bg` (#fafafa) es el fondo de la plataforma. `c-color-surface` (#fff) es para contenedores. Los bordes van en `c-color-border-neutral` (controles), `-subtle` (separadores) y `-soft` (bordes de tabla y deshabilitados).

## Tipografía

- **Rubik** para headlines y títulos (`--font-headline`, `--font-title`).
- **Roboto** para body, labels, captions y overline.
- Cualquier otra familia es violación `[TOKEN_INCORRECTO]`.

Ambas se cargan desde Google Fonts.

- Headlines 1–6 son responsive: en código usan `clamp()` (el rango figura en cada estilo) y acá se muestra el valor máximo.
- `body-medium` (16px/24px) es el default de lectura. `body-small` (14px/20px) es el texto de tablas, alerts, modales y toasts.
- Cada tamaño va con su line-height del mismo nombre. Nunca se cruza `font.size.X` con `font.lineheight.Y`.
- Pesos: 400 para lectura y 500 para labels, headers de tabla y títulos. 700 solo para énfasis fuerte. 300 no se usa en consolas operativas.

## Espaciado, radios y bordes

- La escala es discreta, de `c-spacing-0` a `c-spacing-9` (0–80px), con base `c-spacing-4` = 16px. No se usan valores intermedios.
- En contextos densos (tablas, paneles operativos) el espaciado interno es `c-spacing-2`/`c-spacing-3`. `c-spacing-5` o más ahí es violación `[CONTEXTO_LOGISTICO]`.
- Radios:
  - Los botones, chips, pills y el FAB son siempre pill: `c-border-radius-xlarge`, sin excepción.
  - Inputs, selects, alerts, cards, popovers y el contenedor de tabla: `c-border-radius-small` (8px).
  - Checkbox y skeleton de texto: `c-border-radius-xsmall` (4px).
- Grosor de borde: se usa `c-border-width-regular` (1px). El naming de border width está en conflicto en Figma, así que conviene evitar `subtle`.

## Elevación

- Todas las sombras usan gray/600 al 25%.
- Los usos son:
  - `c-shadow-bottom-subtle` para cards en lista, toast y date picker.
  - `-regular` para modal, card, popover y dropdown.
  - `-strong` para el FAB.
  - `c-shadow-drawer` para el drawer.
- Nunca va sombra en tablas. Tampoco se combinan dos sombras en un mismo elemento.

## Layout

- **Mobile:** botones e inputs al 90% del ancho, con botones Large (48px) y targets táctiles de al menos 44×44px.
- **Desktop:** botones de 40px y modal centrado.
- **Tablas:** fila de 40px, padding de celda 16px, contenedor de 8px, sin sombras y un máximo de 3 acciones por fila (el resto va a un Kebab).
- **Header:** logo siempre a la izquierda. **Footer:** anclado al pie. **Drawer:** siempre full-height. **FAB:** uno por pantalla.

## Elegir componentes

1. **¿Es feedback?** Si es transitorio, **Toast**. Si es persistente dentro del flujo, **Alert**. Si bloquea y exige una decisión, **Modal**. Usar un Toast para una acción irreversible o un Modal para feedback transitorio es `[ANTI_PATTERN]`.
2. **¿Acción flotante única?** FAB.
3. **¿Jerarquía de más de 2 niveles?** Breadcrumbs. En un wizard no: ahí va Progress (step).
4. **¿Acciones extra en una tabla?** Kebab, en el margen derecho.
5. **¿Selección múltiple o única?** Checkbox o Radio Button.
6. **¿Filtros compactos?** Chips. ¿Estado de solo lectura de una entidad? Pill. ¿Un contador sobre navegación? Badge.
7. **Botones:** un solo Primary por contexto. En filas de tabla, siempre Ghost Small (32px).

## Contenido

La voz es **amigable, creativa y especialista**: cercana y clara, sin formalidad corporativa, sin chistes forzados y sin jerga interna.

**Reglas inviolables:**

- **Nunca nombrar a la empresa en textos de producto.** Se escribe «la app», «la plataforma» o «nuestro equipo».
- Español de Argentina con voseo: «Revisá», «Elegí», «Tenés».
- La persona usuaria es la protagonista: se le habla de vos.
- Frases cortas, con una idea por frase. Sin tecnicismos, sin dobles negaciones y sin perífrasis («vas a poder hacer»).
- Inclusión por redacción: «Te damos la bienvenida», no «bienvenido/a».
- **Todo error o estado crítico dice qué pasó y qué hacer.** ❌ «Hubo un problema con tu envío.» ✅ «No pudimos confirmar tu dirección. Revisá que esté completa y volvé a intentar.»

**Convenciones de UI:**

- Los botones arrancan con un verbo y van en sentence case: «Confirmar envío».
- En navbars y menús el verbo va en el disparador («Agregar…») y los ítems nombran el objeto.
- Los estados de una Pill tienen una o dos palabras («En tránsito», «Entregado»).
- Los empty states explican la causa y ofrecen una salida.
- Las fechas van en formato DD/MM/YYYY.

**Tono por situación:**

- Éxito, info o CTA: simpático o casual.
- Error o advertencia: prudente y considerado.
- Pago, cancelación o retención: prudente, con una solución concreta.

## Iconografía

- Cartesian usa **Font Awesome 6 Pro** (Regular/Light). En tablas va a 16px con `c-color-icon-neutral-subtle`; en chips, a 14px.
- Esa familia tiene licencia y no se incluyó acá. Las previews usan glifos SVG genéricos (chevron, check, cruz, info) como sustitutos.
- En producción el ícono sale de FA 6 Pro con el color del token `c-color-icon-*` de su namespace.
- Los botones solo-ícono llevan `aria-label`.

## Marca

El logo de Andreani está en `assets/Logo Andreani/logoAndreani.svg` (SVG de una sola tinta, #D71920; no hereda color por `currentColor`). El logo de Norlog no se entregó. Header y Footer muestran un placeholder en las previews. No hay que redibujar el logo: se usa el archivo oficial.

## Accesibilidad (WCAG 2.1 AA)

- Texto normal 4.5:1, texto grande 3:1 y controles o íconos con significado 3:1.
- Target táctil mínimo de 44×44px.
- Se respeta `prefers-reduced-motion` (skeleton, spinner).
- `Escape` cierra modales y drawers, y el orden de tabulación es lógico.

Pares de la fuente que **no** cumplen y se mantienen tal cual (sin re-teñir la marca):

- `c-color-text-caution` (1.6:1) y `c-color-text-warning` (3.3:1) sobre blanco. Para texto sobre `-subtle` se usan las variantes `-strong`.
- Alert «Alto»: `c-color-text-error-subtle` sobre `c-color-surface-error` da 3.5:1, success 4.3:1 e info 4.48:1.
- Chip Primary en default: `c-color-text-primary` sobre `c-color-fill-primary-subtle` da 3.1:1. Chip Secondary en hover: 3.1:1.
- Badge error: blanco sobre `c-color-fill-error` da 4.1:1.
- Links del Footer sobre `c-color-surface-neutral`: 3.9:1.
- Íconos de tabla en `c-color-icon-neutral-subtle`: 2.5:1, por debajo del 3:1 para íconos con significado.
- Track apagado del Toggle (`c-color-fill-neutral`) sobre blanco: 1.3:1.
- **Focus:** `c-shadow-focus-regular` es un glow de 2px al 25% de alpha y no alcanza 3:1. La regla del DS («focus con shadow-focus-*, nunca outline») hoy produce un foco casi invisible. Es un fallo fatal para operación por teclado.
- El placeholder (`c-color-text-content-subtitle`, #3a4046) es **más oscuro** que el texto ingresado (`c-color-text-content-base`). Un campo vacío puede leerse como completo.
