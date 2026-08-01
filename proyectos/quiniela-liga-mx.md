# Quiniela Liga MX

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-07-27
- **Última actualización:** 2026-07-27

## Qué es

Una aplicación web (quiniela) para que un grupo de amigos (2 a 10 personas)
pronostique los resultados de cada jornada de la Liga MX: quién gana, el
marcador, y quién mete los goles. La app calcula los puntos de cada quien
automáticamente, muestra un ganador por jornada y, al final del torneo, un
ganador general.

## Decisiones importantes

- (2026-07-27) — Acceso: será una app web con un link. Cada amigo la abre
  desde su celular o computadora para meter sus pronósticos (no por
  WhatsApp ni a mano).
- (2026-07-27) — Resultados: se buscarán automáticamente, sin que Alex tenga
  que cargarlos a mano. El marcador/ganador se calcula 100% automático, y
  el goleador también se llena 100% automático (sin que nadie tenga que
  confirmar cada partido). Lo que sí existe es un botón de "solicitar
  revisión" (ver detalle en la sección de especificación funcional más
  abajo).
- (2026-07-27) — **Cambio de fuente de datos.** La idea original era usar
  API-Football, pero se descubrió construyendo que su plan gratis **no
  incluye la temporada que se está jugando ahora** (solo temporadas viejas
  2022-2024) — para eso hay que pagar mínimo $19 usd/mes. Alex prefirió no
  pagar y buscar otra fuente gratis. Se encontró una API pública de ESPN
  (sin necesidad de cuenta ni clave) que sí da datos de la jornada actual:
  tabla de posiciones, goleadores, y hasta quién anotó cada gol por
  partido. Es gratis y funciona bien, pero es una API "no oficial" (ESPN
  no la documenta ni promete que siga funcionando igual para siempre) —
  es el trade-off que Alex aceptó a cambio de no pagar.
- (2026-07-27) — Participantes: grupo chico, entre 2 y 10 amigos.
- (2026-07-27) — Sistema de puntos por partido:
  - **5 puntos:** marcador exacto (incluye acertar quién gana).
  - **3 puntos:** solo acierta el resultado (quién gana o empate), sin
    marcador exacto.
  - **Bonus goleador:** 1 punto extra por cada gol cuyo autor se adivinó
    correctamente. Si un jugador mete 2 goles y Alex lo puso como anotador
    de esos 2 goles, son 2 puntos extra (no 1).
  - Ejemplo usado para confirmar la regla: alguien pronostica 2-1 con el
    mismo jugador anotando los 2 goles del equipo que gana. El resultado
    real es 3-1 y ese mismo jugador metió esos 2 goles reales → gana 3
    puntos (acertó quién gana, no el marcador exacto) + 2 puntos de
    goleador = 5 puntos totales.

## Especificación funcional (detallada por Alex el 2026-07-27)

**Entrada / roles**
- Un usuario nuevo ve dos opciones al abrir la app: "Crear quiniela" o
  "Ingresar a una" (con un código).
- Quien crea la quiniela es el **líder**: puede verificar resultados, sacar
  participantes, y es quien comparte el código de acceso con los demás.
- Cada quien se registra con su nombre al entrar, para que todos sepan
  quiénes están participando.

**Página principal de una quiniela**
- Nombre de la quiniela (lo pone el líder al crearla).
- Resultado de la jornada anterior (actualmente van en la jornada 2).
- Tabla de posiciones real de la Liga MX: partidos ganados/perdidos/
  empatados, goles a favor/en contra, y los últimos 5 partidos de cada
  equipo como círculos de color (verde = victoria, gris = empate, rojo =
  derrota).
- Tabla de goleadores de la Liga MX.
- El "juego" (la quiniela en sí, ver abajo).
- Botón resaltado "Jornada (siguiente)" con el número de la próxima
  jornada — al presionarlo aparecen todos los partidos de esa jornada en
  orden (del primero al último).
- Tabla de todos los participantes de esa quiniela, ordenada de quien va
  ganando a quien va perdiendo.
- Arriba, en grande: la posición actual del usuario en la quiniela (medalla
  de oro/plata/bronce si va 1º/2º/3º), y más chico al lado, la posición que
  tenía en la jornada anterior.
- Al final de la página: tabla de puntos ganados solo en la última
  jornada, ordenada de mejor a peor.
