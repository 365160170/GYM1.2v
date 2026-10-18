# ESPECIFICACIÓN — APP WEB MÓVIL «MI RUTINA»

> Documento de implementación para un desarrollador o asistente de programación. Fuente de verdad: archivo adjunto `RUTINA_MARIA_BLOQUE2.pdf` (APEX · Protocolo Salud · Fuerza · Longevidad 60+ · v3.0, Bloque 2, semanas 7–12). **No colocar nombres de personas en ninguna pantalla**. La aplicación es un acompañamiento rápido del PDF, no un reemplazo ni una fuente independiente de prescripción médica.

## 1. Objetivo y alcance

Construir una **aplicación web mobile-first, simple y agradable para una persona de aproximadamente 60 años**, accesible desde el navegador del teléfono y alojable en GitHub Pages. La persona recibe el PDF por separado y usa la app para ver rápidamente qué le toca, cómo ejecutar los ejercicios, cuánto descansar, registrar series y cargas, y consultar alternativas y precauciones.

**Reglas innegociables:**
- Sin fotografías, GIF, videos, ilustraciones ni imágenes de ejercicios. Usar texto y, cuando ayuden, iconos SVG sencillos (por ejemplo Lucide embebidos de forma local); no depender de una librería de iconos remota.
- Sin nombres personales, sin registros, login, contraseñas, anuncios ni cuentas.
- Un solo archivo principal `index.html` con HTML, CSS y JavaScript vanilla. Se permite `RUTINA_BLOQUE2.pdf` como archivo estático adicional para abrir la guía completa; si no está disponible, ocultar el botón al PDF y mostrar una nota configurable, sin enlaces rotos.
- Sin frameworks ni backend. Sin servicios externos de analítica. Sin llamadas a API necesarias.
- La información visible para cada día y semana debe reflejar el PDF; **no inventar ejercicios, cargas, equivalencias ni recomendaciones clínicas**.
- Compatible con Android e iPhone, Safari y Chrome recientes. Debe funcionar con conexión débil una vez cargada; no prometer modo offline completo salvo que se implemente y pruebe caché.

## 2. Prioridad UX: «lo abro y sé qué hacer»

La pantalla principal debe mostrar de inmediato:
1. Título **Mi rutina**.
2. **Hoy: [día] — [nombre del entrenamiento]**.
3. **Semana [7–12]**, con un control muy claro para cambiarla en Configuración.
4. Botón principal grande: **Ver rutina de hoy** o **Continuar entrenamiento**.
5. Vista de la lista de ejercicios del día en el mismo flujo (sin navegar por cinco pantallas).
6. Accesos secundarios discretos a **Información**, **Guía PDF** y **Ayuda / Ver recorrido**.

**Interacción preferida:** lista completa del día con cada ejercicio como tarjeta de texto. Al tocarla se expande dentro de la misma página para mostrar series, repeticiones, descanso, consejos, alternativa y registro. No forzar una navegación paso a paso ni ocultar el resto de la lista.

## 3. Pantalla de bienvenida y recorrido (onboarding)

La **primera vez** que alguien abre la app, mostrar una bienvenida accesible, no un formulario. El recorrido debe estar diseñado para completarse en **menos de un minuto**, con texto grande y ejemplos muy concretos. Cada paso tiene **Atrás / Siguiente**, y siempre **Saltar explicación**. Indicador claro `Paso 1 de 4`.

### Pantalla inicial, antes del recorrido
- Título: **Tu rutina, siempre a la mano**.
- Subtítulo: **Consulta tus ejercicios, marca lo que ya hiciste y guarda el peso que utilizaste.**
- Texto de apoyo: **El documento PDF sigue siendo tu guía completa. Aquí encontrarás lo necesario para entrenar más fácilmente.**
- Botón grande: **Conocer cómo funciona**.
- Botón secundario: **Ir directamente a mi rutina**.

### Recorrido de cuatro pasos

