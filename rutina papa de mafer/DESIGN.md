# DESIGN.md — Mi rutina

Decisiones visuales duraderas de la app. Si algo se añade después, se construye con esto.

## Qué es y para quién

App web móvil de un solo archivo para seguir una rutina de gimnasio de 6 semanas.
La usa una persona de unos 60 años, **de pie, frente a una máquina**, con el teléfono en
una mano. Lee a un brazo de distancia, a veces con mala luz y a veces sin lentes.

Eso manda sobre el estilo: lo que se toca tiene que ser grande y evidente, y las cifras
—series, repeticiones, kilos, segundos— tienen que leerse antes que cualquier otra cosa.

Modo: **Operate**. El éxito es terminar el entrenamiento sin pensar en la app.

## Dirección

Minimalista deportiva. Blanco cálido, negro carbón y un verde lima que solo aparece en
lo que se toca o en lo que ya está hecho. Esquinas muy redondeadas, mucho aire, contraste
fuerte. Nada de fotos, ilustraciones ni emojis: el peso visual lo cargan el tamaño, el
espacio y el lima.

## Color

| Token | Valor | Uso |
|---|---|---|
| `--lime` | `#D9FF43` | Acciones, lo marcado, el día activo. Siempre con texto `--ink` encima. |
| `--lime-press` | `#C8EE2E` | El mismo lima, presionado. |
| `--lime-wash` | `#F2FFC7` | Fondo de un ejercicio ya terminado. |
| `--ink` | `#171717` | Títulos, texto, botones oscuros, barra de descanso. |
| `--paper` | `#F8F8F6` | Fondo de la app. |
| `--white` | `#FFFFFF` | Tarjetas de ejercicio principal, superficies interiores. |
| `--gray` | `#F0F0F0` | Tarjetas secundarias y bloques que no son fuerza. |
| `--muted` | `#888888` | **Solo rótulos grandes o en mayúsculas.** Sobre papel da 3.3:1. |
| `--muted-aa` | `#5A5A5A` | Todo el texto secundario pequeño. Da 6.5:1 sobre papel. |
| `--alert` | `#B3261E` | Borrar y avisos clínicos. |

**La regla de contraste manda sobre la paleta.** `#888888` no cumple AA (4.5:1) para texto
normal, y esta persona no puede permitirse un texto flojo: por eso existe `--muted-aa`.
Las tres pantallas están verificadas a cero fallos de contraste.

El lima nunca lleva texto blanco. El par `#171717` sobre `#D9FF43` da 14:1.

## Tipografía

Dos familias de Google Fonts, cada una con un trabajo:

- **Space Grotesk** (500/700) — cifras, rótulos en mayúsculas, el logotipo, el reloj de
  descanso. Siempre con `font-variant-numeric: tabular-nums` para que los números no
  bailen al contar hacia atrás. Es la voz "técnica".
- **Manrope** (400–800) — todo lo que se lee como frase: títulos, nombres de ejercicio,
  explicaciones, avisos clínicos.

Cuerpo 18 px. Títulos de pantalla 38 px, `letter-spacing: -.03em`. Ningún texto baja
de 15 px.

**Las mayúsculas son para rótulos cortos.** Una frase nunca va en mayúsculas: cuesta
leerla porque se pierde la silueta de las palabras. Los encabezados de las filas de datos
y de los avisos van en caja normal, en negrita.

## Forma y espacio

Radios: 32 px (bloques grandes y hojas), 24 px (tarjetas), 18 px (controles),
`999px` (botones y pastillas). Escala de espacio de 4 a 56 px.

Sombras con desplazamiento **y** desenfoque; nunca un halo de color centrado.

Objetivo táctil mínimo 44 px; los botones principales miden 56 y el de empezar, 64.
Ancho máximo 580 px, centrado en pantallas grandes.

## Jerarquía de la lista

No todas las tarjetas pesan igual, porque los ejercicios no pesan igual:

- **Principal** (los dos con ★ en el documento): tarjeta blanca con sombra, título 21 px,
  cifra 28 px y un punto lima junto al nombre.
- **Accesorio**: fondo gris plano, sin sombra, más compacto.
- **Calentamiento, cardio, vuelta a la calma, muñeca**: no son tarjetas de ejercicio sino
  filas con icono y un botón "Hecho". Nunca se cuentan como ejercicios de fuerza.
- **Terminado**: fondo `--lime-wash`, el número se sustituye por una palomita.

Cada ejercicio muestra su nombre corto arriba y el detalle debajo; el nombre completo del
documento aparece dentro del panel y en el lector de pantalla.

## Iconos

Set propio, dibujado en el HTML. Dos reglas y ninguna excepción:

- **Los objetos son macizos**, con las esquinas redondeadas por un trazo del mismo color
  y `stroke-linejoin: round`. Así tienen el peso óptico de la tipografía en negrita.
- **Las marcas son trazo** de 3 px con punta redonda: la palomita, el chevron, el pulso.

Pares con sentido en vez de pictogramas sueltos: sol para el calentamiento y luna para la
vuelta a la calma; la muñeca es una articulación entre dos apoyos, no una mano.

Ninguno viene de una librería. El CSS solo decide el tamaño; la pintura va en el SVG.

## Voz

Habla quien escribió el plan. De tú, directo, y explica el porqué en una línea cuando
ayuda: «Es uno de los dos principales del día: aquí va tu mejor esfuerzo, siempre con 2
repeticiones de reserva. Nunca al fallo.»

Las frases propias de la app son **neutras en género**. Las citas del documento se
transcriben tal cual, con el género en que están escritas.

Los rótulos en mayúsculas son rótulos: una palabra o dos. Una frase nunca va en
mayúsculas, porque se pierde la silueta de las palabras y cuesta leerla.

## La primera vez

No es un folleto de cuatro láminas con puntitos. Son dos preguntas que de verdad cambian
la aplicación y después una práctica sobre la pantalla real:

1. **Portada**: un campo entero de lima, el texto abajo, dos botones.
2. **¿En qué semana vas?** Fija el contenido de todo el bloque.
3. **¿Te autorizaron el esfuerzo fuerte?** Si no, el viernes pasa a cardio continuo, como
   manda el documento. La respuesta se guarda y se cambia en Ajustes.
4. **Práctica**: foco sobre el primer ejercicio, luego sobre «Serie 1», luego sobre el
   reloj. Son los controles de verdad, con las marcas desviadas para que la práctica no
   quede guardada.

## Movimiento

Un solo momento con intención: al terminar un ejercicio, la app cierra ese y abre el
siguiente. Lo demás son transiciones de 120–200 ms con `cubic-bezier(.2,.8,.2,1)`.
Todo se apaga bajo `prefers-reduced-motion`.

## Lo que no se hace

- Rachas, puntos, medallas, ranking ni mensajes de culpa.
- Semáforos clínicos interactivos: los datos de salud se consultan, no deciden nada.
- Proponer kilos. La app recuerda el último peso y nada más.
- Inventar cifras: donde el documento deja un guion, la app lo dice y manda a consultar.
- Carruseles de bienvenida, puntitos de paso y pantallas que describen lo que se ve detrás.
