# Temario: De Junior a Guru en OpenCode

Temario oficial del laboratorio `ExpertoOpenCode/`. Cada nivel tiene un objetivo claro, temas, una práctica con ruta de entrega y criterios de completado que el agente docente valida.

Estructura de prácticas:
`practicas/nivel-0X-<nombre>/practica-0Y-<tema>/`

---

## Nivel 1 — Fundamentos (Junior)

### Objetivo
Instalar, iniciar y usar por primera vez OpenCode en un proyecto real, y conocer los comandos esenciales de la TUI.

### Temas
1.1 Qué es OpenCode
- CLI que conecta LLM a tu terminal y código.
- Diferencia con ChatGPT/Claude en navegador: contexto real del proyecto + ejecución en tu máquina.
- Disponibilidad: TUI (terminal), desktop, IDE extension, web.

1.2 Instalación y configuración básica
- Comando de instalación: `curl -fsSL https://opencode.ai/install | bash`
- Alternativas: `npm install -g opencode-ai`, brew, paru.
- Ejecutar: `opencode` desde la raíz de tu proyecto.
- Conectar proveedores: comando `/connect` dentro de la TUI.
- Ver modelos disponibles: `opencode models`.

1.3 Primeros pasos
- Preguntar sobre tu código: "¿Qué hace este archivo?"
- Referenciar archivos con `@`: "Explica @src/main.ts"
- Navegar la TUI: leer respuestas, entender el formato.

1.4 Comandos built-in esenciales
- `/init` — crea o mejora `AGENTS.md` analizando tu proyecto.
- `/undo` y `/redo` — deshacer / rehacer cambios del agente.
- `/share` — compartir conversación vía link.
- `/models` — cambiar de modelo en vivo.
- `/help` — lista de comandos.
- `Tab` — alternar entre agentes primarios (Build ↔ Plan).

1.5 Modelos gratuitos de OpenCode
- OpenCode incluye modelos gratuitos del proveedor `opencode/` sin necesidad de API key.
- Ejemplos disponibles en esta instalación:
  - `opencode/mimo-v2.5-free`
  - `opencode/nemotron-3-ultra-free`
  - `opencode/nemotron-3.5-lightning-free`
  - `opencode/ling-3.0-flash-fin-free`
- Consulta la lista actualizada con `opencode models`.

### Práctica
**Ruta:** `practicas/nivel-01-fundamentos/practica-01-primer-uso/`

**Enunciado:** Ejecuta `opencode`, pregunta sobre un archivo de un proyecto real, prueba `/init` para generar o mejorar su `AGENTS.md`, y explora los comandos con `/help`.

**Entrega:**
- Un `README.md` en la carpeta de la práctica explicando con tus palabras: qué es OpenCode, en qué se diferencia de un chat en navegador, qué comando usaste, qué modelo gratuito elegiste y por qué.

### Criterios de completado
- Explica correctamente qué es OpenCode y su diferencia con un chat web.
- Usó `@` para referenciar un archivo en una pregunta.
- Ejecutó `/models` y sabe cuál modelo está usando.
- Generó o mejoró un `AGENTS.md` con `/init`.
- Se entregó el `README.md` solicitado.

---

## Nivel 2 — Práctico (Intermedio Bajo)

### Objetivo
Dominar los dos modos de operación (Plan y Build) y los patrones de prompts efectivos para resolver tareas reales con OpenCode.

### Temas
2.1 Modos de operación
- **Build mode** (default): puede leer, escribir y ejecutar comandos.
- **Plan mode**: solo analiza, no modifica nada (ideal para revisar antes de actuar).
- Cambiar con `Tab` en la TUI.

2.2 Patrones de prompts efectivos
- Dar contexto suficiente: "Agrega autenticación a /settings, mira cómo está hecha en @packages/functions/src/notes.ts".
- Incluir ejemplos y especificaciones claras.
- Usar lenguaje descriptivo, no ambiguo.