**Paso 1 — Encuentra tu entrenamiento**
- Título: **Tu entrenamiento del día**.
- Mensaje: **Al abrir la aplicación aparecerá la rutina que corresponde al día de hoy. También puedes elegir otro día de lunes a domingo.**
- Mini demostración *sin imágenes*, usando una tarjeta HTML con el texto `Lunes · Tren superior A`.

**Paso 2 — Marca tus series**
- Título: **Marca lo que vas terminando**.
- Mensaje: **Abre un ejercicio y toca “Serie 1”, “Serie 2” o “Serie 3” cuando termines cada una.**
- Mini demostración con tres botones interactivos de muestra. Que no alteren datos reales.

**Paso 3 — Descansa y guarda tus pesos**
- Título: **Lleva el control fácilmente**.
- Mensaje: **Al completar una serie, podrás usar el reloj de descanso. Si entrenas con peso, anota los kilogramos para recordarlos la próxima vez.**
- Mini demostración de `Descanso: 01:30` y `Peso utilizado: __ kg`; sin ejecutar entrenamientos reales.

**Paso 4 — Elige tu semana**
- Título: **La rutina cambia según tu semana**.
- Mensaje: **Selecciona la semana del programa en la que vas. Las series y repeticiones se ajustarán automáticamente a lo indicado en tu PDF.**
- Selector accesible con **Semana 7** a **Semana 12**; inicializar en 7 si aún no hay selección. **No asumir que la fecha actual revela la semana real.**
- Recordatorio breve: **Lee las precauciones y consulta con tu profesional de salud antes de comenzar el bloque.**
- Botón **Terminar y ver mi rutina**.

### Comportamiento del recorrido
- Al terminar o saltar, `localStorage.onboardingSeen = true`.
- Acceso permanente **Ayuda → Ver recorrido otra vez**.
- Si borra los datos del navegador, puede volver a verlo.
- Evitar animaciones complejas, carruseles automáticos y texto que avance solo.
- Soportar cerrar el recorrido, botón Atrás y foco de teclado.

## 4. Estructura de pantallas

### A. Inicio / Rutina
- Encabezado: Mi rutina · Semana N.
- Día predeterminado = día local del teléfono; **domingo = descanso** (sin entrenamientos obligatorios).
- Selector de día: botones grandes **Lun, Mar, Mié, Jue, Vie, Sáb, Dom** con nombre completo en una etiqueta accesible. Permitir cambiar día sin modificar la fecha del sistema.
- Bloque de entrenamiento: título, subtítulo, instrucciones breves y lista completa de ejercicios.
- Mostrar **calentamiento y vuelta a la calma** como secciones propias cuando correspondan, no contarlas erróneamente como ejercicios de fuerza. Preservar el orden particular del martes (potencia antes que fuerza), viernes (cardio por intervalos según autorización) y sábado.
- Tarjeta de ejercicio: nombre, series × repeticiones o duración según semana; descanso si aplica; al expandir: consejo técnico del PDF, botones por serie, carga en kg cuando aplica, botón para alternativas únicamente si el PDF ofrece alguna, e inicio de descanso.
- Estado de sesión visible: `X de Y ejercicios terminados` (además de marcas por serie); botón `Finalizar entrenamiento` con confirmación si hay partes pendientes.
- Botón **Reanudar** si hay sesión guardada. Jamás reiniciar al actualizar página.

### B. Información
Acceso sencillo, sin largas tablas de 12 páginas. Agrupar en acordeones:
- **Antes de entrenar y precauciones** (glucosa, síntomas, medicamentos, presión, ojos y pies): resumen fiel y legible del documento, con botón para ver el PDF original.
- **Cuidado de muñeca**: escala de dolor, progresión y cuándo consultar.
- **Cómo progresar**: indicaciones por semanas, RPE y repeticiones en reserva; que la app no proponga incrementos automáticos.
- **Alimentación y suplementos**: información del PDF con sus advertencias sobre riñón, medicamentos y autorización profesional, nunca como recomendaciones personales nuevas.
- **Guía completa en PDF**, si el archivo está disponible.

