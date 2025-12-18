# Proyecto AgenticAI - Configuración de Claude Code

## Descripción del Proyecto

Este proyecto explora e implementa el **Framework de Optimización de Contexto** para Claude Code.

El framework se enfoca en:
- Context Engineering mediante persistencia en Markdown
- Sub-agentes especializados como planificadores (NO ejecutores)
- Desarrollo paralelo con Git Worktrees
- Optimización de tokens a través de planificación estructurada

## Estructura del Proyecto

```
pydantic-ai/
├── .claude/                 # Configuración de Claude Code
│   ├── CLAUDE.md           # Este archivo - System prompt
│   ├── settings.json       # Configuración
│   ├── agents/             # Subagentes especializados
│   ├── commands/           # Comandos slash
│   ├── sessions/           # Sesiones de contexto
│   ├── doc/                # Planes de implementación
│   └── research/           # Resultados de investigación de mercado
├── docs/                   # Documentación del framework
│   ├── framework-claude-code.md
│   ├── diagrama-framework-explicado.md
│   ├── optimizacion-contexto-explicado.md
│   └── ...
├── templates/              # Templates reutilizables
```

## Stack Tecnológico

- **Lenguaje:** Python 3.11+
- **Gestor de Paquetes:** Poetry (NO pip)
- **Framework:** Pydantic AI
- **Modelos de IA:** Claude (vía API de Anthropic)
- **Control de Versiones:** Git con Worktrees

## Convenciones de Código

### Python
- Usar type hints de forma consistente
- Seguir la guía de estilo PEP 8
- Async/await para operaciones de I/O
- Modelos Pydantic para validación de datos

### Organización de Archivos
- Una responsabilidad por archivo
- Nombres claros y descriptivos
- Código modular y testeable

### Documentación
- Docstrings para APIs públicas
- Comentarios inline para lógica compleja
- Mantener este CLAUDE.md actualizado con cambios importantes

## REGLAS DE WORKFLOW

### Fase 0: Descubrimiento de Requisitos

**DEBE HACER:**
- Cuando la solicitud del usuario sea vaga, ambigua, o no tenga requisitos claros, DEBES usar el subagente `requirements-engineer` ANTES de pasar a planificación técnica
- El subagente usará técnicas estructuradas (5W1H, JTBD, User Stories) para transformar ideas en requisitos claros
- DEBES crear `.claude/sessions/discovery_{nombre_feature}.md` con problema statement, criterios de aceptación medibles, y scope definido
- DEBES validar el documento de descubrimiento con el usuario antes de continuar

**Proceso:**
1. Usuario proporciona solicitud inicial (puede ser vaga)
2. Evaluar si requisitos están claros:
   - ¿El problema real está identificado?
   - ¿Los criterios de éxito son medibles?
   - ¿El scope está definido (in/out)?
3. Si NO están claros → Usar comando `/discover` o delegar a `requirements-engineer`
4. Crear archivo de descubrimiento: `.claude/sessions/discovery_{nombre_feature}.md`
5. Validar con usuario que el documento refleja correctamente su necesidad
6. Una vez validado → Proceder a Fase 1

**Cuándo usar Fase 0:**
- ✅ Solicitud vaga: "necesito algo para usuarios"
- ✅ Solo solución sin problema: "quiero un dashboard"
- ✅ Criterios no claros: "debe ser rápido"
- ✅ Scope ambiguo: no está claro qué incluye/excluye

**Cuándo saltar Fase 0:**
- ❌ Requisitos ya están completamente claros
- ❌ Ya existen user stories con criterios medibles
- ❌ Feature muy simple y obvia

### Fase 1: Planificación

**DEBE HACER:**
- Al inicio de una feature en plan mode, DEBES SIEMPRE crear `.claude/sessions/context_session_{nombre_feature}.md` con el análisis inicial
- DEBES preguntar qué subagentes deberían estar involucrados en la implementación
- Intenta SIEMPRE ejecutar subagentes en paralelo si es posible
- Después de la fase de plan mode, SIEMPRE actualiza `.claude/sessions/context_session_{nombre_feature}.md` con la definición del plan y recomendaciones de subagentes

**Proceso:**
1. El usuario proporciona la solicitud de feature
2. Activar plan mode (si no está activo)
3. Crear archivo de sesión de contexto
4. Identificar subagentes requeridos
5. Delegar a subagentes EN PARALELO
6. Consolidar planes
7. Actualizar sesión de contexto con el plan completo

### Fase 2: Implementación

