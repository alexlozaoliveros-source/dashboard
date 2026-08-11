# Pulso — bot de market-making en Polymarket (5 min crypto)

- **Tipo:** Programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-07-30
- **Última actualización:** 2026-08-04

## Qué es

Un sistema que mide, con datos reales, si existe una oportunidad ("edge")
para poner órdenes pasivas (maker) en los mercados de Polymarket que
apuestan si el precio de bitcoin/eth/sol/xrp sube o baja en ventanas de 5
minutos. **No es "un bot que ya gana dinero"** — es primero un sistema de
medición; solo si los datos confirman que hay oportunidad real se construye
la parte que opera con dinero.

El proyecto avanza en fases con "compuertas" (gates): no se pasa a la
siguiente fase sin que Alex apruebe los resultados de la anterior con datos
reales, no con promesas.

- **Fase 0 (hecha):** un programa recolector que guarda, sin parar, los
  datos de los mercados (libro de órdenes de Polymarket, precio de Binance,
  precio del oráculo que usa Polymarket para resolver). No apuesta nada,
  solo observa y guarda. Sigue corriendo de fondo, acumulando más datos.
- **Fase 1 (hecha — resultado: no hay oportunidad, ver abajo):** con esos
  datos, calcular si el precio del mercado se desvía de lo que "debería"
  valer, y si esa desviación es aprovechable después de costos reales.
- **Fase 2:** simular apuestas en vivo (dinero de mentira) durante 2 semanas.
- **Fase 3:** solo si todo lo anterior sale bien, operar con dinero real,
  empezando con un límite muy bajo (200 USDC) y límites de pérdida
  automáticos.

## Decisiones importantes

- (2026-07-30) — Alex trajo un plan técnico ya escrito (un archivo
  "CLAUDE.md" de instrucciones para el proyecto) con reglas estrictas de
  seguridad: nada de llaves privadas ni dinero real hasta la Fase 3, todo
  simulacro de ganancias debe incluir costos reales, y cada fase requiere su
  aprobación antes de avanzar.
- (2026-07-30) — Siguiendo el mismo patrón que los demás proyectos, el
  código vive aparte de este cuaderno, en `~/pulso` (su propia carpeta con
  su propio historial de cambios). Aquí solo se guarda el resumen y las
  decisiones.
- (2026-07-30) — Se empezó por la Fase 0: el programa que solo recolecta y
  guarda datos (no apuesta nada, no usa dinero, no necesita llave privada).

## Lecciones aprendidas

- _(ninguna todavía)_

- (2026-07-31) — El recolector de la Fase 0 se puso a correr solo, todo el
  tiempo: es un "servicio" de la Mac (`launchd`) que arranca automático, se
  reinicia solo si se cae, y sigue corriendo aunque cierres la terminal o
  la sesión. Se probó apagándolo a la fuerza y confirmando que vuelve a
  arrancar solo. **Importante:** si cierras la tapa del laptop (sin un
  monitor externo conectado) la Mac igual se duerme — eso es del hardware,
  ningún programa lo puede evitar. Hay que mantener la tapa abierta y la
  Mac conectada a corriente durante las 72 horas que dura la primera
  prueba.
- (2026-07-31) — Se le agregaron al plan técnico unas "reglas aprendidas"
  de un bot parecido de otra persona (qué precios NO vale la pena cotizar,
  qué señales sí ayudan) y 3 preguntas concretas que se van a responder
  con los datos propios más adelante.
- (2026-07-31) — **Bug real encontrado y corregido en las primeras horas:**
  el programa se reconectaba solo cada 10 segundos por error (mal
  interpretaba la respuesta del servidor a su propio "¿sigues ahí?"). Ya
  se arregló y se confirmó que dejó de pasar.
- (2026-07-31) — Por ese bug, el conteo de las 72 horas limpias se
  reinició a las 02:29:59 UTC del 31 de julio (no tenía sentido contar el
  tramo con el error). Debería estar listo para revisar alrededor del 3 de
  agosto de 2026.
