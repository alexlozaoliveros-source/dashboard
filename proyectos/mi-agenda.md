# Mi agenda (agenda personal web)

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-04
- **Última actualización:** 2026-08-08

## Cómo usarla (versión con sincronización real)

Link: **https://mi-agenda-ten.vercel.app**

Alex la abre igual desde el celular o la computadora. La primera vez que
la abre en cada aparato, sale una pantalla oscura con un teclado
numérico (como desbloquear un iPhone) pidiendo el PIN de 6 dígitos — se
manda solo al completar los 6 números, no hay que darle a nada más. Es
siempre el mismo PIN en todos los aparatos (Claude se lo comparte a Alex
por chat, no va escrito aquí por seguridad). Una vez puesto, ese aparato
lo recuerda solo. Lo que se anota en un aparato aparece en el otro sin
hacer nada más — puede tardar algunos segundos en aparecer (ver
"Lecciones aprendidas").

Al final de la página siguen los botones "Guardar copia" / "Cargar una
copia guardada" como respaldo extra, ya no son necesarios para pasar
datos entre aparatos (eso ahora es automático), pero sirven si algún día
se quiere sacar una copia de todo.

### Partidos de Chivas (automático)

Cada día se agregan solos los próximos partidos de Chivas (Liga MX,
fuente: API pública de ESPN, mismo proveedor que usa la quiniela) como
compromisos normales, con el rival, si es local o visita, y el estadio
en la nota. Hay un botón "⚽ Actualizar partidos de Chivas" para forzarlo
al momento en vez de esperar al día siguiente. Si Chivas avanza de
ronda (ej. pasa a semifinales), ese partido nuevo aparece solo la
siguiente vez que se actualice (automático o con el botón) — apenas
ESPN lo publique. Ahora mismo solo cubre Liga MX (no Leagues Cup ni
amistosos).

### Mi semana (horario de clases)

Cuadro nuevo con una cuadrícula: los 7 días de la semana como columnas,
bloques de 2 horas (8–10, 10–12 … 20–22) como filas. Se toca un espacio
vacío para agregar algo — puede ser una **clase que se repite cada
semana** (vive en una lista aparte, `clases`) o **algo de un solo día**
(un compromiso normal, con esa fecha exacta — solo aparece esa semana,
no se repite). Los compromisos normales de la semana en curso que caen
dentro de un bloque de 2 horas también se ven ahí mismo. Tocar una
clase ya puesta la abre para editarla o eliminarla.

**Limitación conocida:** las clases se repiten *todas* las semanas sin
excepción — todavía no sabe distinguir semanas de puente/vacaciones
según el calendario de la universidad (se dejó pendiente, ver
"Decisiones importantes").

**Horario real de Alex ya cargado (2026-08-08):**
- Martes 16–18: Filosofía del Derecho — D-111
- Martes 18–20: Derecho Procesal Administrativo — D-104
- Miércoles 18–20: Derecho Procesal Laboral — en línea (videoconferencia)
- Jueves 18–20: Derecho Procesal Administrativo — D-104 (2ª sesión de la semana)
- Viernes 16–18: PAP Investigación y Práctica Jurídica — SO-W-112

Cada clase tiene un campo opcional `desde` (fecha) para que no aparezca
en semanas anteriores al inicio del semestre — se dejó en blanco/`null`
porque Alex no ha confirmado la fecha exacta de inicio (pendiente).

## Qué es

Una agenda personal en forma de página web, estilo "hoja de calendario de
taco" (papel, rojo, números grandes). Reproduce una app que Alex ya tenía
en otro ordenador. Existen dos versiones:

1. **Versión con sincronización (la que usa Alex ahora):** vive en
   `/Users/alexloza/mi-agenda` (carpeta/repo aparte de este cuaderno de
   notas, igual que la quiniela), desplegada gratis en Vercel. Tiene una
   página (`index.html`, mismo diseño que la v1) + una función pequeña de
   servidor (`api/agenda.js`) que guarda los datos en un almacenamiento de
   archivos de Vercel ("Vercel Blob").
2. **Versión de un solo archivo, sin internet:** sigue viviendo en
   `proyectos/mi-agenda/index.html` en este repositorio. Se abre con
   doble clic, no necesita internet, pero **no se sincroniza** con el
   celular — cada aparato guarda sus propios datos ahí. Se deja como
   respaldo/alternativa si algún día no hay internet.

## Decisiones importantes

- (2026-08-04) — Formato exacto de cada compromiso guardado:
  `{id, titulo, fecha:"AAAA-MM-DD", hora:"HH:MM" o null, nota o null,
  hecho:true/false}`. Se mantuvo igual en ambas versiones.
- (2026-08-04) — Alex pidió que se viera lo mismo en el celular y la
  computadora automáticamente. Primer intento: usar su cuenta de Google
  (Drive/Sheets) conectada a Claude como almacenamiento compartido, pero
  **el conector de Google Drive no tenía ninguna herramienta disponible**
  para que Claude la usara. Ver
  [[conector-google-drive-sin-herramientas]] en lecciones generales.
- (2026-08-06) — **Se construyó una app real con hosting propio**, igual
  que la quiniela: cuenta de Vercel de Alex (la misma de la quiniela,
  `alexlozaoliveros-3295`/`alexxxx2`), proyecto nuevo `mi-agenda`,
  guardando los datos en Vercel Blob (no se usó Postgres/Neon como la
  quiniela porque acá solo hace falta guardar una listita de compromisos,
  no tablas relacionadas — un solo archivo JSON en Blob alcanza y es más
  simple).