2.3 Crear features con Plan mode
1. `Tab` → Plan mode.
2. Describir la feature detalladamente.
3. Iterar el plan con feedback.
4. `Tab` → Build mode → "Haz los cambios".

2.4 Deshacer errores
- `/undo` múltiples veces para revertir.
- `/redo` para restaurar si te pasaste.
- El agente recupera tu mensaje original para reintentar.

### Práctica
**Ruta:** `practicas/nivel-02-practico/practica-01-plan-y-build/`

**Enunciado:** Elige una tarea pequeña (modificar un config, añadir una función simple, renombrar algo). Halla un plan con Plan mode, itéralo con feedback y luego ejecútalo en Build.

**Entrega:**
- `README.md` con el plan original, los ajustes tras tu feedback y lo que hizo Build.
- Evidencia del uso de `/undo` o `/redo` (descríbelo).

### Criterios de completado
- Usó Plan mode antes de tocar código.
- Iteró el plan al menos una vez con feedback.
- Manejó `/undo` o `/redo` correctamente.
- Explica la diferencia entre Build y Plan.

---

## Nivel 3 — Configuración (Intermedio)

### Objetivo
Entender `opencode.json`: ubicaciones con precedencia, modelos, permisos y variables de entorno.

### Temas
3.1 Archivo de configuración `opencode.json`
- Ubicaciones (en orden de precedencia):
  1. Remote (`.well-known/opencode`)
  2. Global (`~/.config/opencode/opencode.json`)
  3. Custom (`OPENCODE_CONFIG`)
  4. Project (`opencode.json` en raíz del repo)
- Se fusionan, no reemplazan.

3.2 Configurar modelos
```json
{
  "model": "opencode/mimo-v2.5-free",
  "small_model": "opencode/nemotron-3.5-lightning-free"
}
```
- `model` → modelo principal.
- `small_model` → tareas ligeras (títulos, resúmenes).
- Ver disponibles con `opencode models`.

3.3 Configurar permisos de herramientas
```json
{
  "permission": {
    "edit": "ask",
    "bash": "ask"
  }
}
```
- `allow` → ejecuta sin preguntar.
- `ask` → pide aprobación.
- `deny` → desactiva la herramienta.
- Reglas basadas en patrones (glob): las más específicas van al final.

3.4 Variables de entorno útiles
- `OPENCODE_CONFIG` — ruta a un config custom.
- `OPENCODE_CONFIG_CONTENT` — inyecta config inline en JSON.
- `OPENCODE_PURE=1` — omite plugins externos.
- `OPENCODE_SERVER_PASSWORD` — autenticación para `opencode serve` / `opencode web`.
- `OPENCODE_DISABLE_PROJECT_CONFIG=1` — ignora el config del proyecto (escape hatch).

### Práctica
**Ruta:** `practicas/nivel-03-configuracion/practica-01-opencode-json/`

**Enunciado:** Crea tu primer `opencode.json` en un proyecto de prueba con modelo gratuito y permisos personalizados (`ask` en `edit` y `bash`). Verifica que se carga con `opencode debug config`.

**Entrega:**
- `opencode.json` funcionando en un proyecto.
- `README.md` explicando cada campo y qué hace.

### Criterios de completado
- Config válida: `opencode debug config` la muestra sin errores.
- Explica la precedencia de ubicaciones de config.
- Define `permission` y explica los tres valores posibles.
- Distingue `model` de `small_model`.

---

## Nivel 4 — Agentes y Flujos (Intermedio Alto)

### Objetivo
Comprender agentes primarios vs subagentes, los agentes built-in de OpenCode, y crear agentes custom con permisos propios.

### Temas
4.1 Tipos de agentes
- **Primary**: interactúas directamente (Build, Plan) — se alternan con `Tab`.
- **Subagent**: invocados por agentes primarios o con `@mención` (General, Explore, Scout).

