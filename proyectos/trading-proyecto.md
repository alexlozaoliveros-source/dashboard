# Trading Proyecto (página web del plan de estudios)

- **Tipo:** Personal (plan de estudios) + página web
- **Estado:** 🟢 Activo
- **Inicio:** 2026-11-17
- **Última actualización:** 2026-11-17

## Qué es

Una página web que presenta el "Plan de estudios para convertirse en
trader desde cero" (documento Word que Alex compartió, plan de 3 años:
año 1 inglés, año 2 bases de economía y finanzas, año 3 especialización
en mercados; después cursos de traders y primer dinero real).

Link público (Vercel): **https://trading-proyecto.vercel.app** — proyecto `trading-proyecto` en la cuenta de Vercel de Alex (`alexlozaoliveros-3295`). También existe una copia privada en Claude: https://claude.ai/artifact/FU7qGJYBYtAqzPjJAjdn62

La página vive en `proyectos/trading-proyecto/index.html` (un solo
archivo, se puede abrir con doble clic) y se publica en Vercel.

Orden de la página (lo pidió Alex así):
1. Título grande "TRADING PROYECTO".
2. Imagen grande tipo gráfica de bolsa (velas japonesas verdes/rojas,
   dibujada dentro de la misma página, no es una foto descargada).
3. La visión: la frase de "más del 90% de los day traders pierde" + 4
   puntos (inglés primero, bases antes que estrategias, el riesgo decide,
   la psicología pesa).
4. Los 7 principios del plan, tal cual vienen en el documento.
5. Tres cuadros en línea (Año 1, Año 2, Año 3); al tocar uno se abre
   debajo el detalle de ese año (meta, cursos con precios y links, viaje
   de estudio y la regla del año).

## Decisiones importantes

- (2026-11-17) — Diseño oscuro estilo "terminal de bolsa" (verde/rojo,
  cinta de cotizaciones arriba). Las cotizaciones de la cinta y la
  gráfica son decorativas, no son datos reales del mercado.
- (2026-11-17) — Se escribió "Trading" (no "Traiding") en el título.

## Lecciones aprendidas

- (2026-11-17) — Publicar en Vercel desde una sesión en la nube: Alex
  crea una llave en vercel.com/account/tokens (Scope: Full Account) y la
  pega en el formulario seguro de Claude. **El código correcto empieza con
  `vcp_`**; la primera vez se guardó algo que no era la llave y Vercel
  respondía "falta la llave" (incluso al intentar subir sin cuenta). Con la
  llave correcta funcionó al instante.

## Notas sueltas

- Las secciones del documento que no se pidieron en la página (etapa
  final, lista de verificación de cursos, presupuesto, fuentes) no se
  pusieron; se pueden agregar si Alex quiere.
