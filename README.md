# Mi rutina — aplicación web

Aplicación web móvil para seguir la rutina del Bloque 2 (semanas 7 a 12): ver qué toca hoy,
marcar cada serie, cronometrar el descanso y anotar los pesos.

Sin cuentas, sin registro, sin servidores y sin imágenes. Todo lo que se guarda queda en la
memoria del navegador de ese teléfono.

## Archivos

| Archivo | En el repositorio | Para qué sirve |
|---|---|---|
| `index.html` | Sí | La aplicación completa: HTML, CSS y JavaScript en un solo archivo. Es lo único imprescindible. |
| `README.md` | Sí | Esto. |
| `DESIGN.md` | Sí | Las decisiones de diseño: paleta, tipografía, iconos, voz. Para quien vaya a tocarla después. |
| `RUTINA_BLOQUE2.pdf` | **No** | La guía completa. Lleva el plan clínico de una persona, así que está en `.gitignore`. |
| `ESPEC_APP_RUTINA_GYM.md` | **No** | La especificación de partida. Menciona un nombre propio, así que tampoco se sube. |

### Por qué el PDF no se publica

GitHub Pages sirve el sitio **de forma pública**, también cuando el repositorio es privado:
las Pages privadas necesitan un plan de pago. Todo lo que subas queda accesible para
cualquiera con la dirección.

La aplicación está preparada para eso: sin el PDF al lado, oculta el botón y muestra una
nota, sin enlaces rotos. La persona abre el PDF desde el archivo que recibió aparte.

Si aun así quieres publicarlo, quítalo de `.gitignore` y fuérzalo:

```bash
git add -f RUTINA_BLOQUE2.pdf
```

Ten en cuenta que el propio `index.html` sí lleva el contenido clínico del documento
(umbrales de glucosa, medicamentos, precauciones), aunque **sin ningún dato que
identifique a nadie**: ni nombre, ni edad, ni fechas personales.

Si el PDF no está, la app **no muestra ningún enlace roto**: en su lugar sale una nota.
El nombre del archivo y el texto de la nota se cambian al principio del `<script>`,
en `PDF_FILE` y `PDF_MISSING_NOTE`.

## Una dependencia externa: las tipografías

La app carga **Manrope** y **Space Grotesk** desde Google Fonts. Es la única cosa que pide
a internet, y es lo que le da carácter propio.

Consecuencias, para que no haya sorpresas:

- La primera carga necesita conexión. Después el navegador las guarda en caché y la app
  funciona con señal débil o sin ella.
- Si las tipografías no cargan, la app **se ve bien igual**: cae a la fuente del sistema y
  no se rompe nada.
- Si prefieres cero dependencias externas, borra las tres etiquetas `<link>` del `<head>`.
  La app seguirá funcionando con la fuente del teléfono.

## Publicar en GitHub Pages

1. Crea un repositorio nuevo en GitHub.
2. Sube `index.html` y, si quieres el botón del PDF, también `RUTINA_BLOQUE2.pdf`,
   los dos en la **raíz** del repositorio.
3. En el repositorio, entra en **Settings → Pages**.
4. En *Source* elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`. Guarda.
5. Espera un minuto. GitHub te dará una dirección con la forma
   `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.
6. Ábrela en el teléfono y añádela a la pantalla de inicio
   (iPhone: Compartir → Añadir a pantalla de inicio. Android: menú → Añadir a pantalla de inicio).

A partir de ahí se abre como cualquier aplicación.

## Probarla en la computadora

Para verla como se verá publicada:

```bash
python -m http.server 8777
```

Después abre `http://127.0.0.1:8777/index.html`. Para verla desde el teléfono en la misma
red wifi, usa la IP de la computadora en vez de `127.0.0.1`.

## Cómo está pensada

- **La primera vez pregunta dos cosas** y luego deja practicar sobre la pantalla real: en
  qué semana va y si tiene autorización médica para esfuerzo vigoroso. Lo que se marca
  durante la práctica no se guarda.
- **El viernes se adapta.** Los intervalos del viernes solo aparecen si hay autorización
  médica para ejercicio vigoroso, que es lo que exige el documento. Sin ella, ese día
  muestra 20 a 25 minutos continuos en Zona 2. Se cambia en *Ajustes*.
- **La semana la eliges tú.** La app nunca la cambia sola. En *Ajustes* puedes darle la fecha
  en que empezaste el bloque para que la calcule; es opcional y reversible.
- **Al terminar un ejercicio se abre el siguiente** y el reloj de descanso arranca solo al
  marcar una serie. El reloj usa la hora real, así que sigue bien aunque bloquees el teléfono.
- **Los datos viven solo en ese teléfono.** `localStorage` puede borrarse al limpiar los datos
  del navegador; por eso está *Ajustes → Guardar una copia*, que baja un JSON. No hay
  sincronización entre dispositivos: no es posible sin un servidor.
- **Cada ejercicio recuerda su historia.** Muestra «La última vez: 42.5 kg · semana 8» y los
  tres pesos anteriores. Es información, no una meta.
- **La app no prescribe nada.** Recuerda el último peso de cada ejercicio como referencia,
  pero no propone aumentos ni interpreta síntomas. Lo de *Cuidados* es un resumen fiel del
  documento, no indicaciones nuevas.
- **Donde el documento deja un guion, la app lo dice.** En la semana 12, los ejercicios de
  potencia, equilibrio y movilidad no tienen dosis propia en el PDF: se muestra la de las
  semanas anteriores con un aviso de que hay que consultarlo.

## Qué se verificó

En el navegador, sobre `http://127.0.0.1:8777`:

- primera visita completa: portada, elección de semana, pregunta de autorización médica y
  práctica guiada con foco sobre el primer ejercicio, la serie y el reloj; al terminar, la
  práctica no deja ni una serie marcada ni una sesión guardada (verificado contra
  `localStorage`);
- el viernes cambia según la respuesta: con autorización muestra los intervalos, sin ella
  muestra 20 a 25 minutos continuos, y el aviso del día se reescribe en consecuencia;
- el historial de pesos por ejercicio: «La última vez» no se altera al escribir el peso de hoy;
- cambio de día y de semana, incluida la semana 12 con su descarga, su aviso cuando el
  documento no da cifra y la evaluación funcional del jueves;
- marcar y desmarcar series, arranque automático del descanso, y avance automático al
  siguiente ejercicio al completar uno;
- el reloj de descanso sigue correcto después de recargar la página;
- peso guardado por ejercicio y mostrado como «La vez pasada: X kg» al volver;
- sesión a medias de un día anterior, con *Seguir con ese* y *Empezar uno nuevo*, sin mezclarlas;
- funcionamiento con el almacenamiento bloqueado: avisa y la rutina se sigue pudiendo usar;
- sin el PDF al lado: ningún enlace, solo la nota;
- copia JSON y borrado con doble confirmación;
- **contraste: cero fallos de AA** en las tres pantallas, medido sobre el render real;
- ningún objetivo táctil por debajo de 44 px y ningún desbordamiento horizontal;
- jerarquía de encabezados sin saltos (h1 → h2 → h3);
- el detector de patrones de diseño pasa limpio salvo un aviso de «fuente repetida», que es
  la decisión deliberada de usar Space Grotesk para cifras y rótulos;
- ningún icono roto: todos los `<use>` apuntan a un símbolo existente del set propio.

**Falta probarla en un teléfono real**, iPhone con Safari y Android con Chrome. El
comportamiento táctil, el área segura bajo la cámara, la vibración al acabar el descanso y
el zoom al 200 % solo se confirman en el aparato.
