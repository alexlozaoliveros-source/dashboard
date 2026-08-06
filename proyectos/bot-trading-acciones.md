# Bot de trading de acciones (análisis y alertas)

- **Tipo:** Programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-05
- **Última actualización:** 2026-08-06

## Qué es

Un sistema que vigila el mercado de acciones en vivo, aplica varias estrategias
técnicas (medias móviles, RSI, rupturas, VWAP, etc.) y avisa por Telegram
cuándo detecta una oportunidad de comprar barato / vender caro. Es un
proyecto **separado de Pulso** (ese es sobre apuestas de 5 minutos en
Polymarket con cripto; este es sobre acciones con un bróker real).

Igual que Pulso, avanza por fases con aprobación de Alex entre cada una: no
se arriesga dinero real hasta validar con datos reales que la estrategia
funciona.

- **Fase 1:** estructura del proyecto + datos históricos + 2 estrategias
  (medias móviles y RSI) + backtester básico.
- **Fase 2:** resto de estrategias + motor de señales combinado + métricas.
- **Fase 3:** datos en vivo + alertas por Telegram (solo avisos, sin operar).
- **Fase 4:** analizar un día completo de datos reales en vivo para ver si
  conviene comprar/vender (Alex propuso el 6 de agosto para esto, pero
  depende de que las Fases 1-3 ya estén listas y probadas).
- **Fase 5:** noticias + robustez/auto-recuperación.
- **Fase 6:** diario de operaciones + aprendizaje automático de pesos.
- **Fase 7 (nueva, no estaba en el plan original):** modo autónomo — el
  programa opera solo, con dinero real, sin que Alex apruebe cada operación.
  Solo se activa si las fases anteriores confirman que hay ganancia real, y
  con límites de pérdida automáticos.

## Decisiones importantes

- (2026-08-05) — Alex trajo un plan técnico ya escrito (parecido al que usó
  para Pulso) con reglas de seguridad: sin apalancamiento por defecto, con
  backtesting y validación antes de arriesgar dinero real.
- (2026-08-05) — Es un proyecto nuevo, no una continuación de Pulso.
- (2026-08-05) — Mercado: acciones (bróker tradicional), no cripto.
- (2026-08-05) — **Decisión importante sobre el modo de operar:** Alex
  quiere que, eventualmente, el programa compre/venda solo con dinero real,
  sin que él apruebe cada operación — esto no estaba en las "reglas
  absolutas" que él mismo escribió (que decían que las alertas son solo un
  aviso y que él decide). Se le explicó el riesgo (un bug puede perder
  dinero solo, sin nadie que lo frene a tiempo — como pasó tres veces con
  bugs reales en Pulso, aunque ahí no había dinero en juego). **Acordamos
  que el modo autónomo solo se activa después de que las fases de
  validación (backtest + varios días en vivo sin dinero real) confirmen que
  la estrategia de verdad gana dinero, y con topes de pérdida automáticos.**
  No se activa desde el día uno.
- (2026-08-05) — **Bróker confirmado: Alpaca.** Permite datos, simular con
  dinero falso y (más adelante) operar con dinero real, todo con la misma
  conexión. No es 100% gratis: el plan gratis da datos en tiempo real pero
  solo de una bolsa (IEX) y con límite de 200 consultas/minuto — alcanza
  para construir y probar (Fases 1-2). El plan de pago ($99 USD/mes) da
  datos de todas las bolsas juntas y un límite mucho más alto — probable
  que se necesite recién en la Fase 3 (vigilar el mercado en vivo). No se
  paga nada todavía.
- (2026-08-05) — Telegram: no existe todavía un bot para las alertas, hay
  que crearlo (son unos pasos simples hablando con @BotFather en Telegram).
- (2026-08-05) — **No se usa una lista fija de acciones grandes** (Alex lo
  pidió explícitamente: quiere day trading de minutos, con acciones de
  precio bajo y volumen alto, no empresas conocidas). En vez de una lista
  fija, el sistema arma un "filtro" que cada día revisa el mercado completo
  y se queda con las acciones entre ~$1 y $20 con volumen alto ese día —
  esas son las "candidatas" a las que luego se les aplican las estrategias
  en detalle. Revisar miles de acciones en detalle a la vez (segundo a
  segundo) no es realista ni siquiera pagando — se hace un filtro barato
  primero sobre todo el mercado, y el análisis fino solo sobre la lista
  corta que queda.