**DEBE HACER:**
- Antes de cualquier trabajo, DEBES ver los archivos en `.claude/sessions/context_session_{nombre_feature}.md` para obtener el contexto completo
- `.claude/sessions/context_session_{nombre_feature}.md` debe contener la mayoría del contexto (qué hiciste, plan general, los subagentes añaden contexto continuamente)
- Después de terminar el trabajo, DEBES actualizar `.claude/sessions/context_session_{nombre_feature}.md` para asegurar que otros puedan obtener el contexto completo
- Después de terminar cada fase, DEBES actualizar el archivo de sesión de contexto

**Proceso:**
1. Leer archivo de sesión de contexto
2. Leer todos los planes de `.claude/doc/{nombre_feature}/`
3. Implementar código siguiendo los planes consolidados
4. Actualizar sesión de contexto con checkboxes de progreso
5. Solo el agente principal implementa código

### Fase 3: Validación

**DEBE HACER:**
- Después de la implementación final, DEBES usar el subagente qa-criteria-validator para proporcionar un reporte de feedback
- Después de que qa-criteria-validator termine, DEBES revisar el reporte e implementar el feedback relacionado con la feature

**Proceso:**
1. Delegar a qa-criteria-validator
2. Revisar reporte de feedback
3. Implementar correcciones
4. Iterar hasta que los criterios de aceptación pasen
5. git commit cuando esté completo

## ESTRATEGIA DE MANEJO DE FALLOS

### Filosofía: Regla de los 3 Intentos

**Principio fundamental:** Intenta resolver problemas automáticamente hasta 3 veces. Si fallas 3 veces consecutivas, DEBES escalar al usuario.

**NUNCA hagas reintentos infinitos. SIEMPRE hay un límite de 3 intentos.**

### Aplicación por Fase

#### Fallos en Fase 0-1: Subagentes que No Pueden Completar

**Cuando un subagente falla al crear su plan:**

```
Intento 1 - Retry Directo (Error Transitorio)
├─ Verificar que context_session_{feature}.md existe
├─ Verificar que el subagente tiene acceso a archivos necesarios
├─ Re-ejecutar el mismo subagente
└─ Si falla → Intento 2

Intento 2 - Retry con Contexto Adicional
├─ Pasar contexto más detallado al subagente
├─ Incluir ejemplos de planes exitosos previos (si existen)
├─ Especificar explícitamente qué archivos leer
└─ Si falla → Intento 3

Intento 3 - Investigación Manual por Agente Principal
├─ TÚ (agente principal) lees los archivos que el subagente debía leer
├─ TÚ intentas crear un plan básico manualmente
├─ Documentas el problema en context_session_{feature}.md
└─ Si falla → ESCALAR AL USUARIO

Escalación al Usuario:
"❌ No pude completar la planificación de {componente} después de 3 intentos.

**Intentos realizados:**
1. {descripción breve del error en intento 1}
2. {descripción breve del error en intento 2}
3. {descripción breve del error en intento 3}

**Posible causa raíz:**
{tu mejor hipótesis del problema}

**Necesito tu ayuda con:**
- {pregunta específica 1 para resolver el blocker}
- {pregunta específica 2 para entender mejor el contexto}

**Archivos de diagnóstico:**
- `.claude/sessions/context_session_{feature}.md` (contexto completo)
- `.claude/sessions/error_log_{feature}.md` (log detallado de errores)"
```

**Documentar cada intento:**

En `.claude/sessions/context_session_{feature}.md`, añadir:

```markdown
### Error Log

**[2025-12-11 14:30:00] Attempt 1/3 - backend-developer**
- Status: FAILED
- Error: File not found: context_session_{feature}.md
- Action: Verificando existencia de archivos

**[2025-12-11 14:32:00] Attempt 2/3 - backend-developer**
- Status: FAILED
- Error: Timeout after 120s
- Action: Aumentando contexto y ejemplos

**[2025-12-11 14:35:00] Attempt 3/3 - Manual investigation**
- Status: FAILED
- Error: Requisitos incompletos en discovery document
- Action: Escalando a usuario
```

#### Fallos en Fase 2: Implementación con Tests/Linters Fallando

**Cuando tests o linters fallan repetidamente:**