- Regla general: todas las tablas siempre se ordenan de mejor a peor.

**Cómo se pronostica cada partido**
- Los partidos de la jornada aparecen en orden, uno después del otro.
- Primero se elige el marcador.
- Si el marcador queda 0-0, no aparece nada de goleadores.
- Si hay goles, aparece la lista de jugadores del equipo (o equipos) que
  anotaron, para marcar quién metió cada gol. Se puede repetir el mismo
  jugador si metió más de un gol.
- Si un equipo se quedó en 0 goles, su lista de jugadores no aparece.

**Notificaciones y cierre de pronósticos**
- Dos días antes de que empiece la jornada, se debe avisar al teléfono de
  cada participante que le falta contestar.
- Si un partido específico ya empezó y la persona no puso su pronóstico
  para ese partido, ya no puede hacerlo y ese partido le suma 0 puntos. Los
  demás partidos de la jornada que aún no han empezado los puede seguir
  pronosticando normalmente.

**Verificación de goleadores**
- El sistema llena el goleador automático (ver decisión de arriba sobre
  API-Football).
- Tiene que existir una opción de "solicitar revisión" — la puede pedir
  cualquier participante o el propio Alex si detectan un error. Al
  solicitarla, se revisa el dato de nuevo y el líder da la confirmación
  final.

## Lecciones aprendidas

- _(ninguna todavía)_

## Avance de construcción

- (2026-07-27) — **Fase 1 (Cimientos) completada.** Se creó la cuenta de
  Vercel y la de api-football.com (ambas a nombre de Alex, correo
  alexlozaoliveros@gmail.com). El código vive en
  `/Users/alexloza/quiniela-liga-mx` (carpeta y repositorio aparte de este
  cuaderno de notas). La app ya está viva en:
  **https://quiniela-liga-mx-omega.vercel.app** (por ahora solo muestra
  una página de bienvenida, es la prueba de que todo el sistema
  funciona).
- (2026-07-27) — **Fase 2 (crear/entrar a una quiniela) completada y
  probada por Alex.** Ya se puede crear una quiniela (nombre + tu nombre +
  PIN de 4 dígitos) y queda como líder quien la crea, con un código de 6
  caracteres para invitar amigos. Probado en vivo: Alex creó la quiniela
  "viernes botanero" con código G583M4.
- (2026-07-27) — **Fase 3 (datos reales de Liga MX) completada.** Tabla de
  posiciones, goleadores, resultado de la jornada anterior y detección de
  la jornada siguiente ya funcionan con datos reales (fuente: API de
  ESPN). Actualmente van en la jornada 2 de 17 del torneo. Los datos se
  actualizan solos una vez al día de madrugada; también se puede forzar
  la actualización a mano.
- (2026-07-28) — **Fase 4 (pronosticar) completada.** Ya se puede entrar a
  "Jornada (siguiente)" y poner el marcador de cada partido; si hay goles,
  aparece la lista de jugadores del equipo que anotó para elegir quién los
  metió (repitiendo jugador si metió más de uno). Se cierra solo por
  partido en cuanto arranca.
- (2026-07-28) — **Cambio de PIN a usuario y contraseña.** Alex prefirió
  cuentas de verdad (usuario + contraseña) en vez del PIN de 4 dígitos, y
  que la sesión quede abierta hasta que alguien cierre sesión (no que se
  desconecte solo). Se cambió a un sistema de cuentas donde una misma
  cuenta puede pertenecer a varias quinielas. Pantallas: "Crear quiniela" /
  "Ingresar a una" (piden nombre + código + usuario/contraseña, la primera
  vez) y una nueva "Iniciar sesión" (solo usuario/contraseña) para volver a
  entrar después. Este cambio borró la quiniela de prueba "viernes
  botanero" (no había datos reales todavía, se puede volver a crear en
  segundos). Alex hizo una cuenta en Supabase por su cuenta pensando que
  hacía falta para esto — no se necesita, ya se resolvió con lo que ya
  estaba armado (Vercel + Neon); puede dejarla sin usar o borrarla.
