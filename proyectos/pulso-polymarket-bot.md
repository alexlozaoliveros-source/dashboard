# Pulso — bot de market-making en Polymarket (5 min crypto)

- **Tipo:** Programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-07-30
- **Última actualización:** 2026-07-30

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

- **Fase 0 (actual):** un programa recolector que guarda, sin parar, los
  datos de los mercados (libro de órdenes de Polymarket, precio de Binance,
  precio del oráculo que usa Polymarket para resolver). No apuesta nada
  todavía, solo observa y guarda.
- **Fase 1:** con esos datos, calcular si el precio del mercado se desvía de
  lo que "debería" valer, y si esa desviación es aprovechable después de
  costos reales.
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

## Notas sueltas

- El detalle técnico completo (cómo se conecta a cada fuente de datos, qué
  archivos genera, qué debe cumplir cada fase para aprobarse) vive en
  `~/pulso/CLAUDE.md`, dentro de la carpeta propia del proyecto.
- Cuando se cumplan las 72 horas de datos limpios de la Fase 0, hay que
  mostrarle a Alex el reporte (`report_f0.md`) para que decida si se pasa a
  la Fase 1.