```
Intento 1 - Auto-Fix Común
├─ Ejecutar auto-fixers disponibles (ruff --fix, etc.)
├─ Corregir imports faltantes obvios
├─ Formatear código
├─ Re-ejecutar tests/linters
└─ Si falla → Intento 2

Intento 2 - Análisis de Root Cause
├─ Leer stack traces completos
├─ Identificar causa raíz del error
├─ Aplicar fix específico al problema real
├─ Re-ejecutar tests/linters
└─ Si falla → Intento 3

Intento 3 - Revisar Contra Plan Original
├─ Re-leer todos los planes de .claude/doc/{feature}/
├─ Identificar gaps entre plan e implementación
├─ Ajustar implementación para seguir plan exactamente
├─ Re-ejecutar tests/linters
└─ Si falla → ESCALAR AL USUARIO

Escalación al Usuario:
"❌ Los tests/linters siguen fallando después de 3 intentos de corrección.

**Error persistente:**
```
{error message completo del test/linter}
```

**Archivos afectados:**
- {file1}:{line} - {descripción del problema}
- {file2}:{line} - {descripción del problema}

**Intentos de corrección:**
1. Auto-fix: {qué se intentó}
2. Root cause fix: {qué se identificó y corrigió}
3. Plan alignment: {qué se ajustó}

**Mi análisis:**
{tu hipótesis de por qué sigue fallando}

**Necesito tu input en:**
- ¿El enfoque arquitectónico es correcto?
- ¿Hay algún requisito que no entendí?
- ¿Prefieres una solución alternativa?"
```

#### Fallos en Fase 3: QA Validation Fallando

**Cuando qa-criteria-validator encuentra issues que no se pueden resolver:**

```
Intento 1 - Implementar Fixes del QA Report
├─ Leer qa_report.md completamente
├─ Identificar issues de severidad CRITICAL y HIGH
├─ Implementar todos los fixes sugeridos
├─ Re-ejecutar qa-criteria-validator
└─ Si aún hay issues críticos → Intento 2

Intento 2 - Revisar Criterios de Aceptación Originales
├─ Leer discovery_{feature}.md
├─ Comparar implementación actual vs criterios originales
├─ Identificar funcionalidad faltante
├─ Implementar gaps identificados
├─ Re-ejecutar qa-criteria-validator
└─ Si aún hay issues críticos → Intento 3

Intento 3 - Análisis Profundo
├─ Investigar por qué los criterios no se cumplen
├─ Revisar si el problema es de diseño vs implementación
├─ Documentar limitaciones encontradas
└─ ESCALAR AL USUARIO con análisis completo

Escalación al Usuario:
"❌ La implementación no pasa validación QA después de 3 intentos.

**Criterios que aún fallan:**
- [ ] {criterio 1}: {razón detallada por qué falla}
- [ ] {criterio 2}: {razón detallada por qué falla}

**Hipótesis de causa raíz:**
1. {hipótesis 1 con evidencia}
2. {hipótesis 2 con evidencia}

**Opciones que veo:**
A. {opción 1: qué implicaría, pros/cons}
B. {opción 2: qué implicaría, pros/cons}

**¿Cuál prefieres, o tienes otra solución en mente?**"
```

### Tracking de Intentos

**Crear archivo de tracking:** `.claude/sessions/error_log_{feature}.md`

```markdown
# Error Log: {Feature Name}

## Summary
- **Feature:** {nombre}
- **Current Phase:** {fase actual}
- **Total Failed Attempts:** {número}
- **Escalated to User:** {yes/no}
- **Status:** {in_progress / blocked / resolved}

---

## Attempt Log

### [2025-12-11 14:30:00] Attempt 1/3
**Component:** backend-developer (subagent)
**Action:** Generate backend plan
**Result:** FAILED
**Error:** `FileNotFoundError: context_session_user-auth.md`
**Root Cause:** Context session file not created yet
**Fix Applied:** Created context_session file with initial context
**Next Step:** Retry with context file in place

### [2025-12-11 14:32:00] Attempt 2/3
**Component:** backend-developer (subagent)
**Action:** Generate backend plan (retry)
**Result:** FAILED
**Error:** Timeout after 120 seconds
**Root Cause:** Complex requirements, insufficient context
**Fix Applied:** Added detailed examples and explicit file paths
**Next Step:** Retry with enhanced context

### [2025-12-11 14:35:00] Attempt 3/3
**Component:** Main agent manual investigation
**Action:** Manual plan creation attempt
**Result:** FAILED
**Error:** Incomplete requirements in discovery document
**Root Cause:** Discovery phase didn't capture database schema requirements
**Fix Applied:** None (requires user input)
**Next Step:** ESCALATE - need user to clarify database requirements

---

## Escalation Record

**Escalated At:** 2025-12-11 14:36:00
**Escalated To:** User
**Reason:** Cannot proceed without database schema clarification
**User Response:** [pending]
**Resolution:** [pending]
```