4.2 Agentes built-in
| Agente | Tipo | Para qué sirve |
| --- | --- | --- |
| build | primary | Desarrollo completo, todas las herramientas |
| plan | primary | Análisis sin modificar código |
| general | subagent | Tareas complejas multi-paso |
| explore | subagent | Explorar código (solo lectura, rápido) |
| scout | subagent | Investigar docs externas y dependencias (solo lectura) |

4.3 Crear agentes custom
- Por JSON en `opencode.json`.
- Por Markdown en `.opencode/agents/` (proyecto) o `~/.config/opencode/agents/` (global).
- CLI: `opencode agent create`.

4.4 Ejemplo de agente (security auditor)
```markdown
---
description: Auditoría de seguridad
mode: subagent
permission:
  edit: deny
---
Eres un experto en seguridad. Identifica vulnerabilidades...
```

4.5 Controlar qué subagentes puede invocar un agente
```json
{
  "permission": {
    "task": {
      "*": "deny",
      "code-reviewer": "allow"
    }
  }
}
```

### Práctica
**Ruta:** `practicas/nivel-04-agentes/practica-01-agente-code-reviewer/`

**Enunciado:** Crea un agente `code-reviewer` como subagente de solo lectura (`edit: deny`, `bash: deny`), invócalo con `@code-reviewer` para revisar un archivo del laboratorio.

**Entrega:**
- Archivo del agente (`.opencode/agents/code-reviewer.md` o equivalente).
- `README.md` con la revisión que devolvió el agente y tu reflexión sobre el flujo.

### Criterios de completado
- El agente existe y es invocable con `@`.
- Tiene permisos de solo lectura correctos.
- Explica la diferencia entre primary y subagent, y para qué sirve cada built-in.

---

## Nivel 5 — Comandos y Automatización (Avanzado Bajo)

### Objetivo
Crear comandos reutilizables (`/comando`), usar sus placeholders y operar OpenCode sin TUI (CLI no interactivo y servidor headless).

### Temas
5.1 Custom commands
- Markdown en `.opencode/commands/` (proyecto) o JSON en `opencode.json`.
- Se ejecutan con `/nombre-del-comando`.

5.2 Placeholders en commands
- `$ARGUMENTS` — todos los argumentos.
- `$1`, `$2`, `$3` — argumentos posicionales.
- `` !`git log --oneline -10` `` — inyectar salida de bash.
- `@src/file.ts` — incluir contenido de archivo.

5.3 Ejemplo de command
```markdown
---
description: Ejecutar tests
agent: build
---
Ejecuta la suite de tests con coverage.
Enfócate en tests fallidos y sugiere fixes.
```

5.4 CLI no interactivo
```
opencode run "Explica async/await en JavaScript"
opencode run -f archivo.ts "Analiza este archivo"
```

5.5 Servidor headless
```
opencode serve            # API server
opencode web              # Servidor con interfaz web
opencode attach <url>     # Conectar TUI a un servidor remoto
```

### Práctica
**Ruta:** `practicas/nivel-05-comandos/practica-01-comando-review/`

**Enunciado:** Crea un command `/review` que analice los últimos 10 commits del repo y lo use sobre el propio laboratorio. Prueba además `opencode run` con un prompt.

**Entrega:**
- El archivo del command.
- `README.md` con la salida del `/review` y del `opencode run`.

### Criterios de completado
- El command funciona con `/review`.
- Usa al menos un placeholder (`$ARGUMENTS` o bash injection).
- Ejecutó `opencode run` y entiende la diferencia con la TUI.

---

## Nivel 6 — Skills y MCP (Avanzado)

### Objetivo
Crear Agent Skills que se cargan bajo demanda y conectar herramientas externas vía MCP (local y remoto).

