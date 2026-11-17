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
- (2026-11-17) — **El año 1 pasó a ser de tiempo completo (~43 h por
  semana)**, como Alex lo rehízo en su documento del plan: dos profesores
  particulares (A: 8:00–9:30, gramática/escritura/examen; B: 10:00–11:30,
  conversación e inglés de negocios, de preferencia nativo), horario
  diario de 8:00 a 18:00, sábado de simulacro cada dos semanas, domingo
  libre. 28 h de inglés + 12.5 h de materia del trimestre + 2.5 h de
  repaso. Trimestres: meses 1–3 bolsa, 4–6 IA y Excel, 7–8 Nueva York,
  9–12 B2 y examen en el mes 11. Profesores: $13,000–52,000 MXN al mes.
  La página se actualizó con todo esto y el principio "Medio tiempo
  constante" se cambió a "Tiempo completo en el año 1". Pendiente: armar
  el año 2 con el mismo nivel de detalle cuando Alex lo pida.

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