### Reglas Universales de Error Handling

1. **NUNCA excedas 3 intentos** para la misma operación
2. **SIEMPRE documenta cada intento** en context_session o error_log
3. **SIEMPRE escala con diagnóstico claro** (no solo "falló")
4. **SIEMPRE ofrece opciones** cuando escalas (no solo reportes el problema)
5. **SIEMPRE actualiza el tracking** después de cada intento

### Prevención de Loops Infinitos

**Anti-pattern a evitar:**

```
❌ MAL - Loop infinito:
while tests_failing:
    fix_errors()
    run_tests()
# Puede correr por siempre

✅ BIEN - Loop limitado:
max_attempts = 3
for attempt in range(1, max_attempts + 1):
    fix_errors()
    if run_tests():
        break
    if attempt == max_attempts:
        escalate_to_user()
```

**Cuando detectes que estás iterando:**

1. Cuenta tus intentos
2. Si llegas a 3, PARA
3. Analiza qué está pasando
4. Escala con análisis detallado

### Timeout por Fase

**Tiempos máximos sugeridos antes de escalar:**

| Fase | Operación | Timeout |
|------|-----------|---------|
| 0 | requirements-engineer | 10 min |
| 1 | Subagente planificador | 5 min |
| 1 | Consolidación de planes | 2 min |
| 2 | Implementación (por archivo) | 3 min |
| 2 | Tests/linters (por run) | 2 min |
| 3 | QA validation | 5 min |

**Si una operación excede su timeout 2 veces consecutivas, escala al usuario.**

## Subagentes

Tienes acceso a subagentes especializados:

### Descubrimiento y Requisitos (Fase 0)
- **requirements-engineer:** Descubrir, refinar y documentar requisitos usando 5W1H, JTBD, y User Stories. Solo descubre y documenta, NO implementa.

### Desarrollo Core (Fase 1-2)
- **pydantic-ai-architect:** Todas las tareas relacionadas con agentes de IA usando el framework Pydantic AI
- **backend-developer:** Planificación de lógica de negocio del backend
- **backend-test-engineer:** Planificación de testing del backend después de la implementación
- **frontend-developer:** Planificación de lógica de negocio del lado del cliente antes de crear UI
- **frontend-test-engineer:** Planificación de testing del frontend después de la implementación

### Diseño UI/UX (Fase 1-2)
- **shadcn-ui-architect:** Planificación de construcción y ajuste de UI con componentes shadcn/ui
- **ui-ux-analyzer:** Análisis de UI/UX existente, identificación de mejoras y problemas de usabilidad

### Validación y QA (Fase 3)
- **qa-criteria-validator:** Validación de features completas contra criterios de aceptación con reportes de QA

### Investigación y Estrategia (Pre-Fase 0)
- **product-strategist-agent:** Estrategia de producto e ideación de mercado
- **research-analyst-agent:** Investigación de mercado y análisis competitivo

## Reglas Importantes

### Sobre los Subagentes
- **Los subagentes hacen investigación y reportan feedback, pero TÚ harás la implementación real**
- Cuando trabajes con subagentes, asegúrate de pasar el archivo de contexto: `.claude/sessions/context_session_{nombre_feature}.md`
- Después de que un subagente termine su trabajo, lee la documentación relacionada que crearon antes de ejecutar
- Los subagentes NUNCA implementan código directamente, solo planifican
- Los subagentes NUNCA corren build o servidores de desarrollo

### Sobre la Implementación
- Estamos usando **poetry** NO pip
- Siempre lee la sesión de contexto antes de empezar a trabajar
- Siempre actualiza la sesión de contexto después de completar el trabajo
- Toma en cuenta la implementación actual del proyecto

### Sobre Git Worktrees
- Usa la estructura de directorio `.trees/feature-{nombre}`
- Cada feature en su propio worktree
- Limpia los worktrees después de mergear
- Ver `docs/git-worktrees-guide.md` para detalles

### Sobre Optimización de Contexto
- Persistir planes en archivos Markdown
- Minimizar contexto usando planes en lugar de código completo
- Usar subagentes paralelos sin superposición
- Generar documentación automática
- Habilitar puntos de resume claros

## Workflows Comunes

### Workflow Completo: De Idea a Implementación

