# Mi agenda (agenda personal web)

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-04
- **Última actualización:** 2026-08-04

## Cómo pasar los datos entre el celular y la computadora

No hay sincronización automática (ver más abajo por qué). Hay dos botones
al final de la página: **"Guardar copia de mis datos"** (descarga un
archivo) y **"Cargar una copia guardada"** (lo vuelve a subir). Para
pasarlo de un aparato a otro, Alex tiene que mandarse ese archivo él mismo
(por WhatsApp, AirDrop, correo, etc.) y luego usar "Cargar" en el otro
aparato. Cargar una copia **reemplaza** todo lo que haya en ese aparato en
ese momento (no mezcla).

## Qué es

Una agenda personal en forma de página web de un solo archivo
(`proyectos/mi-agenda/index.html`). Se abre con doble clic, no necesita
internet ni instalar nada, y guarda los compromisos directamente en el
navegador donde se abre. Reproduce una app que Alex ya tenía en otro
ordenador, con estilo "hoja de calendario de taco" (papel, rojo, números
grandes).

## Decisiones importantes

- (2026-08-04) — Un solo archivo `index.html` con todo dentro (diseño y
  funcionamiento), sin servidor ni dependencias externas salvo la
  tipografía "Archivo" de Google Fonts (si no hay internet, cae
  automáticamente a la letra del sistema sin romperse).
- (2026-08-04) — Los datos viven en el almacenamiento local del navegador
  (localStorage), bajo la clave `mi-agenda-v1`. Esto significa que **los
  compromisos quedan guardados en el navegador de cada computadora**, no
  se sincronizan solos entre el celular y la computadora, y si algún día
  Alex borra los datos de navegación de Chrome/Safari puede perderlos (por
  eso se le va a proponer una copia de seguridad).
- (2026-08-04) — Formato exacto de cada compromiso guardado:
  `{id, titulo, fecha:"AAAA-MM-DD", hora:"HH:MM" o null, nota o null,
  hecho:true/false}`.
- (2026-08-04) — Alex pidió un link para que se viera lo mismo en el
  celular y la computadora automáticamente. Se intentó usar su cuenta de
  Google (Drive/Sheets) conectada a Claude como almacenamiento
  compartido, pero **el conector de Google Drive no tenía ninguna
  herramienta disponible** para que Claude la usara (mensaje: "Este
  conector no tiene herramientas disponibles"), así que ese camino quedó
  descartado por ahora. Ver [[conector-google-drive-sin-herramientas]] en
  lecciones generales. En su lugar se agregó una copia de
  seguridad manual (exportar/importar un archivo) — ver sección de
  arriba.

## Lecciones aprendidas

- (2026-08-04) — Antes de entregar la app se probó completa con un
  navegador real automatizado (crear, editar, marcar como hecho, borrar,
  saltar de mes, ver el celular). Un CSS de más (`text-transform:
  capitalize` en el título del día seleccionado del calendario) hacía que
  "de" apareciera como "De" en frases como "Martes 4 de agosto" — se
  corrigió antes de mostrársela a Alex.

## Notas sueltas

- El archivo vive en `proyectos/mi-agenda/index.html` dentro de este
  mismo repositorio, para que quede junto con el resto de la memoria de
  proyectos.