### C. Configuración / Ayuda
- **Semana actual:** selector 7–12. Indicar que se cambia manualmente; no modificarla automáticamente por calendario salvo que el usuario configure explícitamente la fecha de inicio y acepte esa función.
- **Fecha de inicio del bloque (opcional):** permite calcular semana automáticamente como `7 + floor((fechaActual - inicioEnDías)/7)`, limitada para mostrar «Fuera del bloque» cuando corresponda; si se usa selector manual, mostrar cuál prevalece. Recomendado: pedir la fecha opcional durante Configuración, **no** durante la bienvenida. Si la fecha no se configura, funciona perfectamente en modo manual.
- Botón **Ver recorrido otra vez**.
- Botón **Descargar copia de mis registros** (exportar JSON; muy recomendable porque localStorage puede borrarse).
- Botón **Borrar datos guardados** con confirmación doble, sin confundirlo con borrar una única serie.

## 5. Funciones obligatorias

1. **Día automático:** iniciar en día actual usando fecha local; elegir manualmente otro día para adelantar/repasar.
2. **Semana configurable:** 7 a 12; la selección determina valores exactos de los ejercicios, incluyendo descarga en semana 12.
3. **Series completadas:** botones independientes por serie; registrar estado por fecha local + ejercicio + índice de serie. Botón para desmarcar si se pulsó por error. Para ejercicios de tiempo/recorrido, permitir marcar «Realizado».
4. **Temporizador de descanso:** tomar segundos específicos de la tabla, preferir 90 s cuando indica 90 s y 60 s cuando indica 60 s; en rangos mostrar el valor elegido por defecto y dejar cambiar dentro del rango. Iniciar automáticamente **al marcar una serie**, con controles grandes Pausar / Reanudar / Omitir; respetar preferencias de movimiento reducido. Usar `Date.now()` para calcular el tiempo real aun si la app se minimiza, sin depender de un contador en segundo plano. Al llegar a cero, mostrar señal visual; vibración solo cuando el navegador lo permita. No exigir permiso de notificaciones.
5. **Peso utilizado:** campo numérico opcional (kg, admite decimales) en ejercicios con carga; recordar el último peso de **ese ejercicio**, mostrar «Último: X kg» sin asumir que deba aumentarse. No exigir cargas para cardio, respiración, equilibrio o ejercicios sin peso. Nunca indicar peso recomendado si el PDF no lo da.
6. **Ejercicios alternativos:** desplegable con las alternativas A y B de la sección 05 del PDF, con contexto de cuándo usarlas. No transformar automáticamente cambios de ejercicio en equivalencias clínicas universales. El usuario elige la alternativa, pero conservar el ejercicio original en referencia.
7. **Continuar después:** guardar series, cargas, sesión activa, semana, fecha de entrenamiento y temporizador en localStorage. Reabrir exactamente donde se quedó, respetando cambios de día; si era una sesión de ayer, ofrecer «Continuar sesión anterior» o «Empezar hoy», sin mezclarlas.
8. **Texto grande:** cuerpo mínimo 17–18 px, botones 48–56 px de alto, contrastes AA, espacios generosos, objetivos táctiles adecuados, sin información dependiente únicamente del color.
9. **Consulta de PDF:** enlace al documento completo como recurso adicional, si se incluye junto al HTML. No intentar incrustar las 12 páginas en el inicio.

## 6. Datos de entrenamiento: transcripción del PDF

**IMPORTANTE:** Todo lo que sigue está extraído del PDF, especialmente pp. 4–8. Campos `7–8`, `9–10`, `11` corresponden a semanas, no fechas. Para la **semana 12**, el documento indica de forma general: **2 series de todo con carga de semana 9**, RPE 5–6; principales 2 × 8–10, accesorios 2 × 10–12, con excepciones para el cardio y las actividades de movilidad según sus instrucciones. No extrapolar una dosis exacta a una actividad cuando el PDF deja un guion `—`; mostrar la nota de descarga y el texto original de consulta. El **jueves de semana 12** corresponde a evaluación funcional según el plan y debe señalarse claramente.

