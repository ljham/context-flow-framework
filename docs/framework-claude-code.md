# Framework de Claude Code: Optimización del Contexto con Subagentes y Worktrees

## Introducción

Este framework representa una metodología avanzada para trabajar con Claude Code en proyectos reales de producción. Fue presentado en el video ["Deep Dive en Cloud Code"](https://youtu.be/NJ6sO_0BoTA) y se centra en la **optimización del contexto** mediante el uso estratégico de subagentes especializados, persistencia en archivos Markdown y desarrollo paralelo con Git Worktrees.

### ¿Por qué este Framework?

Claude Code es mucho más que un asistente de código: es un **compañero de pair programming** que se integra en tu flujo de desarrollo. Sin embargo, sin una metodología adecuada, es fácil caer en:

- ❌ Pérdida de contexto al cambiar entre tareas
- ❌ Consumo excesivo de tokens
- ❌ Código generado ineficiente e inmantenible
- ❌ Dificultad para trabajar en múltiples features simultáneamente

Este framework resuelve estos problemas mediante **Context Engineering** y una arquitectura bien definida de subagentes planificadores.

## Los 5 Beneficios Clave del Framework

Según el diagrama de optimización del contexto presentado en el video, el framework proporciona cinco beneficios fundamentales:

### 1. 🔄 Minimizar el Contexto

**Problema:** Las ventanas de contexto saturadas reducen la calidad de las respuestas y aumentan costos.

**Solución:**
- Usar archivos Markdown en lugar de código en el contexto
- Mantener conversaciones enfocadas en tareas específicas
- Delegar a subagentes especializados en lugar de manejar todo en una conversación

**Ejemplo:**
```
❌ MAL: Cargar 50 archivos de código en el contexto
✅ BIEN: Subagentes crean planes en .md, agente principal lee solo los planes
```

### 2. 👥 Evitar Superposición de Agentes

**Problema:** Múltiples agentes modificando los mismos archivos causa conflictos y errores.

**Solución:**
- Cada worktree contiene una feature aislada
- Los subagentes solo planifican, NO implementan
- El agente principal es el único que escribe código

**Flujo:**
```
Subagente 1 (Backend) → Plan en .claude/doc/feature-x/backend_plan.md
Subagente 2 (Frontend) → Plan en .claude/doc/feature-x/frontend_plan.md
Agente Principal → Lee ambos planes → Implementa secuencialmente
```

### 3. 📊 Documentación Automática

**Problema:** La documentación se desactualiza o no se crea.

**Solución:**
- Cada feature genera automáticamente:
  - Archivos de sesión (`.claude/sessions/context_session_{feature}_{agent}.md`)
  - Planes de implementación (`.claude/doc/{feature}/{agent}_plan.md`)
  - Research de mercado (`.claude/research/`)

**Beneficio:** Historial completo de decisiones arquitectónicas y cambios.

### 4. 📁 Persistir Planes / Resume Claro

**Problema:** Interrumpir el trabajo implica perder contexto.

**Solución:**
- Todos los planes se guardan en archivos
- Comando `/resume` para continuar desde cualquier punto
- Pausar y retomar sin pérdida de información

**Ejemplo de archivo de sesión:**
```markdown
# Context Session: Feature Dashboard Kanban

## Objective
Create a kanban dashboard for news management

## Status
- [x] Backend developer - API endpoints created
- [x] Frontend developer - UI components ready
- [ ] QA validator - Testing in progress

## Next Steps
- Complete QA validation
- Fix any reported issues
```

### 5. 🔀 Usar Worktrees En Paralelo

**Problema:** Cambiar de rama interrumpe el flujo y causa context switching.

**Solución:**
- Cada feature en su propio worktree (`.trees/feature-{nombre}`)
- Múltiples instancias de Claude Code trabajando simultáneamente
- Desarrollo paralelo sin interferencias

**Ejemplo:**
```bash
# Terminal 1: Feature A
cd .trees/feature-user-auth
claude  # Trabajando en autenticación

# Terminal 2: Feature B
cd .trees/feature-dashboard
claude  # Trabajando en dashboard

# Terminal 3: Research
pwd  # Directorio principal
claude  # Haciendo research de mercado
```

## Diagrama Visual del Framework

El framework se estructura en **6 fases** que se visualizan como un camino de desarrollo:

### Vista de Camino: Las 6 Fases

```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   FASE 0    │      │   FASE 1    │      │   FASE 2    │      │   FASE 3    │      │   FASE 4    │      │   FASE 5    │
│Descubrir    │  →   │ Configurar  │  →   │  Delegar    │  →   │  Ejecutar   │  →   │ Implementar │  →   │  Corregir   │
│ Requisitos  │      │ Sub-Agentes │      │   Tareas    │      │Sub-Agentes  │      │Características     │   Errores   │
└─────────────┘      └─────────────┘      └─────────────┘      └─────────────┘      └─────────────┘      └─────────────┘
```

#### Fase 0: Descubrimiento de Requisitos (NUEVO en v1.1)
- El agente `requirements-engineer` ayuda al usuario a clarificar requisitos
- Usa técnicas estructuradas: 5W1H, Jobs-to-be-Done, User Stories
- Transforma solicitudes vagas en requisitos claros y medibles
- Genera documento de descubrimiento con criterios de aceptación
- Valida con el usuario antes de proceder a planificación técnica
- **IMPORTANTE:** Esta fase solo descubre y documenta, NO planifica soluciones técnicas

#### Fase 1: Configuración de Sub-Agentes
- El agente principal identifica qué subagentes necesita
- Prepara el contexto inicial
- Define la estrategia de delegación

#### Fase 2: Delegación de Tareas
- Delega en PARALELO a subagentes especializados
- Pasa archivos de contexto (context_session)
- Cada subagente recibe una tarea específica

#### Fase 3: Ejecución de Sub-Agentes
- Subagentes realizan research y análisis
- Cada uno crea su propio plan de implementación
- Generan documentación detallada
- **IMPORTANTE:** Los subagentes NO implementan código

#### Fase 4: Implementación de Características
- El agente principal lee TODOS los planes
- Implementa el código siguiendo las especificaciones
- Ejecuta `git add` para preparar cambios
- Corre linters y validaciones

#### Fase 5: Corrección de Errores
- Corrige errores reportados por linters/tests
- Itera hasta que todo pase
- Genera documentación final
- Ejecuta `git commit`

### Vista Técnica: Flujo de Datos

```
                              ┌─────────────────┐
                              │  User Request   │
                              │   (Feature)     │
                              └────────┬────────┘
                                       │
                              ┌────────▼────────┐
                              │ Agente Principal│
                              └────────┬────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                  │
            ┌───────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐
            │ Subagent 1   │   │ Subagent 2  │   │ Subagent 3  │
            │  (Backend)   │   │ (Frontend)  │   │    (QA)     │
            └───────┬──────┘   └──────┬──────┘   └──────┬──────┘
                    │                  │                  │
                    │ Escribe Plan     │ Escribe Plan     │ Escribe Plan
                    │                  │                  │
            ┌───────▼──────────────────▼──────────────────▼───────┐
            │        .claude/doc/{feature}/                        │
            │  - backend_plan.md                                   │
            │  - frontend_plan.md                                  │
            │  - qa_plan.md                                        │
            └───────┬──────────────────────────────────────────────┘
                    │
            ┌───────▼──────┐
            │ Agente       │
            │ Principal    │
            │ Lee Planes   │
            └───────┬──────┘
                    │
            ┌───────▼──────┐
            │ Implementa   │
            │   Código     │
            └───────┬──────┘
                    │
            ┌───────▼──────┐
            │  git add     │
            │  Validación  │
            │  git commit  │
            └──────────────┘
```

## Workflow Completo: De la Idea al Commit

### Paso 0: Descubrimiento de Requisitos (Si es necesario)

```bash
# Si la solicitud del usuario es vaga o ambigua
/discover "necesito algo para manejar usuarios"

# El requirements-engineer hará preguntas estructuradas:
# - ¿Qué problema estás resolviendo?
# - ¿Quiénes son los usuarios?
# - ¿Cuándo/dónde se usará?
# - ¿Qué resultado esperas?
# - ¿Cómo sabrás que está completo?

# Resultado:
# .claude/sessions/discovery_{feature}.md con:
# - Problem statement claro
# - User stories con criterios de aceptación
# - Scope definido (in/out)
# - Success metrics
```

**Ejemplo de transformación:**

```
Input vago: "Necesito un dashboard"

Después de /discover:
- Problema: "Los managers pierden 30min diarios consolidando reportes de 3 sistemas"
- Usuario: Manager de operaciones, acceso diario
- Resultado: Dashboard que muestre métricas clave de los 3 sistemas en tiempo real
- Criterios:
  - [ ] Carga < 2s
  - [ ] Auto-refresh cada 5min
  - [ ] Exportable a PDF
  - [ ] Filtrable por fecha
```

### Paso 1: Iniciar Feature con Worktree

```bash
# Usando el comando slash /worktree
/worktree user-authentication

# Esto ejecuta automáticamente:
# 1. git worktree add ./.trees/feature-user-authentication -b feature-user-authentication
# 2. cd .trees/feature-user-authentication
# 3. Activar plan mode
# 4. Crear archivo: .claude/sessions/context_session_user-authentication.md
```

### Paso 2: Fase de Planificación

El agente principal activa **plan mode** y delega a subagentes:

```
User: "Necesito implementar autenticación de usuarios con JWT"

Claude (Agente Principal):
1. Identifica subagentes necesarios:
   - backend-developer
   - frontend-developer
   - pydantic-ai-architect (si usamos Pydantic)

2. Delega en PARALELO:
   "Usa backend-developer para diseñar la API de autenticación"
   "Usa frontend-developer para diseñar el flujo de login"
```

Cada subagente crea su plan:
- `.claude/doc/user-authentication/backend_plan.md`
- `.claude/doc/user-authentication/frontend_plan.md`
- `.claude/doc/user-authentication/pydantic_agents_plan.md`

### Paso 3: Fase de Implementación

```
User: /work

Claude (Agente Principal):
1. Lee .claude/sessions/context_session_user-authentication.md
2. Lee todos los planes en .claude/doc/user-authentication/
3. Implementa código siguiendo los planes
4. Actualiza context_session con progreso
```

### Paso 4: Fase de Validación

```
Claude:
- Ejecuta git add
- Corre linters
- Si hay errores, los corrige
- Delega a qa-criteria-validator si es UI
- Ejecuta git commit cuando todo pasa
```

## Estructura de Archivos del Framework

```
proyecto/
├── .claude/
│   ├── CLAUDE.md                    # System Prompt del agente principal
│   ├── settings.json                # Configuración global
│   ├── mcp.json                     # Conexiones MCP
│   │
│   ├── agents/                      # Subagentes especializados
│   │   ├── pydantic-ai-architect.md
│   │   ├── backend-developer.md
│   │   ├── frontend-developer.md
│   │   ├── qa-criteria-validator.md
│   │   └── ...
│   │
│   ├── commands/                    # Slash commands
│   │   ├── ideation.md
│   │   ├── worktree.md
│   │   └── work.md
│   │
│   ├── sessions/                    # Contextos de sesión
│   │   └── context_session_{feature}_{agent}.md
│   │
│   ├── doc/                         # Planes de implementación
│   │   └── {feature_name}/
│   │       ├── backend_plan.md
│   │       ├── frontend_plan.md
│   │       └── ...
│   │
│   └── research/                    # Research de mercado
│       └── {research_name}.md
│
├── .trees/                          # Worktrees de Git
│   ├── feature-authentication/
│   ├── feature-dashboard/
│   └── ...
│
└── docs/                            # Documentación del proyecto
```

## Context Engineering: Las 4 Estrategias

El framework aplica las 4 estrategias fundamentales de Context Engineering:

### 1. Write (Escribir)

**Guardar contexto fuera de la ventana de contexto**

- Archivos de sesión: `.claude/sessions/`
- Planes: `.claude/doc/`
- Research: `.claude/research/`

**Beneficio:** Reduce el contexto activo manteniendo la información accesible.

### 2. Select (Seleccionar)

**Recuperar solo la información relevante**

- Antes de implementar, leer `context_session_{feature}.md`
- Cargar solo los planes necesarios para la tarea actual
- No cargar todo el código, solo los archivos relevantes

**Beneficio:** Mantiene el contexto enfocado y relevante.

### 3. Compress (Comprimir)

**Usar resúmenes en lugar de datos completos**

- Planes de implementación en lugar de código completo
- Context sessions en lugar de historial completo de conversación
- Documentación concisa en lugar de explicaciones extensas

**Beneficio:** Maximiza la señal, minimiza el ruido.

### 4. Isolate (Aislar)

**Separar contextos para diferentes tareas**

- Cada subagente tiene su propio contexto independiente
- Cada worktree aísla una feature completa
- Conversaciones separadas para diferentes aspectos

**Beneficio:** Previene "context rot" y contaminación cruzada.

## Subagentes: Planificadores y Descubridores, NO Ejecutores

### Filosofía Central

**Regla de Oro:** Los subagentes realizan research, descubren requisitos, y crean planes detallados, pero NUNCA implementan código directamente.

### Tipos de Subagentes

#### 1. Descubrimiento (Fase 0)
- **requirements-engineer**: Transforma ideas vagas en requisitos claros
  - Usa metodologías: 5W1H, JTBD, User Stories
  - Genera: `.claude/sessions/discovery_{feature}.md`
  - **Output**: Problema statement, criterios de aceptación, scope

#### 2. Planificación Técnica (Fase 1-3)
Los subagentes de desarrollo crean planes de implementación detallados pero NO ejecutan

### ¿Por Qué?

1. **Optimización de Tokens:** Los subagentes pueden investigar extensamente sin saturar el contexto principal
2. **Separación de Concerns:** Planificación vs Implementación son responsabilidades distintas
3. **Control Centralizado:** Un solo agente (el principal) implementa, evitando conflictos
4. **Documentación Rica:** Los planes se persisten y sirven como documentación

### Anatomía de un Subagente

```yaml
---
name: backend-developer
description: Use this agent when you need backend architecture and API design
tools: Read, Grep, Glob, Bash, WebFetch, WebSearch
model: inherit
---

## Goal
Propose a detailed implementation plan for backend features.
**NEVER do the actual implementation, just propose implementation plan**
Save the implementation plan in `.claude/doc/{feature_name}/backend_plan.md`

## Rules
- NEVER do the actual implementation
- NEVER run build or dev server
- Before you start, MUST read `.claude/sessions/context_session_{feature}.md`
- Your final message MUST include the plan file path created
```

## Git Worktrees: Desarrollo Paralelo

### ¿Qué son los Git Worktrees?

Git Worktrees permiten tener múltiples directorios de trabajo desde un único repositorio, cada uno en una rama diferente.

### Ventajas con Claude Code

1. **Sin Context Switching:** Cada instancia de Claude mantiene su propio contexto
2. **Trabajo Paralelo Real:** Múltiples features simultáneamente sin interferencias
3. **Aislamiento Perfecto:** Cada worktree es independiente
4. **Cleanup Fácil:** Remover worktree no afecta el repo principal

### Comandos Esenciales

```bash
# Crear worktree
git worktree add ./.trees/feature-name -b feature-name

# Listar worktrees
git worktree list

# Eliminar worktree
git worktree remove ./.trees/feature-name

# Limpiar worktrees eliminados en remote
git worktree prune
```

### Workflow con Slash Command

```bash
# En Claude Code
/worktree new-feature

# Automáticamente:
# 1. Crea worktree en ./.trees/feature-new-feature
# 2. Cambia al directorio
# 3. Activa plan mode
# 4. Crea context_session
# 5. Identifica agentes necesarios
```

## Casos de Uso Reales

### Ejemplo 1: Requisitos Vagos → Feature Clara (NUEVO)

**Solicitud inicial:** "Necesito mejorar el manejo de usuarios"

```bash
# Paso 1: Descubrimiento
/discover "necesito mejorar el manejo de usuarios"

# El requirements-engineer pregunta:
# - ¿Qué problemas específicos tienes con usuarios actuales?
#   → "Los administradores no pueden bloquear usuarios rápidamente cuando detectan actividad sospechosa"
# - ¿Quién necesita esta capacidad?
#   → "Administradores y moderadores del sistema"
# - ¿Qué tan rápido debe ser?
#   → "Menos de 30 segundos desde detección hasta bloqueo"
# - ¿Qué pasa después del bloqueo?
#   → "Usuario no puede hacer login, recibe email, admin puede desbloquear después"

# Resultado: .claude/sessions/discovery_user_blocking.md
User Story:
Como administrador
Quiero poder bloquear/desbloquear usuarios en menos de 30 segundos
Para detener actividad sospechosa inmediatamente

Criterios:
- [ ] Botón de bloqueo visible en perfil de usuario
- [ ] Confirmación antes de bloquear
- [ ] Usuario bloqueado no puede hacer login
- [ ] Email automático notificando al usuario
- [ ] Admin puede desbloquear desde el mismo lugar
- [ ] Log de auditoría registra bloqueos/desbloqueos

# Paso 2: Planificación técnica
/worktree user-blocking-feature

# Ahora con requisitos claros, los subagentes técnicos pueden planificar efectivamente
```

**Impacto:** De "mejorar manejo de usuarios" (vago) a "feature de bloqueo/desbloqueo con criterios específicos" (claro)

### Ejemplo 2: Agregador de Noticias + Dashboard Kanban (Del Video Original)

**Contexto:** Implementación en paralelo de:
1. Research de mercado para agregador de noticias
2. Dashboard Kanban con drag-and-drop
3. Migración de pip a poetry
4. Actualización de Pydantic

**Ejecución:**

```bash
# Terminal 1: Research (no requiere worktree)
claude
> /ideation agregador de noticias de IA

# Terminal 2: Feature Kanban (en worktree)
cd .trees/feature-kanban-dashboard
claude --permission-mode plan
> Implementar dashboard kanban para noticias con estados: to_read, reading, completed

# Terminal 3: Mantenimiento (repo principal)
claude
> Migrar el proyecto de pip a poetry y actualizar Pydantic a última versión
```

**Resultado:**
- Research completado en `.claude/research/ai-news-aggregator.md`
- Dashboard implementado y validado
- Proyecto migrado a poetry
- Pydantic actualizado
- Todo sin pérdidas de contexto

## Comparación: Cloud Code vs Cursor

| Aspecto | Cloud Code | Cursor |
|---------|-----------|--------|
| **Rol** | Compañero de pair programming | IDE + Asistente |
| **Ubicación** | Terminal | Editor |
| **Fase** | Planificación + Implementación | Revisión + Retoque |
| **Enfoque** | Features completas (sprint) | Asistencia interactiva |
| **Tools** | Acceso directo a terminal | Integración IDE |
| **Workflow** | Diseño → Implementación → QA | Edición asistida en tiempo real |

**Complementariedad:** Usa Cloud Code para planificar e implementar features completas, y Cursor para revisar y retocar detalles.

## Métricas de Optimización

Según la experiencia del autor del video:

### Ahorro de Tokens

**Sin framework:**
- Context con 50+ archivos de código
- Conversaciones de 100K+ tokens
- Regeneración constante de código

**Con framework:**
- Context con archivos .md (planes)
- Conversaciones de 10-20K tokens
- Código generado una sola vez

**Ahorro estimado:** 70-80% en consumo de tokens

### Reducción de Errores

- **Pérdida de contexto:** Eliminada (persistencia en archivos)
- **Conflictos de código:** Eliminados (solo agente principal implementa)
- **Iteraciones innecesarias:** Reducidas 60% (planificación detallada upfront)

### Velocidad de Desarrollo

- **Features pequeñas:** 2-3x más rápido
- **Features complejas:** 3-5x más rápido
- **Trabajo paralelo:** Limitado solo por hardware

## Best Practices

### 0. Clarifica Requisitos Primero (NUEVO)

✅ **HACER:**
- Evaluar si requisitos están claros antes de planificar
- Usar `/discover` cuando hay ambigüedad
- Validar documento de descubrimiento con usuario
- Definir criterios de aceptación medibles

❌ **NO HACER:**
- Asumir que entiendes requisitos vagos
- Saltar directo a soluciones técnicas
- Aceptar criterios no medibles ("rápido", "fácil")
- Implementar sin entender el problema real

### 1. Planifica Siempre

✅ **HACER:**
- Activar plan mode antes de implementar
- Iterar en el plan hasta que esté perfecto
- Involucrar a todos los subagentes necesarios

❌ **NO HACER:**
- Saltar directo a implementación
- Aceptar el primer plan sin revisar

### 2. Usa Persistencia Agresivamente

✅ **HACER:**
- Guardar todo en archivos .md
- Actualizar context_session con progreso
- Documentar decisiones importantes

❌ **NO HACER:**
- Confiar solo en memoria de conversación
- Perder información al cambiar de tarea

### 3. Aprovecha Worktrees

✅ **HACER:**
- Una feature = un worktree
- Múltiples features en paralelo
- Limpiar worktrees completados

❌ **NO HACER:**
- Trabajar todo en main
- Cambiar de rama constantemente

### 4. Especializa Subagentes

✅ **HACER:**
- Limitar herramientas de cada subagente
- Darles roles claros y específicos
- Reutilizarlos entre proyectos

❌ **NO HACER:**
- Subagentes genéricos con todas las herramientas
- Dejarles hacer implementación

### 5. Commitea Frecuentemente

✅ **HACER:**
- Commit después de cada feature validada
- Mensajes descriptivos con contexto
- Incluir co-author de Claude

❌ **NO HACER:**
- Commits gigantes con múltiples features
- Mensajes vagos

## Troubleshooting Común

### Problema: "Subagente implementó código"

**Causa:** System prompt del subagente no es lo suficientemente explícito

**Solución:**
```markdown
## Rules
- **NEVER do the actual implementation, or run build or dev**
- Your goal is to just research and the developer will handle the actual building
```

### Problema: "Pérdida de contexto al resumir"

**Causa:** No se leyó el archivo de sesión

**Solución:**
```markdown
## Rules
- Before you do any work, MUST view files in `.claude/sessions/context_session_{feature}.`
```

### Problema: "Worktrees causan conflictos"

**Causa:** Modificaciones en archivos compartidos

**Solución:**
- Cada worktree debe trabajar en archivos independientes
- Si hay overlap, trabajar secuencialmente, no en paralelo

### Problema: "Consumo excesivo de tokens"

**Causa:** No se están usando planes, se está cargando código

**Solución:**
- Asegurar que subagentes crean planes
- Agente principal lee planes, NO código completo
- Usar compress strategy agresivamente

## Conclusión

Este framework representa un cambio de paradigma en cómo trabajamos con AI coding tools. En lugar de usar Claude Code como un "copiloto glorificado", lo elevamos a un **compañero de equipo especializado** con workflows bien definidos y optimización constante del contexto.

Los resultados hablan por sí mismos:
- ✅ 70-80% menos tokens consumidos
- ✅ Código más limpio y mantenible
- ✅ Capacidad de trabajar en múltiples features simultáneamente
- ✅ Documentación automática completa
- ✅ Zero pérdidas de contexto
- ✅ **Requisitos claros desde el inicio** (v1.1)
- ✅ **Menos retrabajo por malentendidos** (v1.1)

**Novedades v1.1:**
- 🎯 **Fase 0: Descubrimiento de Requisitos** - Transforma ideas vagas en requisitos claros antes de planificar
- 🤖 **requirements-engineer** - Subagente especializado en elicitación de requisitos
- 📋 **Metodologías probadas** - 5W1H, Jobs-to-be-Done, User Stories con criterios medibles
- 💬 **Comando /discover** - Workflow guiado para clarificar necesidades

**Próximo paso:** Implementa el framework en tu proyecto siguiendo el tutorial paso a paso en `tutorial-paso-a-paso.md`.

---

**Referencias completas:** Ver `referencias.md`

**Análisis detallado de diagramas:** Ver `diagrama-framework-explicado.md` y `optimizacion-contexto-explicado.md`

**Deep dive técnico:** Ver `context-engineering-deep-dive.md` y `git-worktrees-guide.md`