- (2026-07-28) — **Separación cuenta / quiniela.** Alex pidió que la
  cuenta (usuario) sea independiente de la quiniela: ahora la pantalla de
  inicio solo pregunta "Iniciar sesión" o "Crear cuenta" (correo, usuario,
  contraseña). Al entrar, cae en una pantalla de "Perfil": si ya tiene
  quiniela(s), las ve ahí para entrar; si no tiene, ve la opción de crear
  una o unirse a una. Desde dentro de una quiniela hay un botón "← Mis
  quinielas" (arriba a la izquierda) para entrar a otra o crear otra sin
  perder la cuenta — una misma cuenta puede estar en varias quinielas a la
  vez. Esto también reinició la data de prueba otra vez (agregar el correo
  como dato obligatorio lo requería).
- (2026-07-28) — **Nuevo sistema de goleador (reemplaza el anterior).** Ya
  no se elige quién mete cada gol de cada partido (era tedioso, muchos
  jugadores desconocidos). Ahora, una vez por jornada, cada quien elige
  **3 jugadores de cualquier equipo** (con buscador y filtro por equipo).
  Por cada gol real que meta alguno de esos 3 jugadores en esa jornada,
  suma **3 puntos** (si mete 2 goles, son 6 puntos, etc.). El resto del
  sistema de puntos (5 marcador exacto / 3 solo resultado) no cambió.
- (2026-07-28) — **Pronosticar ahora es paso a paso.** Al entrar a
  "Jornada (siguiente)" sale un solo partido a la vez (con escudos de los
  equipos); al darle "Enviar" pasa al siguiente, en orden, hasta terminar
  todos. Después sale la pantalla para elegir a los 3 goleadores.
- (2026-07-28) — **Motor de puntos construido.** Ya se calculan solos: en
  cuanto todos los partidos de una jornada terminan (durante la
  actualización diaria), se calculan los puntos de marcador de cada
  pronóstico y el bonus de goleadores, y quedan guardados por jornada.
  Falta todavía mostrar la tabla de posiciones de la quiniela (con
  medallas) en la pantalla principal — los datos ya existen, solo falta la
  parte visual.
- (2026-07-28) — Escudos de los equipos agregados (vienen gratis de la
  misma fuente de datos) en la tabla de posiciones y en la pantalla de
  pronósticos.
- (2026-07-29) — **Pronósticos ya no se pueden editar una vez enviados.**
  Antes se podía volver a cambiar el marcador o los goleadores mientras no
  hubiera empezado el partido/jornada; ahora, en cuanto envías un marcador
  o tus 3 goleadores, queda final. El botón verde de "Jornada (siguiente)"
  desaparece del panel principal en cuanto terminaste tu pronóstico
  completo de esa jornada (ya no hay nada que hacer ahí).
- (2026-07-29) — **Pantalla de "cómo funciona".** Al iniciar sesión, antes
  de entrar al perfil/quiniela, sale una pantalla explicando qué hay que
  hacer cada jornada y cómo se reparten los puntos (5 marcador exacto, 3
  solo resultado, 3 por cada gol de goleador elegido).
- (2026-07-29) — **Rediseño visual completo.** Alex mandó una plantilla de
  Canva de referencia (tema cancha de fútbol: césped verde, foto de balón
  en la portería, tipografía gruesa tipo cartel deportivo, menús con
  números grandes "01/02/03", ilustraciones tipo boceto). Se aplicó un
  sistema de diseño nuevo a toda la app: fondo crema con verde pasto,
  encabezados con franjas de césped (como visto desde arriba de una
  cancha), tipografía Anton (gruesa, para títulos) + Oswald (para
  etiquetas), y las pantallas de "cómo funciona" y "perfil" con el
  formato numerado tipo índice. Instalado con el skill de diseño
  `frontend-design`.
- (2026-07-29) — **Tabla de posiciones + historial.** Donde antes solo
  salía la lista de participantes, ahora sale una tabla de posiciones real
  (con medallas 🥇🥈🥉 y puntos acumulados). Si le dan clic, se abre un
  historial con las jornadas ya terminadas (una vez que se calculan sus
  puntos): ahí se ve, jornada por jornada, qué pronosticó cada quien —
  marcador de cada partido y sus 3 goleadores — incluyendo lo que
  pronosticaron los demás participantes (solo de jornadas ya cerradas, no
  de la actual). Como la jornada 3 apenas va a terminar (según Alex, como
  el lunes), todavía no aparece nada ahí — es normal, aparecerá sola en
  cuanto se calculen sus puntos.