### Lunes · Tren superior A — Empuje + postura (PDF pág. 5)
| Ejercicio | Sem 7–8 | Sem 9–10 | Sem 11 | Descanso |
|---|---|---|---|---|
| ★ Press de pecho en máquina, agarre neutro | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| ★ Remo con pecho apoyado en máquina, neutro | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| Press inclinado 30° con mancuernas, neutro | 2×12–15 | 3×10–12 | 3×10–12 | 75 s |
| Press de hombro en máquina, neutro | 2×12–15 | 3×10–12 | 3×10–12 | 75 s |
| Face pull en polea con cuerda | 2×15 | 3×12–15 | 3×12–15 | 60 s |
| Superserie: extensión de tríceps en polea (cuerda) + elevación lateral en polea baja | 2×12–15 | 3×12 | 3×12 | 60 s al terminar el par |

Cardio: bici Zona 2, **12 min (sem 7–10), 15 min (11), 10 min (12)**. En la superserie explicar que son dos movimientos unidos y no se descansa entre ambos. Consejos técnicos: conservar textualmente o con paráfrasis fiel las indicaciones correspondientes de la tabla del PDF.

### Martes · Tren inferior A — Rodilla + potencia (PDF pág. 5)
| Ejercicio | 7–8 | 9–10 | 11 | Descanso |
|---|---|---|---|---|
| Sentadilla a banco con subida rápida (potencia) | 2×6 | 3×6 | 3×6 | 90 s |
| ★ Prensa de piernas | 3×10–12 | 3×8–10 | 3×8 | 90–120 s |
| ★ Sentadilla dividida estática con apoyo | 2×8 c/u | 3×8–10 c/u | 3×8–10 c/u | 75 s |
| Extensión de cuádriceps en máquina | 2×12–15 | 3×10–12 | 3×10–12 | 60 s |
| Pantorrilla de pie en máquina | 3×12–15 | 3×12–15 | 3×12–15 | 45 s |
| Caminata tándem (talón-punta) junto a barra | 3×4 m | 3×6 m | 3×6 m + giro de cabeza | 30 s |

Cardio: caminata en cinta con inclinación 3–5%; **15 min (sem 7–11), 12 min (12)**. La potencia va antes de los ejercicios de fuerza.

### Miércoles · Tren superior B — Jalón + core (PDF pág. 6)
| Ejercicio | 7–8 | 9–10 | 11 | Descanso |
|---|---|---|---|---|
| ★ Jalón al pecho con barra V (neutro) | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| ★ Remo sentado en polea, neutro | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| Press de pecho en polea de pie | 2×12–15 | 3×10–12 | 3×10–12 | 60 s |
| Jalón con brazos rectos en polea (cuerda) | 2×12–15 | 3×12 | 3×12 | 60 s |
| Superserie: curl martillo con mancuernas + tríceps sobre cabeza en polea | 2×12 | 3×10–12 | 3×10–12 | 60 s al terminar el par |
| Pallof press (antirrotación) | 2×10 c/u | 3×10 c/u | 3×12 c/u | 45 s |
| Dead bug (escarabajo) | 2×8 c/lado | 3×8 c/lado | 3×10 c/lado | 45 s |

Cardio: elíptica o bici, **12 min (sem 7–10), 15 min (11), 10 min (12)**.