### Temas
6.1 Agent Skills
- Archivos `SKILL.md` en `.opencode/skills/<nombre>/`.
- Se cargan bajo demanda vía la herramienta `skill`.
- El agente ve las skills disponibles y decide cuándo usarlas.

6.2 Estructura de SKILL.md
```markdown
---
name: web-search
description: Buscar y navegar en la web
---
## Qué hago
- Buscar información actualizada en internet.
- Resumir documentación y verificar datos.
## Cuándo usarme
Usa esto cuando se pida investigar un tema o buscar en la web.
```
- `name` es obligatorio (minúsculas con guiones).
- `description` es fundamental: define cuándo se dispara la skill.

6.3 MCP Servers (Model Context Protocol)
- Herramientas externas que se integran a OpenCode.
- **Local**: se ejecutan como procesos (ej. `npx @playwright/mcp`).
- **Remote**: se conectan vía URL (ej. Sentry).

6.4 Ejemplo MCP (configuración en `opencode.json`)
```json
{
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {}
    }
  }
}
```
Luego autorizas con: `opencode mcp auth sentry`.

6.5 Gestión de MCP por CLI
```
opencode mcp add [name]     # añadir servidor interactivo
opencode mcp list           # listar y ver estado
opencode mcp auth [name]    # autenticación OAuth
opencode mcp debug <name>   # depurar conexión OAuth
```

### Práctica
**Ruta:** `practicas/nivel-06-skills-mcp/practica-01-skill-release/`

**Enunciado:** Crea una skill de búsqueda web (`web-search`) que se cargue bajo demanda para investigar temas o buscar en internet. Configura (o documenta cómo configurar) un MCP server local de navegador o remoto.

**Entrega:**
- `SKILL.md` en `.opencode/skills/`.
- `README.md` explicando cómo funcionan skills y MCP, y el MCP que elegiste.

### Criterios de completado
- La skill tiene `name` y `description` válidos y está en la ruta correcta.
- Explica cuándo se carga una skill (bajo demanda).
- Distingue MCP local de remoto.

---

## Nivel 7 — Integraciones (Avanzado Alto)

### Objetivo
Integrar OpenCode con GitHub/GitLab, compartir sesiones y habilitar LSP y formatters.

### Temas
7.1 Integración GitHub
- `opencode github install` — configura el agente en GitHub Actions.
- `opencode github run` — ejecuta el agente en CI.
- `opencode pr <n>` — descarga un PR y ejecuta OpenCode sobre él.

7.2 Integración GitLab
- Soporte nativo disponible en la documentación oficial.

7.3 Compartir sesiones
- `/share` genera link público.
- `share: "auto"` en config para auto-compartir.
- `opencode export` / `opencode import` para sesiones.

7.4 LSP integration
- Habilitar: `"lsp": true` en config.
- Herramientas `lsp` para definitions, references, hover, etc.
- Configurar servidores LSP específicos por lenguaje.

7.5 Formatters
```json
{
  "formatter": {
    "prettier": { "disabled": true },
    "custom": {
      "command": ["npx", "prettier", "--write", "$FILE"],
      "extensions": [".js", ".ts"]
    }
  }
}
```

### Práctica
**Ruta:** `practicas/nivel-07-integraciones/practica-01-integraciones/`

**Enunciado:** Prepara el laboratorio para integrarse: documenta cómo usar `opencode github install` en un repo real y habilita LSP y formatter en un proyecto de prueba con el lenguaje que prefieras.

**Entrega:**
- Config de `lsp` y `formatter` en un proyecto de prueba.
- `README.md` con el plan de integración GitHub/GitLab.

### Criterios de completado
- Explica para qué sirven `opencode github install` / `github run` / `pr <n>`.
- Configuró `lsp` y `formatter` correctamente y lo demuestra.
- Entiende `/share` y la diferencia con `export`/`import`.

---

## Nivel 8 — Guru (Experto)