- (2026-07-29) — **Dos bugs reportados por el primer amigo real que se
  metió.** 1) La pantalla de "cómo funciona" solo salía al iniciar sesión
  (para quien ya tenía cuenta), pero no al entrar con el código por
  primera vez — se corrigió para que salga justo después de entrar/crear
  una quiniela, que es cuando de verdad hace falta. 2) La tabla de
  posiciones solo mostraba a quien ya tenía puntos calculados, así que
  alguien que se acababa de unir no aparecía en ningún lado — se corrigió
  para que siempre se parta de la lista real de participantes (con 0
  puntos si todavía no ha jugado ninguna jornada).
- (2026-07-29) — Tres ajustes más: 1) se corrigió un bug donde el
  marcador del partido anterior se quedaba puesto al pasar al siguiente
  (ahora siempre empieza en 0-0). 2) Se reemplazó el escudo de América por
  uno personalizado que mandó Alex (una rata sobre el escudo, sin fondo
  blanco) — vive en el propio proyecto (`public/escudos/america-rata.png`)
  y se aplica automático en la sincronización diaria. 3) Se agregó un
  recuadro de "Cómo se reparten los puntos" justo debajo del código para
  invitar amigos, en el panel principal.
- (2026-07-30) — Ícono de la app: cuando alguien agregue la página a su
  celular (pantalla de inicio) o la vea en la pestaña del navegador, ahora
  sale una foto de un árbitro señalando, que mandó Alex.
- (2026-07-31) — Alex pidió que la tabla se actualice al momento de que
  termine cada partido. Se probó subir la frecuencia de la actualización
  automática, pero el plan gratis de Vercel bloquea cualquier cosa más
  seguida que una vez al día (lo confirmó al intentar desplegarlo). Para
  no tener que crear otra cuenta externa, se agregó un botón de
  "🔄 Actualizar resultados ahora" dentro del panel principal — cualquiera
  lo puede presionar cuando quiera ver los resultados más recientes al
  momento, sin esperar a la actualización automática de la madrugada.
- (2026-07-31) — Los puntos de marcador (5/3) ahora se calculan al momento
  en que cada partido termina, no hasta que cierra toda la jornada — así
  la tabla de posiciones se ve viva mientras se juega. El bonus de
  goleadores sigue esperando a que termine la jornada completa (no se
  puede saber a medias, porque un jugador puede meter goles en varios
  partidos de la misma jornada).
- (2026-07-31) — Alex hizo notar que un jugador solo juega un partido por
  jornada, así que en cuanto termina ESE partido específico ya se sabe si
  metió gol o no — no hace falta esperar a que cierre toda la jornada para
  el bonus de goleador. Se corrigió: el bonus de goleador (3 pts por gol)
  ahora también se suma al momento, en cuanto termina el partido del
  jugador elegido. Además, el historial (se abre tocando la tabla de
  posiciones) ahora muestra la jornada en curso con los partidos ya
  jugados, aunque la jornada completa no haya cerrado.
- (2026-08-01) — Junto a cada goleador elegido (en el panel principal y en
  el historial) ahora sale una ✓ por cada gol que metió, o una ✗ si su
  partido ya se jugó y no metió gol. Si su partido todavía no se juega, no
  sale ninguna marca.

## Notas sueltas

- Notificaciones: van a ser notificaciones push gratis desde la propia
  app (como las de WhatsApp pero de la app de la quiniela), no SMS de
  pago. En iPhone requiere agregar la página a la pantalla de inicio una
  vez (limitación de Apple).
- La app real se vive en una carpeta/repo aparte (no en este
  repositorio de notas): `/Users/alexloza/quiniela-liga-mx`, con Next.js +
  Vercel (hosting gratis) + la API pública de ESPN (datos reales de Liga
  MX, gratis). El plan técnico completo quedó guardado como plan de
  Claude Code el 2026-07-27.
- Importante: por los límites de las cuentas gratis, la tabla, los
  goleadores y los puntos se actualizan una vez al día (de madrugada), no
  en vivo durante los partidos.
- Para arrancar la construcción, Alex tiene que crear 2 cuentas gratis
  (Vercel y api-football.com) — Claude Code lo guía paso a paso por chat.