### Jueves · Tren inferior B — Cadera + equilibrio (PDF pág. 6)
| Ejercicio | 7–8 | 9–10 | 11 | Descanso |
|---|---|---|---|---|
| ★ Hip thrust (máquina o barra con almohadilla) | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| ★ Peso muerto rumano con mancuernas | 3×10–12 | 3×8–10 | 3×8 | 90 s |
| Curl femoral sentado en máquina | 2×12–15 | 3×10–12 | 3×10–12 | 60 s |
| Subida a cajón de 20–25 cm | 2×8 c/u | 3×8 c/u | 3×10 c/u | 75 s |
| Abducción de cadera en máquina | 2×15 | 3×12–15 | 3×12–15 | 45 s |
| Equilibrio en una pierna (progresivo) | 3×20–30 s | 3×20–30 s | 3×20–30 s | 30 s |

Cardio: bici **15 min (sem 7–11), 12 min (12)**. El PDF detalla progresión de apoyos para equilibrio; no simplificarla eliminando la barra de seguridad. **En semana 12, destacar evaluación funcional del jueves**.

### Viernes · Full body funcional (PDF pág. 7)
| Ejercicio | 7–8 | 9–10 | 11 | Descanso |
|---|---|---|---|---|
| Bici con intervalos | 4 intervalos | 5 intervalos | 6 intervalos | — |
| Sentadilla goblet | 2×10–12 | 3×10 | 3×10 | 75 s |
| Remo a una mano en polea, de pie | 2×12 c/u | 3×10–12 c/u | 3×10–12 c/u | 60 s |
| Leñador en polea (alta → baja) | 2×10 c/u | 3×10 c/u | 3×10 c/u | 45 s |
| Paseo con mancuerna al pecho | 3×20 m | 3×30 m | 3×30 m | 45 s |
| Práctica de levantarse del suelo | 2×2 | 2×3 | 3×3 | 60 s |

**Aviso obligatorio y claro:** el PDF permite los intervalos **solo con autorización médica para ejercicio vigoroso**; si no la hay, indica 20–25 min continuos en Zona 2. Las duraciones totales del bloque de cardio en la tabla general son **22 min (7–8), 24 min (9–10), 26 min (11), 20 min continuo (12)**. La descripción del intervalo en el PDF es 5 min suave → 1 min RPE 6–7 / 2 min RPE 3, repetir → 3–5 min suave. **No automatizar el inicio de intervalos ni presuponer autorización.**

### Sábado · Cardio + movilidad — Recuperación activa (PDF pág. 7)
| Ejercicio | 7–8 | 9–10 | 11 | Descanso |
|---|---|---|---|---|
| Caminata a paso vivo | 30 min | 35 min | 40 min | — |
| Gato-vaca sobre antebrazos | 2×8 | 2×8 | 2×8 | — |
| Rotación torácica acostada de lado (libro abierto) | 2×8 c/u | 2×8 c/u | 2×8 c/u | — |
| Estiramiento flexor de cadera en rodilla | 2×30 s c/u | 2×30 s c/u | 2×30 s c/u | — |
| Movilidad tobillo frente a pared | 2×10 c/u | 2×10 c/u | 2×10 c/u | — |
| Plancha lateral sobre codo | 2×20 s c/u | 3×25 s c/u | 3×30 s c/u | 45 s |
| Respiración diafragmática | 5 min | 5 min | 5 min | — |

Semana 12: caminata **30 min** según tabla general de cardio, y descarga global según documento; no inventar otra progresión detallada si no aparece. Domingo: descanso completo o paseo tranquilo.

### Reglas semanales comunes (PDF pág. 4)
| Semana | Principales | Accesorios | Esfuerzo | Descanso general |
|---|---|---|---|---|
| 7–8 | 3×10–12 | 2–3×12–15 | RPE 7 | 90 s / 60 s |
| 9–10 | 3×8–10 | 3×10–12 | RPE 7–8 | 90–120 s / 60 s |
| 11 | 3×8 | 3×10–12 | RPE 8 | 120 s / 60–75 s |
| 12 | 2×8–10 | 2×10–12 | RPE 5–6 | 90 s / 60 s |

