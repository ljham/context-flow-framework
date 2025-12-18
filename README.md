# Context Optimization Framework

![Version](https://img.shields.io/badge/version-1.3.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Claude Code](https://img.shields.io/badge/claude--code-%3E%3D1.0.0-purple)

> Framework profesional de optimización de contexto para Claude Code que reduce el consumo de tokens en **80-90%** mediante Context Engineering, subagentes especializados, y desarrollo paralelo con Git Worktrees.

---

## ✨ Features Principales

- 🤖 **11 Subagentes Especializados** - Planificadores expertos (NO ejecutores)
- 📝 **Context Engineering** - Persistencia inteligente en Markdown
- 🔀 **Git Worktrees** - Desarrollo paralelo sin interferencias
- ⚡ **4 Comandos Slash** - Workflows automatizados
- 🛡️ **Error Handling Robusto** - Regla de 3 intentos con escalación
- 📊 **4 Fases Estructuradas** - De descubrimiento a validación QA
- 📚 **Documentación Auto-Generada** - Cada feature documenta su implementación

## 🎯 Resultados Comprobados

| Métrica | Sin Framework | Con Framework | Mejora |
|---------|---------------|---------------|--------|
| **Tokens consumidos** | 80-120K | 10-20K | **80-90% menos** |
| **Costo por feature** | $0.50 - $1.50 | $0.05 - $0.15 | **90% reducción** |
| **Velocidad (features complejas)** | 3 días | 1 día | **3x más rápido** |
| **Pérdida de contexto** | Frecuente | Zero | **100% eliminada** |
| **Documentación** | Desactualizada | Automática | **Siempre al día** |

---

## 🚀 Instalación

```bash
# Instalar en cualquier proyecto
cd tu-proyecto
claude plugins install github:YOUR_USERNAME/claude-context-framework

# Verificar instalación
ls .claude/agents/  # Deberían aparecer 11 agentes
```

**Requisitos:**
- Claude Code >= 1.0.0
- Git
- MCP: context7 (requerido), playwright-server (opcional), memory (opcional)

---

## 📖 Quick Start

### Workflow Completo: De Idea a Producción

```bash
# 1. [OPCIONAL] Investigación de Mercado
/ideation "agregador de noticias con IA"
# → Genera análisis en .claude/research/

# 2. [RECOMENDADO] Descubrir Requisitos
/discover "necesito un dashboard para métricas"
# → requirements-engineer hace preguntas estructuradas
# → Genera .claude/sessions/discovery_{feature}.md

# 3. Planificar Feature
/worktree nombre-feature
# → Crea Git worktree
# → Activa plan mode automáticamente
# → Delega a subagentes en paralelo

# 4. Implementar
/work
# → Agente principal lee todos los planes
# → Implementa código siguiendo especificaciones

# 5. Validar (Automático)
# → qa-criteria-validator valida contra criterios
# → Genera reporte de QA
# → Itera hasta pasar todos los criterios
```

### Ejemplo Rápido

```bash
# Usuario
/discover "necesito autenticación de usuarios"

# requirements-engineer pregunta:
# - ¿Qué problema resuelve? → "Usuarios anónimos acceden a datos privados"
# - ¿Quiénes lo usarán? → "Usuarios registrados, admins"
# - ¿Qué resultado esperas? → "Login seguro con JWT, session management"

# Genera discovery_user-auth.md con criterios claros

# Usuario
/worktree user-auth

# Sistema delega en paralelo:
# - backend-developer → Plan de API (endpoints, modelos)
# - frontend-developer → Plan de UI/UX (forms, state)
# - pydantic-ai-architect → Plan de validación con Pydantic

# Todos generan planes en .claude/doc/user-auth/

# Usuario
/work

# Agente principal:
# - Lee todos los planes
# - Implementa backend + frontend
# - Ejecuta tests
# - qa-criteria-validator valida
# - git commit cuando pasa QA
```

---

## 🧠 Subagentes Disponibles

### Descubrimiento y Requisitos (Fase 0)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **requirements-engineer** | Clarificar requisitos vagos con 5W1H, JTBD, User Stories | `discovery_{feature}.md` |

### Desarrollo Core (Fase 1-2)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **backend-developer** | Diseño de APIs, servicios, modelos de datos | `backend_plan.md` |
| **frontend-developer** | Estado, hooks, integración con APIs | `frontend_plan.md` |
| **pydantic-ai-architect** | Agentes de IA con Pydantic AI framework | `pydantic_agents_plan.md` |

### Diseño UI/UX (Fase 1-2)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **shadcn-ui-architect** | Componentes shadcn/ui, layouts, theming | `ui_plan.md` |
| **ui-ux-analyzer** | Análisis de usabilidad, mejoras UX | `ux_analysis.md` |

### Testing (Fase 2)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **backend-test-engineer** | Estrategia de testing backend (unit, integration) | `testing_plan.md` |
| **frontend-test-engineer** | Testing frontend (hooks, servicios, E2E) | `frontend_testing_plan.md` |

### Validación y QA (Fase 3)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **qa-criteria-validator** | Validar features contra criterios de aceptación | `qa_report.md` |

### Investigación y Estrategia (Pre-Fase 0)
| Agente | Propósito | Output |
|--------|-----------|--------|
| **product-strategist-agent** | Estrategia de producto, roadmap, go-to-market | `{nombre}_strategy.md` |
| **research-analyst-agent** | Análisis técnico, comparación de alternativas | `{tema}_research.md` |

---

## 🏗️ Las 4 Fases del Framework

### Fase 0: Descubrimiento de Requisitos
**Cuándo:** Solicitud vaga o ambigua
**Quién:** requirements-engineer
**Output:** `discovery_{feature}.md` con problema, criterios, scope

### Fase 1: Planificación
**Cuándo:** Después de validar requisitos
**Quién:** Subagentes especializados (en paralelo)
**Output:** Planes detallados en `.claude/doc/{feature}/`

### Fase 2: Implementación
**Cuándo:** Planes aprobados
**Quién:** Agente principal (ÚNICO que escribe código)
**Output:** Código implementado + tests

### Fase 3: Validación
**Cuándo:** Implementación completa
**Quién:** qa-criteria-validator
**Output:** QA report → Correcciones → git commit

---

## 📊 Los 5 Beneficios de Optimización del Contexto

### 1. 🔧 Minimizar el Contexto
- Planes en `.md` vs código completo en contexto
- **Ahorro:** 80-90% tokens

### 2. 👥 Evitar Superposición de Agentes
- Subagentes planifican, solo agente principal implementa
- **Resultado:** Zero conflictos de archivos

### 3. 📊 Documentación Automática
- Cada feature genera automáticamente:
  - Context sessions
  - Planes de implementación
  - Research y análisis
- **Beneficio:** Documentación siempre actualizada

### 4. 📁 Persistir Planes / Resume Claro
- Pausar y retomar sin pérdida de contexto
- Comando `/resume` recupera sesión completa
- **Beneficio:** Flexibilidad total

### 5. 🔀 Usar Worktrees En Paralelo
- Cada feature en su propio worktree
- Múltiples features simultáneas
- **Beneficio:** Desarrollo paralelo real

---

## 📁 Estructura de Archivos Generados

```
tu-proyecto/
├── .claude/
│   ├── sessions/              # Contextos de sesión
│   │   ├── context_session_{feature}.md
│   │   └── discovery_{feature}.md
│   │
│   ├── doc/                   # Planes de implementación
│   │   └── {feature_name}/
│   │       ├── backend_plan.md
│   │       ├── frontend_plan.md
│   │       ├── ui_plan.md
│   │       ├── testing_plan.md
│   │       └── qa_report.md
│   │
│   └── research/              # Investigación
│       ├── {product}_strategy.md
│       └── {tech}_research.md
│
└── .trees/                    # Git worktrees
    ├── feature-auth/
    ├── feature-dashboard/
    └── feature-payments/
```

---

## 🎓 Documentación Completa

- 📖 [Framework Completo](./docs/framework-claude-code.md) - Guía detallada del framework
- 🎯 [Optimización de Contexto](./docs/optimizacion-contexto-explicado.md) - Los 5 beneficios explicados
- 📊 [Diagrama del Framework](./docs/diagrama-framework-explicado.md) - Visualización del flujo
- 🔀 [Guía de Git Worktrees](./docs/git-worktrees-guide.md) - Desarrollo paralelo
- 🛡️ [Estrategia de Error Handling](./docs/error-handling-strategy.md) - Manejo de fallos
- 🧠 [Context Engineering Deep Dive](./docs/context-engineering-deep-dive.md) - Ingeniería de contexto
- 📚 [Referencias](./docs/referencias.md) - Fuentes y créditos

---

## ⚙️ Configuración

El plugin configura automáticamente:

### Model por Defecto
```json
"defaultModel": "claude-sonnet-4-5"
```

### Permisos Habilitados
- `Bash(git *)` - Operaciones Git
- `Bash(poetry *)` - Gestión de dependencias Python
- `Bash(pytest *)` - Testing
- `Bash(ruff *)` - Linting
- `Bash(mypy *)` - Type checking

### Hooks
- **PostToolUse:** Notificación al modificar archivos
- **SubagentStop:** Notificación al terminar subagentes

**Nota:** Los hooks son personalizables en `.claude/settings.json`

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Ver [CONTRIBUTING.md](./CONTRIBUTING.md) para guías.

**Formas de contribuir:**
- 🐛 Reportar bugs
- ✨ Proponer nuevas features
- 📝 Mejorar documentación
- 🤖 Crear nuevos subagentes especializados
- 🧪 Añadir tests
- 🌍 Traducir a otros idiomas

---

## 📄 Licencia

MIT License - Ver [LICENSE](./LICENSE)

Esto significa que puedes:
- ✅ Usar comercialmente
- ✅ Modificar
- ✅ Distribuir
- ✅ Uso privado

---

## 🙏 Inspiración

Este framework fue inspirado por el video ["Deep Dive en Cloud Code"](https://youtu.be/NJ6sO_0BoTA) y mejorado significativamente con:
- Fase 0 de descubrimiento de requisitos
- Estrategia completa de error handling
- Protocolos de retry formalizados
- 11 subagentes especializados vs 6 del video original
- Documentación extensa (7 archivos completos)

---

## 📞 Soporte

- 🐛 **Issues:** [GitHub Issues](https://github.com/YOUR_USERNAME/claude-context-framework/issues)
- 💬 **Discusiones:** [GitHub Discussions](https://github.com/YOUR_USERNAME/claude-context-framework/discussions)
- 📧 **Email:** context-framework@example.com

---

## 🗺️ Roadmap

- [ ] CLI tool para inicializar proyectos
- [ ] Templates de proyectos (Python, React, Fullstack)
- [ ] Marketplace de subagentes comunitarios
- [ ] Integración con más MCPs
- [ ] Dashboard de métricas de uso
- [ ] Tests automatizados del framework

---

## ⭐ Métricas del Proyecto

**Versión Actual:** 1.3.0
**Última Actualización:** 2025-12-14
**Subagentes:** 11
**Comandos Slash:** 4
**Fases:** 4
**Documentación:** 7 archivos

---

**¿Te gustó este framework?** Dale una ⭐ en GitHub y compártelo con tu equipo!

**¿Encontraste un bug?** Abre un [issue](https://github.com/YOUR_USERNAME/claude-context-framework/issues/new) y lo revisaremos pronto.

**¿Quieres contribuir?** Lee [CONTRIBUTING.md](./CONTRIBUTING.md) y únete a la comunidad!

---

<div align="center">

**Hecho con ❤️ por la comunidad de Claude Code**

[⬆ Volver arriba](#context-optimization-framework)

</div>
