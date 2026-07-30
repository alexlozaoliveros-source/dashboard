# Orquestador de IAs (extensión propia para VSCode)

- **Tipo:** programación
- **Estado:** 🟢 Activo
- **Inicio:** 2026-07-30
- **Última actualización:** 2026-07-30

## Qué es

Una herramienta nueva y propia (no una app web, sino algo que corre dentro de
VSCode) para que varias IAs distintas colaboren o "debatan" en una misma
tarea de programación, en vez de tener que elegir manualmente cuál usar cada
vez — la idea que Alex vio funcionando en la computadora de un amigo.

## Decisiones importantes

- (2026-07-30) — **No es posible "fusionar" las 5 extensiones que Alex ya
  tiene instaladas en VSCode** (ej. Claude Code, Codex, Copilot). Las
  extensiones no tienen forma de hablarse entre sí ni de controlarse unas a
  otras — no existe esa función en VSCode.
- (2026-07-30) — Alex decidió construir una herramienta **nueva y separada**
  que se conecte directamente a las IAs (por API) y las haga colaborar,
  en lugar de usar una ya existente como Antigravity (que hace algo
  parecido, hecho por Google). Se le explicó el trade-off: cuesta dinero
  real (pago por uso de cada IA, aparte de las suscripciones que ya tiene)
  y toma tiempo de desarrollo, a cambio de quedar a su medida.

- (2026-07-30) — **Las 4 IAs a combinar:** Gemini, Kimi, ChatGPT/Codex y
  Claude Code. Deben usar las cuentas propias de Alex tal cual están hoy
  (la que tenga plan premium se queda en premium, la que sea gratis se
  queda en gratis) — no se van a crear cuentas ni planes nuevos para esto.
- (2026-07-30) — **Cómo funciona técnicamente:** se descubrió que las 4
  herramientas tienen una versión de "línea de comandos" (fuera de VSCode)
  que se puede llamar de forma automática y en paralelo, y que usa la
  sesión con la que uno ya inició sesión (por eso respeta el plan
  premium/gratis de cada una sin configurar nada aparte). Es una función
  oficial de cada empresa, documentada por ellas mismas — no es hackear ni
  saltarse reglas de uso.
- (2026-07-30) — **Mecanismo de decisión final:** se le manda la misma
  tarea a las 4 IAs a la vez. Luego, **Claude actúa de "juez"**: recibe las
  4 respuestas (incluyendo la propia) y entrega una respuesta final que
  toma en cuenta lo que dijeron las otras 3, no solo su propia opinión.
  Alex ve solamente ese resultado final (no las 4 respuestas por separado).

## Lecciones aprendidas

- _(ninguna todavía)_

## Avance de construcción

- (2026-07-30) — Se revisó la computadora de Alex: las 4 IAs están
  instaladas como extensiones de VSCode, pero **no** como programas de
  línea de comandos (que es lo que hace falta para automatizarlas). Falta
  instalar las 4 versiones de línea de comandos antes de poder construir
  el orquestador.
- (2026-07-30) — Se instalaron las 4 herramientas de línea de comandos en
  la computadora de Alex: Claude Code CLI, Codex CLI, Gemini CLI y Kimi
  Code CLI. **Falta que Alex inicie sesión** en cada una con su cuenta
  (paso pendiente, se hace una sola vez por herramienta). Próximo paso:
  guiar a Alex para iniciar sesión en las 4 y confirmar que las 4
  responden correctamente antes de empezar a programar el orquestador.

## Notas sueltas

- Siguiendo el mismo patrón que la Quiniela Liga MX: el código de este
  proyecto vivirá en una carpeta/repositorio aparte de este cuaderno de
  notas (este repo `dashboard` solo guarda el resumen y las decisiones).