**Resolución de posibles diferencias:** en el UI usar primero la pauta explícita de cada ejercicio y semana y preservar la tabla general como información contextual. En semana 12, usar la pauta de descarga global sin deducir automáticamente repeticiones de ejercicios de potencia, equilibrio o movilidad que no estén especificadas; dejar claro cuándo una instrucción general requiere consultar el PDF o al responsable del plan.

### Orden de la sesión (PDF pág. 4)
- Calentamiento: **8–10 min** (5 min caminata/bici suave, 2 min movilidad y 1–2 series de aproximación del primer ejercicio).
- Potencia: martes y viernes solo cuando el plan lo indique; el documento incluye la sentadilla potente el martes y entrenamiento funcional/intervalos el viernes. No crear ejercicios adicionales.
- Fuerza: principales primero, accesorios después, superseries cuando corresponde.
- Cardio: usar las duraciones por día y semana anteriores.
- Vuelta a la calma: 5 min de caminata lenta y respiraciones; registro de glucosa conforme a indicaciones profesionales y del documento.

## 7. Alternativas: incluir exactamente las del PDF (pág. 8)

Presentar botón «¿Máquina ocupada o molestia?» solo cuando haya alternativa documentada. Al desplegar mostrar alternativas A y B y el contexto:

| Original | Alternativa A | Alternativa B |
|---|---|---|
| Press pecho en máquina | Press mancuernas en banco, neutro | Press en polea de pie |
| Remo pecho apoyado | Remo mancuerna apoyada en banco | Remo sentado en polea |
| Press inclinado mancuernas | Press inclinado en máquina | Press en polea baja a 30° |
| Face pull | Aperturas inversas con banda | Pájaro sentado con mancuernas ligeras |
| Sentadilla a banco (potencia) | Levantarse de silla con brazos cruzados | Prensa con subida rápida, carga ligera |
| Sentadilla dividida estática | Subida a cajón bajo | Prensa a una pierna |
| Peso muerto rumano | Barra hexagonal desde bloques | Hiperextensión a 45° sin peso |
| Hip thrust | Puente glúteo en piso | Patada glúteo en polea |
| Curl femoral sentado | Curl femoral tumbado | Curl femoral de pie a una pierna |
| Pallof press | Pallof con banda | Plancha lateral sobre codo |
| Dead bug | Dead bug solo piernas | Respiración con abdomen activo boca arriba |
| Paseo mancuerna al pecho | Marcha en lugar con rodillas altas | Caminata con chaleco ligero |
| Intervalos en bici | Intervalos en elíptica | Caminata con inclinación variable |

Al implementar, capturar además la columna «Cuándo usarla» y la instrucción de conservar el mismo patrón y rango de repeticiones. La app nunca garantiza que una alternativa sea adecuada para síntomas nuevos: consultar al personal de salud si es necesario.

## 8. Salud y seguridad: información accesible, sin convertirla en decisión automática

Crear una sección «Antes de entrenar» visible desde Inicio e Información. Extraer fielmente del PDF pp. 2–3 y 8–10; **no omitirla por hacer la app sencilla**. Contiene:
- Semáforo de transición del bloque (verde, amarillo, rojo) y condiciones para no progresar.
- Tabla de glucosa antes de entrenar y señales de hipoglucemia; incluir **unidades mg/dL**, procedimiento del documento y advertencia de riesgo según medicación. No inferir medicamentos ni recomendar dosis.
- No entrenar y solicitar atención apropiada ante síntomas de alarma descritos (p. ej., dolor en pecho, mareo, dificultad respiratoria desproporcionada). En una urgencia, buscar atención inmediata.
- Medidas de cuidado de muñeca y escala 0–2 / 3–5 / >5 de dolor; escalones de carga documentados.
- Precauciones de presión arterial, ojos, pies, hidratación y respiración.
- Las pautas de nutrición/suplementos del PDF, especialmente condicionantes de función renal y medicamentos.
- Nota visible: **«Esta aplicación organiza una rutina incluida en el documento original. No reemplaza la valoración médica ni modifica sus indicaciones.»**