- (2026-07-31) — Se detectó algo que **no** es un bug pero hay que decidir
  qué hacer con ello más adelante: el "libro de órdenes" de Polymarket
  aparece cruzado (algo que en teoría no debería pasar) muy seguido, pero
  se corrige solo un instante después — parece ser así como Polymarket
  publica los datos, no un error del programa. Se necesita más análisis
  antes de decidir si eso cambia cómo se mide el gate F0.
- (2026-07-31) — **Segundo bug real, encontrado al revisar el estado unas
  horas después:** uno de los 3 "oídos" del programa (el que escucha el
  precio del oráculo) se quedó 5+ minutos sin recibir nada, sin avisar que
  algo estaba mal — el programa pensaba que todo iba bien. Se corrigió
  agregando una alarma de "no he oído nada en 30 segundos, reconéctate".
  Por este bug, el conteo de las 72 horas se reinició otra vez, ahora
  desde las 03:20:53 UTC del 31 de julio. Se está revisando el estado con
  más frecuencia mientras se estabiliza.
- (2026-08-01) — **Tercer bug, el más grave hasta ahora:** el "oído" más
  importante — el que escucha el precio de Polymarket (el order book) —
  dejó de recibir datos durante **2 horas y 41 minutos**, sin ningún
  aviso de error. Los otros dos oídos (Binance, oráculo) seguían
  funcionando normal, así que los chequeos rápidos de "¿cómo vas?" no lo
  detectaron — se necesitó comparar el estado en dos momentos distintos
  para notar que uno de los contadores no se había movido nada. Causa: un
  error de red pasajero tumbó silenciosamente una de las partes del
  programa, y nadie estaba "escuchando" para darse cuenta. Se corrigió con
  dos capas de protección — la parte específica que falló ahora se
  recupera sola, y además se agregó un "supervisor" que vigila que todas
  las partes del programa sigan vivas y las reinicia solas si alguna se
  cae, sin esperar a que alguien lo note. El conteo de las 72 horas se
  reinició otra vez, desde ~05:19 UTC del 1 de agosto.
- (2026-08-01) — **Se cayó el internet de verdad (~8.4 horas, por la
  lluvia)** — esta vez no fue un bug del programa. Lo bueno: el programa no
  se murió, se quedó reintentando solo todo ese tiempo, y en cuanto volvió
  la señal se reconectó él solo en 4 minutos, sin que nadie tuviera que
  hacer nada — primera prueba real de que las correcciones anteriores
  funcionan ante una caída larga de verdad. Lo malo (inevitable): durante
  esas 8.4 horas no se guardó ningún dato, así que el conteo de las 72
  horas se reinició una vez más, desde las 15:44 UTC del 1 de agosto.

- (2026-08-04) — **Se cumplieron las 72 horas del tramo limpio, y ya hay
  reporte con números reales** (`~/pulso/report_f0.md`). La noticia, sin
  adornos: **no pasa el gate tal como está escrito**, aunque tampoco está
  lejos:
  - 3 de 5 "oídos" cumplen el límite de menos de 0.1% de cortes; los otros
    2 (el oráculo y el order book de bitcoin) lo pasan por poco (0.17% y
    0.20% en vez de 0.1%).
  - El "libro cruzado" (algo que la regla original dice que nunca debería
    pasar) sí pasa, miles de veces — pero ya se había visto antes que
    parece autocorregirse solo casi al instante, no un error real. No se
    revisó eso a fondo sobre las 73 horas completas, solo una muestra.
  - No se cambió ninguna regla por cuenta propia. Hay 2 decisiones
    pendientes que le tocan a Alex: si se ajusta cómo se mide lo del
    "libro cruzado" (o se investiga más primero), y si vale la pena
    corregir el problema de reconexión antes de repetir la prueba o si
    0.17%/0.20% ya es aceptable.