```bash
# 1. [OPCIONAL] Investigación de Mercado (si es nueva idea de producto)
/ideation "descripción de la idea o feature"
# Salida en: .claude/research/{nombre}.md

# 2. [RECOMENDADO] Descubrimiento de Requisitos (si requisitos no están claros)
/discover "solicitud inicial del usuario"
# Salida en: .claude/sessions/discovery_{feature}.md
# El requirements-engineer hará preguntas estructuradas

# 3. Validar documento de descubrimiento con usuario
# Revisar: problema statement, criterios de aceptación, scope

# 4. Iniciar Feature con Worktree (una vez requisitos están claros)
/worktree nombre-feature
# Esto activa automáticamente Plan Mode (Fase 1)

# 5. Implementación
/work
# El agente principal lee los planes e implementa
```

### Descubrimiento de Requisitos (Fase 0)
```bash
# Cuando la solicitud es vaga o ambigua
/discover "necesito algo para manejar usuarios"

# El requirements-engineer preguntará:
# - ¿Qué problema estás resolviendo?
# - ¿Quién usará esto?
# - ¿Cuándo se usará?
# - ¿Qué resultado esperas?
# etc.

# Resultado:
# .claude/sessions/discovery_{feature}.md con requisitos claros
```

### Iniciar una Nueva Feature (Fase 1)
```bash
# Opción 1: Con comando slash (RECOMENDADO)
/worktree nombre-feature

# Opción 2: Manual
git worktree add ./.trees/nombre-feature -b nombre-feature
cd .trees/nombre-feature
claude --permission-mode plan
```

### Investigación e Ideación (Pre-Fase 0)
```bash
/ideation "descripción de la idea o feature"
# Salida en: .claude/research/{nombre}.md
```

### Implementación
```bash
# Después de que la planificación esté completa
/work
# El agente principal lee los planes e implementa
```

### Ejecutar Tests
```bash
poetry run pytest
poetry run pytest tests/ -v
```

### Linting y Formateo
```bash
poetry run ruff check .
poetry run ruff format .
poetry run mypy .
```

## MCPs (Model Context Protocol)

Servidores MCP disponibles:
- **context7:** Documentación actualizada de librerías
- **memory:** Grafo de conocimiento para memoria del proyecto
- **n8n-mcp:** Herramientas de integración de workflows n8n
- **playwright-server:** Automatización de navegador

## Referencias

Para información detallada sobre este framework:
- `docs/framework-claude-code.md` - Documentación completa del framework
- `docs/diagrama-framework-explicado.md` - Análisis del diagrama de workflow
- `docs/optimizacion-contexto-explicado.md` - Los 5 beneficios clave explicados
- `docs/referencias.md` - Todas las fuentes y referencias

---

**Última Actualización:** 2025-12-14
**Versión del Framework:** 1.3

**Cambios v1.3:**
- 🎨 **Mejora de Calidad de Subagentes:** Ampliación de metodología y estructura en agentes UI/UX
  - `shadcn-ui-architect`: Metodología de planificación UI detallada, verificaciones de calidad, protocolo de error handling
  - `ui-ux-analyzer`: Metodología de análisis UX completa, análisis heurístico, protocolo de error handling
- 📊 **Mejora de Agentes de Investigación:** Estandarización de formato y protocolos
  - `product-strategist-agent`: Flujo de trabajo estructurado, formato de salida estandarizado, protocolo de error handling
  - `research-analyst-agent`: Metodología de investigación comparativa, matriz de decisión, protocolo de error handling
- 🏗️ **Recategorización de Subagentes:** Separación clara de responsabilidades por fase
  - Nueva categoría "Diseño UI/UX (Fase 1-2)" separada de "Validación y QA (Fase 3)"
  - `qa-criteria-validator` ahora correctamente categorizado en "Validación y QA (Fase 3)"
- ✅ **Estandarización de Formato de Salida:** Todos los subagentes ahora tienen formato de mensaje final consistente

**Cambios v1.2:**
- 🛡️ **CRÍTICO:** Añadida Estrategia de Manejo de Fallos con regla de 3 intentos
- 🛡️ Protección contra loops infinitos en todas las fases
- 🛡️ Escalación formal al usuario con diagnóstico detallado
- 🛡️ Tracking de intentos en error_log_{feature}.md
- 🛡️ Timeouts sugeridos por fase y operación
- 📊 Templates de error logging y escalación

**Cambios v1.1:**
- ✨ Añadida Fase 0: Descubrimiento de Requisitos
- ✨ Nuevo subagente: requirements-engineer
- ✨ Nuevo comando: /discover
- ✨ Template de discovery session
- 📝 Workflow completo actualizado con fase de descubrimiento
