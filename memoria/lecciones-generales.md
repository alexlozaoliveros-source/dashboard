# Lecciones generales

Aprendizajes o errores que no pertenecen a un solo proyecto, sino que
aplican a varios. Se actualiza automáticamente durante las conversaciones.

- (2026-07-28) — Alex a veces copia comandos de tutoriales o videos sin
  entender qué hacen (ej. instalar un "skill" para Claude Code). Antes de
  ejecutar algo así, hay que revisar de dónde salió y qué hace realmente
  (sobre todo si baja algo de internet y pide "seguir sus instrucciones"
  a ciegas), y explicárselo en simple antes de correrlo. Cuando se verifica
  que es seguro, Alex está de acuerdo en seguir adelante.
- (2026-07-28) — Quedaron instalados a nivel de usuario (para todos los
  proyectos futuros, no solo la quiniela), en `~/.claude/skills/`:
  - `frontend-design` (de github.com/anthropics/skills) — guía de diseño
    visual para cuando se construya una interfaz.
  - `find-skills` (de github.com/vercel-labs/skills) — le sirve a Claude
    para buscar y sugerir otros "skills" (paquetes de instrucciones) que
    puedan ayudar en tareas futuras; Alex pidió que se avise cuando
    convenga usar uno.
- (2026-08-04) — <a id="conector-google-drive-sin-herramientas"></a>Alex
  conectó su cuenta de Google Drive en Configuración → Conectores de
  claude.ai (los pasos: perfil abajo a la izquierda → Configuración →
  Personalizar → Conectores → Google Drive → Conectar → iniciar sesión
  con Google), y quedó "conectado" de verdad (con botón "Desconectar"
  visible). Aun así, Claude no pudo usarlo: la propia pantalla de
  Conectores mostraba "Este conector no tiene herramientas disponibles",
  y no aparecía ninguna herramienta `mcp__claude_ai_*` para Drive/Sheets
  ni siquiera abriendo una conversación nueva. No se encontró la causa ni
  una forma de arreglarlo desde el chat. **Antes de prometerle a Alex una
  función que dependa de un conector (Google Drive, Sheets, etc.), hay
  que confirmar primero que existan herramientas `mcp__claude_ai_*`
  reales para ese conector**, no asumir que "conectado" implica
  "utilizable".