Mostrar esta información como texto educativo consultable; **no crear un semáforo médico interactivo que autorice o prohíba entrenar a partir de una cifra aislada**, no generar alertas clínicas automatizadas ni cambiar prescripción según datos del usuario. El plan debe ser aprobado por profesional de salud antes de ejecutarse.

## 9. Modelo de datos recomendado en JS

```js
const APP_KEY = 'mi-rutina-v1';
const state = {
  onboardingSeen: false,
  weekMode: 'manual', // 'manual' | 'byStartDate'
  selectedWeek: 7,
  startDate: null, // 'YYYY-MM-DD'; opcional
  selectedDay: null, // null = día local actual
  activeSession: null, // { date, day, week, startedAt, completedAt, seriesByExercise, exerciseDone, weightsByExercise }
  history: [], // sesiones terminadas
  lastWeights: {}, // { exerciseId: { kg, date } }
  restTimer: null // { startedAt, durationSec, pausedRemainingSec, running }
};
```

- Rutinas, notas técnicas, alternativas y cardio: datos estáticos en objeto `PROGRAM` dentro del mismo HTML, **sin nombres personales**. Cada ejercicio tiene `id`, `label`, `isPrincipal`, `byWeek`, `restSec`, `technicalCue`, `alternatives`, `type` (`strength`, `time`, `distance`, `interval`, `mobility`, `superset`).
- `localStorage`: escritura tras cada marca o cambio, y lectura al iniciar. Usar `try/catch` por si el almacenamiento falla o el usuario lo deshabilita; en ese caso informar «Los cambios de esta sesión podrían no guardarse» sin bloquear la rutina.
- Evitar usar únicamente `day + exerciseId` como clave de sesión; incluir fecha local y semana para no mezclar días o bloques.
- Al elegir otra semana a mitad de sesión, advertir antes de modificar el entrenamiento activo; no perder registros.
- Si `startDate` existe, calcular semana por días de calendario **locales** (no restar horas UTC sin normalizar, para evitar errores por zona horaria/DST).
- Validar campos numéricos: peso >= 0, valores razonables, permitir vacío y no crear objetivos de carga.
- Exportar JSON de datos personales y ofrecer importar opcionalmente en una versión posterior. No sincronizar automáticamente entre teléfonos: `localStorage` no lo permite.

## 10. Estilo visual y accesibilidad

Dirección: **calma, claridad y dignidad; no infantilizar**.
- Fondo marfil casi blanco `#F8F8F5` o blanco; texto muy oscuro `#17241D`; verde bosque `#245C49` para acciones; acento cálido discreto. Botones peligrosos solo en rojo y confirmación.
- Fuente del sistema `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif` sin descargas.
- Cuerpo 18 px y títulos 24–28 px; mínimo 16 px incluso en ayudas secundarias. Interlineado generoso.
- Alto mínimo de pulsación de botones 48 px; espaciado 12–16 px; ancho máximo 560 px en desktop centrado.
- Nada de tablas anchas en móvil: tarjetas compactas y acordeones. Evitar exceso de iconos o barras de progreso.
- Navegación principal simple y persistente: **Rutina · Información · Ayuda**. Usar etiquetas completas además de cualquier icono.
- Estado marcado: palabra «Completada» o ✓ y contraste; no depender solo de verde/rojo.
- Cumplir teclado, foco visible, `aria-label`, `aria-expanded`, accesibilidad de modales, `prefers-reduced-motion`, zoom 200%, Safe Area móvil.
- Sin banners promocionales, ranking, rachas, puntos ni mensajes de presión o culpa.

## 11. Diseño textual de referencia

