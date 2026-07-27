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
  que cargarlos a mano. Se investigó y sí existe una fuente gratuita
  (API-Football) que cubre Liga MX, incluyendo marcador y goleadores, sin
  costo para un grupo chico. El único detalle: para ligas "no top" como
  Liga MX, los datos de goleador a veces llegan incompletos o tarde justo
  después del partido (el marcador final sí es confiable siempre). Por
  eso: el marcador/ganador se calcula 100% automático, y el goleador
  también se llena 100% automático (sin que nadie tenga que confirmar cada
  partido). Lo que sí existe es un botón de "solicitar revisión" (ver
  detalle en la sección de especificación funcional más abajo).
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
  "viernes botanero" con código G583M4. Siguiente paso: Fase 3, traer la
  tabla real de la Liga MX, goleadores, y los partidos de la jornada.

## Notas sueltas

- Si alguien pronostica más o menos goles de un jugador de los que metió
  en realidad, se cuenta el número que sí coincide (ej. predijo 2 goles de
  un jugador y solo metió 1 real → cuenta 1 punto extra, no 2). Los
  autogoles no se le pueden "atinar" a nadie, pero sí cuentan para el
  marcador final.
- Notificaciones: van a ser notificaciones push gratis desde la propia
  app (como las de WhatsApp pero de la app de la quiniela), no SMS de
  pago. En iPhone requiere agregar la página a la pantalla de inicio una
  vez (limitación de Apple).
- La app real se va a construir en una carpeta/repo aparte (no en este
  repositorio de notas), con Next.js + Vercel (hosting gratis) +
  API-Football (datos reales de Liga MX, gratis). El plan técnico
  completo quedó guardado como plan de Claude Code el 2026-07-27 (fases:
  cimientos → crear/entrar a quiniela → datos reales de Liga MX →
  pronósticos y puntos → notificaciones → revisión de resultados).
- Importante: por los límites de las cuentas gratis, la tabla, los
  goleadores y los puntos se actualizan una vez al día (de madrugada), no
  en vivo durante los partidos.
- Para arrancar la construcción, Alex tiene que crear 2 cuentas gratis
  (Vercel y api-football.com) — Claude Code lo guía paso a paso por chat.
