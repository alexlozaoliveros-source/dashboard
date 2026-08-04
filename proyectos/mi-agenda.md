# Mi agenda (agenda personal web)

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-04
- **Última actualización:** 2026-08-04

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