- (2026-08-06) — **Alex pidió no avanzar de la Fase 2 hasta encontrar
  estrategias con 90% de acierto** (después lo bajó a "3 de cada 4"),
  buscando entre muchas variantes sin parar, incluso creando estrategias
  propias, y usando las herramientas de investigación necesarias. Se le
  explicó un problema real antes de hacerlo así: buscar entre muchas
  combinaciones apuntando a un número fijo, sin separar datos de prueba y
  confirmación, casi garantiza "encontrar" algo que parece bueno por pura
  casualidad — ya le había pasado a Pulso (132 combinaciones, y hasta las 2
  "ganadoras" eran menos de lo esperable por azar). **Alex aceptó el
  enfoque corregido: buscar en serio, pero medir por ganancia promedio
  después de costos, confirmada con datos que el sistema no vio antes
  (walk-forward), no por tasa de acierto sola.**
- (2026-08-06) — Alex se fue a dormir y pidió seguir solo, sin preguntar,
  sin parar hasta conseguirlo. Se siguió trabajando toda la noche con el
  método acordado (ver resultado completo abajo, en "Notas sueltas").

## Lecciones aprendidas

- (2026-08-05) — Cuando Alex pega un plan técnico que dice tener "reglas
  absolutas e innegociables", vale la pena revisar sus respuestas contra
  esas reglas antes de anotarlas — puede pedir algo en el chat que
  contradiga lo que él mismo escribió (como pasó aquí con "que el programa
  opere solo"), sin darse cuenta del cambio. Conviene señalarlo y confirmar
  antes de seguir. Ver también la lección de Pulso: no hay ventaja
  confirmada hasta que los datos reales lo demuestren.
- (2026-08-06) — **"Buscar hasta encontrar X% de acierto" es una trampa
  estadística, no una meta razonable.** Si se prueban muchas estrategias o
  variantes apuntando a un número fijo, casi seguro se "encuentra" algo que
  lo cumple por pura casualidad en los datos ya pasados — y casi seguro
  falla con dinero real, porque nunca hubo ventaja de verdad. La forma
  correcta: separar datos de "prueba" y "confirmación" que nunca se
  mira hasta el final (walk-forward), y corregir el umbral de confianza
  según cuántas cosas se probaron a la vez (corrección por pruebas
  múltiples). Medir por ganancia promedio después de costos, no por tasa
  de acierto sola (una estrategia puede ganar dinero acertando solo 1 de
  cada 3 veces, si cuando gana gana mucho más de lo que pierde cuando
  pierde).
- (2026-08-06) — Al construir el primer script de búsqueda automática, se
  cometió un error real: marcaba como "candidata real" cualquier resultado
  estadísticamente significativo, sin fijarse si la ganancia era positiva o
  negativa — así que reportaba pérdidas muy consistentes como si fueran
  buenas noticias. Se detectó al revisar los números antes de reportarlos
  a Alex, y se corrigió. Lección: "es estadísticamente significativo" no
  es lo mismo que "es bueno" — hay que revisar siempre el signo.

## Notas sueltas

- El código vive en `~/bot-trading-acciones` (carpeta propia, con su
  propio historial de cambios, igual que Pulso vive en `~/pulso`).
- (2026-08-05) — **Fase 1 construida y probada:** estructura del proyecto,
  el filtro de acciones candidatas (precio $1-$20, volumen alto — sin lista
  fija de empresas grandes), el descargador de historial de Alpaca, las dos
  estrategias (cruce de medias móviles y RSI) y el backtester básico. Se
  corrieron 12 pruebas automáticas con datos inventados y todas pasaron —
  eso confirma que la lógica del código funciona como se espera, pero
  **todavía no se probó con datos reales de mercado** porque falta una
  pieza: las llaves de la cuenta de Alpaca.
- (2026-08-06) — Alex creó la cuenta de Alpaca y consiguió las llaves de
  paper trading. Se guardaron en el archivo `.env` de la carpeta del
  proyecto (no se suben a ningún lado). Se encontraron y corrigieron 2
  errores chicos al conectar de verdad: el script no encontraba sus propios
  archivos (import mal armado), y el plan gratis de Alpaca no deja pedir
  datos "recientes" del feed completo (SIP) — hay que pedirlos del feed de
  una sola bolsa (IEX), que sí es gratis en tiempo real.
- (2026-08-06) — **Primer backtest real (con datos de verdad), resultado
  preliminar:** con el filtro se encontraron 143 acciones candidatas
  (precio $1-$20, volumen alto) sobre 180 días de historial **diario**
  (una vela por día, no por minuto):
  - Cruce de medias móviles: 387 operaciones, 48.1% de acierto, ganancia
    promedio de apenas 0.01% por operación (ya con costos descontados) —
    básicamente empate, no alcanza a verse como una ventaja real.
  - RSI: 936 operaciones, 41.8% de acierto, **pierde** 0.49% en promedio
    por operación.
  - **Ojo con esto:** esta primera prueba usó velas de un día completo,
    no de minutos — es solo para confirmar que el código funciona de
    punta a punta con datos reales, todavía no es la prueba real de "día
    trading de minutos" que Alex quiere. Falta repetir esto con datos
    intradía (minuto a minuto) antes de sacar una conclusión sobre si hay
    oportunidad real.
  - Recordatorio: con solo 2 estrategias simples y sin separar datos de
    "entrenamiento" y de "prueba" (walk-forward), es muy pronto para
    concluir nada — eso es parte de fases posteriores del plan.
- (2026-08-06) — **Segundo backtest real, esta vez con velas de 1 minuto**
  (las mismas 143 candidatas, ~10 días hábiles, 417,612 velas descargadas
  — este sí es el que de verdad importa para "día trading de minutos"):
  - Cruce de medias móviles: 8,412 operaciones, 40.4% de acierto, **pierde**
    0.02% en promedio por operación.
  - RSI: 14,908 operaciones, 22.7% de acierto (bastante menos de la mitad),
    **pierde** 0.11% en promedio por operación.
  - Con tantas operaciones (miles), esto no es ruido — es una señal bastante
    clara de que, tal como están hoy, estas dos estrategias simples no
    tienen ventaja en velas de 1 minuto sobre este grupo de acciones. El
    RSI en particular acierta muy por debajo de la mitad, lo que sugiere
    que estas acciones tienden a seguir la tendencia en vez de "rebotar"
    cuando llegan a zona de sobrecompra/sobreventa — justo lo contrario de
    lo que asume la estrategia.
  - Sigue pendiente probar walk-forward (separar datos de prueba) y las
    demás estrategias del plan (Fase 2) antes de sacar una conclusión
    definitiva — con 2 estrategias sin ajustar es prematuro cerrar el
    tema, pero el patrón es parecido al de Pulso: hasta ahora, ningún
    "no arreglado" mejora las cosas por sí solo.

### (2026-08-06) Fase 2 completa: resultado final de la búsqueda de ventaja

Se construyeron las 8 estrategias que faltaban (ruptura con volumen, VWAP,
momentum, Opening Range Breakout, Gap and Go, pullback, reversión por
divergencia, rango/soporte-resistencia), el motor de confluencia, y las
herramientas para buscar de forma seria sin engañarse (walk-forward,
prueba de significancia estadística, corrección por pruebas múltiples).
Reporte técnico completo en `~/bot-trading-acciones/report_fase2.md`.

Se corrieron **3 pruebas rigurosas** sobre 40 acciones (las de mayor
volumen), 21 días de velas de 1 minuto, con costos reales de operar:

1. **Las 10 estrategias tal como están:** las 10 pierden dinero. En 8 de
   10, la pérdida es tan consistente que es estadísticamente segura (no es
   casualidad del muestreo) — con miles de operaciones de por medio en la
   mayoría.
2. **Diagnóstico + ajuste:** se descubrió que en varias estrategias
   (sobre todo RSI y reversión), incluso las operaciones que llegaban a la
   meta de ganancia terminaban en pérdida neta, porque el "premio" que se
   buscaba era más chico que el costo real de operar. Se probó exigir que
   el riesgo de cada operación fuera al menos 3 veces el costo — mejoró la
   tasa de acierto en varias, pero **la ganancia promedio siguió siendo
   negativa en 8 de 9 estrategias con datos suficientes**, y 6 de ellas
   siguen perdiendo de forma estadísticamente segura.
3. **Motor de confluencia** (exigir que 2 o 3 de las 10 coincidan a la
   vez): tampoco funcionó — combinar estrategias sin ventaja individual no
   crea una ventaja de la nada.

**Conclusión honesta: no se encontró ninguna estrategia con ventaja real,
confirmada con datos que el sistema no vio antes, sobre este grupo de
acciones en velas de 1 minuto.** No es "no se buscó lo suficiente" — se
probaron 10 estrategias conocidas, un ajuste basado en un diagnóstico
real, y 2 formas de combinarlas (sin contar las 2 pruebas previas de la
Fase 1); la mayoría pierde de forma estadísticamente segura, no por
casualidad.

**Decisión pendiente de Alex** (para cuando despierte, no se avanzó más
sin su aprobación, seguiendo la regla del proyecto de que cada fase
necesita su visto bueno): con este resultado, ¿qué sigue? Ideas honestas,
ninguna probada todavía:
1. Aceptar el resultado y pausar/cerrar esta fase (como con Pulso).
2. Probar con acciones más grandes/líquidas en vez de $1-$20 (el spread
   real pesa menos ahí).
3. Probar con velas más largas (5 o 15 minutos) en vez de 1 minuto.
4. Un enfoque de aprendizaje automático con validación estricta — más
   grande y con más riesgo de auto-engaño si no se hace con cuidado.