### Objetivo
Combinar todo: políticas, optimización de contexto, plugins, SDK/server y un setup completo y reproducible.

### Temas
8.1 Arquitectura de configuración avanzada
- Managed settings (MDM para empresas).
- Remote config (`.well-known/opencode`).
- Precedencia completa: remote → global → custom → project → inline.

8.2 Policies (políticas de gobernanza)
```json
{
  "experimental": {
    "policies": [
      { "effect": "deny", "action": "provider.use", "resource": "openai" }
    ]
  }
}
```

8.3 Subagent depth
- `"subagent_depth": 0` → sin subagentes.
- `"subagent_depth": 2` → subagentes que invocan otros subagentes.

8.4 Variables en config
- `{env:VAR_NAME}` — variables de entorno.
- `{file:path/to/file}` — contenido de archivos (API keys en archivos separados).

8.5 Compaction y optimización
```json
{
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 10000
  }
}
```
- `auto: true` comprime contexto cuando la ventana se llena.
- `prune` elimina outputs viejos de herramientas.
- `reserved` deja buffer para evitar overflow.

8.6 Plugins
- `.opencode/plugins/` (auto-descubiertos) o npm packages.
- Hooks: `tool.execute.before`, `tool.execute.after`, `event`, `config`, etc.
- Crear herramientas completamente custom.

8.7 SDK y Server
- `opencode serve` — API HTTP programática.
- SDK para integrar OpenCode en tus propias herramientas.
- `opencode acp` — protocolo ACP para comunicación vía stdin/stdout.

8.8 Comandos CLI avanzados
```
opencode stats                # estadísticas de uso y costos
opencode stats --days 7       # últimos 7 días
opencode session list         # listar sesiones
opencode session delete <id>  # borrar una sesión
opencode debug config         # config resuelta (debugging)
opencode models --verbose     # modelos con metadata de costos
opencode debug skill          # listar skills disponibles
```

8.9 AGENTS.md como fuente de verdad
- `/init` genera o mejora `AGENTS.md`.
- Incluir: comandos exactos, arquitectura no obvia, convenciones del proyecto.
- Referenciar archivos externos vía `instructions` en `opencode.json`.

### Práctica
**Ruta:** `practicas/nivel-08-guru/practica-01-setup-completo/`

**Enunciado:** Arma un setup completo y reproducible: agentes custom útiles, al menos una skill, un MCP, permisos granulares, políticas y un workflow con `opencode run` + commands custom. Documenta el setup como "fuente de verdad".

**Entrega:**
- El setup completo en un proyecto.
- `README.md` (o `AGENTS.md` de ese proyecto) documentando cada pieza y cómo reproducirlo.

### Criterios de completado
- El setup junta agentes + skills + MCP + permisos + políticas.
- Un workflow automatizado funciona (`opencode run` con commands).
- El `README.md` serviría a otra persona (o a una sesión futura) para reproducirlo.
- Explica `subagent_depth`, compaction y `{env:}`/`{file:}`.

---

## Referencia rápida

| Comando | Qué hace |
| --- | --- |
| `opencode` | Lanza la TUI |
| `opencode run "prompt"` | Ejecuta sin TUI |
| `opencode serve` | Server headless |
| `opencode web` | Server con UI web |
| `opencode attach <url>` | Conectar TUI a server |
| `opencode models` | Listar modelos |
| `opencode agent create` | Crear un agente |
| `opencode agent list` | Listar agentes |
| `opencode mcp add` | Agregar un servidor MCP |
| `opencode mcp list` | Listar MCP servers |
| `opencode stats` | Estadísticas de uso y costos |
| `opencode upgrade` | Actualizar OpenCode |
| `opencode pr <n>` | Trabajar en un PR |
| `opencode export / import` | Exportar / importar sesiones |
| `opencode github install / run` | Integración GitHub |
| `opencode debug config` | Ver la configuración resuelta |
| `opencode session list` | Listar sesiones |