- (2026-08-04) — **Se hizo la Fase 1 completa, y el resultado es que NO
  hay una oportunidad real de ganar dinero con esta estrategia** — al
  menos no con los datos de estos días. Se lo digo tal cual porque así lo
  pediste desde el principio: un "no" con datos también es un resultado
  válido, no un fracaso del sistema.
  - Se probaron 12 formas distintas de "cotizar" (comprar barato / vender
    caro, en bitcoin y ethereum, con distintos márgenes) usando los ~4.7
    días de datos guardados. **Las 12 salieron perdiendo dinero**, de forma
    consistente y no por casualidad.
  - Se revisó con más detalle (132 combinaciones, separando también por
    momento de la ventana) y solo 2 salieron ganando — pero esa cantidad es
    **menos** de lo que se esperaría por puro azar probando tantas
    combinaciones a la vez, así que no cuenta como una oportunidad real.
  - Se probaron también 3 ideas concretas basadas en lo que le funcionó a
    otra persona con un bot parecido: ninguna revirtió el resultado.
  - Reporte completo con todos los números en `~/pulso/report_f1.md`.
  - El colector de datos se deja corriendo igual — los datos guardados
    siguen siendo valiosos por si más adelante cambian las condiciones del
    mercado y vale la pena repetir el análisis.

- (2026-08-11) — **Se cerró la versión propia (Mac) y se reemplazó por la
  versión de un amigo, corriendo en un servidor real.** Con el resultado
  negativo de la Fase 1 confirmado, Alex decidió no seguir con su propia
  copia y usar en su lugar el paquete `pulso-amigo` que le compartió un
  amigo suyo (que construyó un bot parecido de forma independiente — ver
  la "advertencia entre amigos" en el propio `EMPIEZA_AQUI.md` del
  paquete, sobre no pisarse si ambos terminan operando el mismo nicho).
  - Se apagó y se quitó el servicio (`launchd`) del colector viejo en la
    Mac. Sus datos y reportes (`report_f0.md`, `report_f1.md`) se dejaron
    intactos en `~/pulso` por si se quieren consultar después.
  - Se montó un servidor nuevo en Hetzner (Helsinki, CX23, ~$6.49/mes) con
    llave SSH propia (`~/.ssh/pulso_amigo`), siguiendo al pie de la letra
    la guía `EMPIEZA_AQUI.md` del paquete (esa guía advierte que una
    laptop de casa no sirve para esto: se duerme, compite por CPU, el
    internet de casa falla de noche).
  - Blindaje aplicado: solo entra por llave SSH (contraseña y root por
    contraseña desactivados), firewall (`ufw`) solo con SSH abierto,
    `fail2ban`, actualizaciones de seguridad automáticas, usuario sin
    privilegios (`pulso`) para el día a día.
  - Colector corriendo como servicio `systemd` (`pulso-collector`,
    reinicio automático si se cae), con los 5 flujos de datos conectados
    (Bitcoin, Ethereum, oráculo x2, Binance) confirmados en `status.json`.
  - Alertas al celular de Alex vía `ntfy.sh` (tema privado
    `pulso-amigo-alex-7f69ed98957c`), revisando cada 5 min por cron:
    servicio caído, heartbeat congelado, algún stream mudo >5 min, o disco
    <10% libre — máximo 1 aviso por hora por tipo de problema. Probado y
    confirmado que le llega a su teléfono.
  - **Reloj limpio del gate F0 reiniciado desde ~2026-08-11 04:14 UTC** en
    el servidor nuevo (es una base de código y una máquina distintas a las
    del intento anterior, así que no tendría sentido heredar el conteo).

## Notas sueltas

- El detalle técnico completo (cómo se conecta a cada fuente de datos, qué
  archivos genera, qué debe cumplir cada fase para aprobarse) vive en
  `CLAUDE.md` dentro del paquete `pulso-amigo` (copiado al servidor en
  `/home/pulso/pulso/CLAUDE.md`). La copia vieja en `~/pulso/CLAUDE.md` (Mac)
  ya no es la que está activa.
- El paquete original que dio origen a esta versión sigue en
  `~/Downloads/pulso-amigo` (y sus .zip hermanos) por si hace falta
  reconsultarlo.
