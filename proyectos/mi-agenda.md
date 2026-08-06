# Mi agenda (agenda personal web)

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-04
- **Última actualización:** 2026-08-06

## Cómo usarla (versión con sincronización real)

Link: **https://mi-agenda-ten.vercel.app**

Alex la abre igual desde el celular o la computadora. La primera vez que
la abre en cada aparato, pide un PIN — es siempre el mismo PIN, lo
comparte Claude con Alex por chat (no va escrito en este archivo por
seguridad). Una vez puesto, ese aparato lo recuerda solo. Lo que se anota
en un aparato aparece en el otro sin hacer nada más — puede tardar
algunos segundos en aparecer (ver "Lecciones aprendidas").

Al final de la página siguen los botones "Guardar copia" / "Cargar una
copia guardada" como respaldo extra, ya no son necesarios para pasar
datos entre aparatos (eso ahora es automático), pero sirven si algún día
se quiere sacar una copia de todo.

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

## Notas sueltas

- Versión sincronizada (código real, con `git init` local pero sin
  GitHub, igual que la quiniela): `/Users/alexloza/mi-agenda`.
- Versión de un solo archivo (respaldo sin internet): este repositorio,
  `proyectos/mi-agenda/index.html`.
- Cuenta de Vercel reusada de la quiniela: `alexlozaoliveros-3295`
  (correo alexlozaoliveros@gmail.com).