```text
┌────────────────────────────────────┐
│ Mi rutina              Semana 7 ▾  │
│                                    │
│ Hoy · Lunes                        │
│ Pecho, espalda y hombros           │
│                                    │
│ [ Antes de entrenar / cuidados ]   │
│                                    │
│ 1. Press de pecho en máquina       │
│    3 series · 10–12 repeticiones   │
│    [ Ver / Registrar ▾ ]           │
│                                    │
│    Descanso 90 segundos            │
│    [Serie 1] [Serie 2] [Serie 3]   │
│    Peso: [___] kg  Último: —       │
│    Consejo de ejecución            │
│    [ Ver alternativas ▾ ]          │
│                                    │
│ 2. Remo con pecho apoyado          │
│    3 series · 10–12 repeticiones   │
│    [ Ver / Registrar ▾ ]           │
│                                    │
│ ... demás ejercicios ...           │
│                                    │
│ [Rutina] [Información] [Ayuda]     │
└────────────────────────────────────┘
```

En la primera visita aparece el recorrido antes de esta pantalla. Luego no interrumpe la rutina.

## 12. Criterios de aceptación y pruebas

**La entrega se considera terminada si:**
1. Es una web real, funcional en un móvil, con **solo `index.html`** como código principal.
2. La primera visita muestra bienvenida + recorrido de 4 pasos; es saltable y repetible desde Ayuda.
3. Aparece el día actual; permite cambiarlo manualmente. El domingo no muestra ejercicios de fuerza.
4. Selector semanas 7, 8, 9, 10, 11, 12 muestra el contenido correcto del PDF, incluyendo semana 12.
5. Incluye todos los ejercicios, tiempos de cardio, consejos técnicos, descansos y alternativas del PDF correspondientes. No omitir superseries ni condiciones del viernes.
6. Cada serie se marca y desmarca individualmente, y el temporizador se activa después de marcarla; funciona al volver a la pestaña después de minimizar el teléfono.
7. Guarda el último peso **por ejercicio** y lo muestra en su siguiente aparición.
8. Recargar la página no pierde progreso y puede recuperar una sesión anterior sin mezclarla con la actual.
9. Información conserva todas las precauciones médicas relevantes, con acceso a PDF si existe.
10. No hay nombres de personas, imágenes, dependencias externas, registro ni base de datos.
11. Probado en iPhone/Safari y Android/Chrome; sin desplazamiento horizontal y legible con tamaño de texto grande.
12. Incluye botón de exportación JSON funcional y confirmación antes de borrado.

**Casos de prueba obligatorios:** primera visita; salto del onboarding; ver recorrido de nuevo; pasar domingo→lunes; cambiar semana 8→9 y 11→12; terminar una superserie; pausar/reanudar descanso y minimizar pantalla; refrescar tras registrar dos series; sesión incompleta de ayer; cambio de fecha del móvil; sin localStorage; sin archivo PDF; intentar borrar registros; usar 200% de zoom.

## 13. Entregables

- `index.html` autosuficiente, con el programa completo, estilos y JavaScript.
- `README.md` mínimo con instrucciones para subir a GitHub Pages y colocar opcionalmente el PDF junto al HTML bajo el nombre `RUTINA_BLOQUE2.pdf`.
- Código claro, comentado solo donde la lógica es delicada (semana, sesiones, temporizador, descarga).
- **No entregar una maqueta que aparenta funcionar**. Todos los botones visibles deben cumplir su acción o no mostrarse.

## 14. Orden de implementación sugerido

1. Cargar datos exactos del PDF y verificar tablas de lunes a sábado y semanas.
2. Construir la lista móvil y las tarjetas expandibles.
3. Implementar selección de día/semana y lógica de semana 12.
4. Añadir marcas por serie, descanso y guardado de cargas.
5. Persistencia y reanudación de sesiones.
6. Información clínica textual y alternativas.
7. Bienvenida y onboarding; ayuda para reabrirlo.
8. Exportación, accesibilidad y pruebas manuales.

**Principio final:** si una característica añade más pasos que utilidad, quitarla. La aplicación debe facilitar consultar el PDF **sin reemplazarlo**, especialmente mientras la persona está de pie frente a una máquina del gimnasio.