- (2026-08-06) — **Protección con PIN**, no con cuentas de
  usuario/contraseña como la quiniela: como es una agenda de una sola
  persona (no hay que distinguir entre varios usuarios), un PIN de 6
  dígitos compartido entre los aparatos de Alex es suficiente. El PIN
  vive como variable de entorno `AGENDA_PIN` en Vercel — quien no lo
  tenga no puede leer ni escribir nada (se probó con un PIN incorrecto:
  no se ve ningún dato real).
- (2026-08-06) — Los botones de exportar/importar de la v1 se dejaron
  también en esta versión, como respaldo extra, aunque ya no hacen falta
  para sincronizar entre aparatos.
- (2026-08-07) — **PIN cambiado a `123456`**, con teclado numérico en
  pantalla (como iPhone) en vez de un cuadro de texto — Alex lo pidió
  así explícitamente. El PIN se sigue guardando como variable de entorno
  `AGENDA_PIN` en Vercel.
- (2026-08-07) — Partidos de Chivas: se agregan como compromisos
  normales con `origen: "chivas"` (campo nuevo, opcional, no rompe los
  compromisos viejos que no lo tienen). Esto permite que la
  sincronización sepa cuáles borrar/actualizar sin tocar nada que Alex
  haya escrito a mano. Si Alex borra un partido de Chivas a mano, puede
  volver a aparecer la próxima sincronización mientras siga siendo un
  partido futuro según ESPN — es el comportamiento esperado de "se
  actualiza sola", no un bug.
- (2026-08-07) — **Pendiente, no se construyó todavía:** que las clases
  de "Mi semana" respeten el calendario oficial de la universidad
  (ITESO) y no aparezcan en semanas de puente/vacaciones. Alex mandó el
  calendario oficial (imagen), pero es una infografía compleja con
  muchos símbolos — se prefirió no arriesgar a leerla mal y equivocar el
  horario de Alex. Si esto se retoma, pedirle a Alex las fechas exactas
  en texto plano en vez de volver a interpretar la imagen.
- (2026-08-08) — Se agregó un campo opcional `desde` (fecha) a cada
  clase de `clases`: si tiene valor, esa clase deja de mostrarse en Mi
  semana en las semanas anteriores a esa fecha (comparación simple de
  fecha ISO, sin lógica de puentes/vacaciones intermedias). Es la pieza
  que falta para resolver el punto de arriba, en cuanto Alex confirme la
  fecha de inicio de clases del semestre de otoño.

## Lecciones aprendidas

- (2026-08-04) — Antes de entregar la primera versión se probó completa
  con un navegador real automatizado (crear, editar, marcar como hecho,
  borrar, saltar de mes, ver el celular). Un CSS de más (`text-transform:
  capitalize` en el título del día seleccionado del calendario) hacía que
  "de" apareciera como "De" en frases como "Martes 4 de agosto" — se
  corrigió antes de mostrársela a Alex.
- (2026-08-06) — **Vercel cachea las respuestas de las funciones de
  servidor por default.** El primer despliegue de `api/agenda.js`
  devolvía a veces datos viejos (guardabas algo y no aparecía de
  inmediato al leerlo). La causa: faltaba mandar la instrucción
  `Cache-Control: no-store` en la respuesta — sin eso, Vercel guarda una
  copia en su red y a veces sirve esa copia vieja en vez de ejecutar la
  función de nuevo. Se corrigió agregando ese header al principio de la
  función. **Aplica a cualquier función de servidor en Vercel que
  devuelva datos que cambian** (no solo esta app).
- (2026-08-06) — **Vercel Blob (el almacenamiento de archivos que usa
  esta app) tiene unos segundos de "consistencia eventual":** después de
  guardar algo, puede tardar un par de segundos en verse reflejado si se
  lee casi al instante desde otro lugar. Se confirmó probando con dos
  navegadores simulando dos aparatos distintos — guardar en uno y leer
  en el otro esperando 1 segundo a veces mostraba datos viejos, pero
  esperando 3-4 segundos siempre mostraba lo correcto. Para el uso real
  de Alex (revisar el otro aparato en otro momento, no en el mismo
  segundo) esto no se nota.
- (2026-08-07) — **El endpoint de ESPN "horario de un equipo"
  (`/teams/{id}/schedule`) no traía los partidos futuros de Chivas**
  (solo mostraba partidos ya jugados), aunque esos partidos sí existían.
  Se encontró que el endpoint del **marcador de toda la liga**
  (`/scoreboard?dates=RANGO`) sí los tenía, filtrando después por el id
  del equipo. Mismo truco que puede servirle a la quiniela si algún día
  hace falta el calendario de un equipo específico en vez de toda la
  jornada.

## Notas sueltas

- Versión sincronizada (código real, con `git init` local pero sin
  GitHub, igual que la quiniela): `/Users/alexloza/mi-agenda`.
- Versión de un solo archivo (respaldo sin internet): este repositorio,
  `proyectos/mi-agenda/index.html`.
- Cuenta de Vercel reusada de la quiniela: `alexlozaoliveros-3295`
  (correo alexlozaoliveros@gmail.com).
