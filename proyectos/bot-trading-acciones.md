# Bot de trading de acciones (análisis y alertas)

- **Tipo:** Programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-08-05
- **Última actualización:** 2026-08-05

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

## Lecciones aprendidas

- (2026-08-05) — Cuando Alex pega un plan técnico que dice tener "reglas
  absolutas e innegociables", vale la pena revisar sus respuestas contra
  esas reglas antes de anotarlas — puede pedir algo en el chat que
  contradiga lo que él mismo escribió (como pasó aquí con "que el programa
  opere solo"), sin darse cuenta del cambio. Conviene señalarlo y confirmar
  antes de seguir. Ver también la lección de Pulso: no hay ventaja
  confirmada hasta que los datos reales lo demuestren.

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
- **Pendiente de Alex para poder seguir:** crear una cuenta gratis en
  Alpaca (alpaca.markets) y conseguir las llaves de la cuenta de "paper
  trading" (dinero falso, sin ningún riesgo) para poder descargar datos
  reales y correr el primer backtest de verdad.
