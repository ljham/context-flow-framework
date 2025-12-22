# Changelog

Todos los cambios notables en el Context Optimization Framework serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

---

## [1.4.0] - 2025-12-22

### Added
- Auto-instalación de CLAUDE.md en nuevos proyectos
- Template del framework en `templates/CLAUDE.template.md`
- Detección automática de framework en comandos `/discover` y `/worktree`
- Copia automática de CLAUDE.md si no existe en el proyecto actual

### Changed
- Comandos `/discover` y `/worktree` ahora instalan automáticamente el framework
- Mejora de portabilidad del plugin entre proyectos
- Uso de variable `${CLAUDE_PLUGIN_ROOT}` para acceso a templates

### Benefits
- **DX mejorado**: No necesitas copiar manualmente CLAUDE.md a cada proyecto nuevo
- **Cero configuración**: El framework se auto-instala al primer uso
- **Consistencia**: Todos los proyectos usan la misma versión del framework
- **Mantenibilidad**: Actualizar el framework actualiza automáticamente nuevos proyectos

---

## [1.3.0] - 2025-12-14

### Added
- Metodología de planificación UI detallada en `shadcn-ui-architect`
- Verificaciones de calidad en componentes UI
- Protocolo de error handling en `shadcn-ui-architect`
- Metodología de análisis UX completa en `ui-ux-analyzer`
- Análisis heurístico sistemático en `ui-ux-analyzer`
- Protocolo de error handling en `ui-ux-analyzer`
- Flujo de trabajo estructurado en `product-strategist-agent`
- Formato de salida estandarizado en `product-strategist-agent`
- Protocolo de error handling en `product-strategist-agent`
- Metodología de investigación comparativa en `research-analyst-agent`
- Matriz de decisión para análisis técnico en `research-analyst-agent`
- Protocolo de error handling en `research-analyst-agent`

### Changed
- Ampliación de metodología y estructura en agentes UI/UX
- Mejora de calidad de subagentes de diseño
- Estandarización de formato y protocolos en agentes de investigación
- Recategorización de subagentes con separación clara de responsabilidades por fase
- Nueva categoría "Diseño UI/UX (Fase 1-2)" separada de "Validación y QA (Fase 3)"
- `qa-criteria-validator` ahora correctamente categorizado en "Validación y QA (Fase 3)"
- Estandarización de formato de salida en todos los subagentes

---

## [1.2.0] - 2025-12-11

### Added
- **Estrategia de Manejo de Fallos** con regla de 3 intentos (CRÍTICO)
- Protección contra loops infinitos en todas las fases
- Escalación formal al usuario con diagnóstico detallado
- Tracking de intentos en `error_log_{feature}.md`
- Timeouts sugeridos por fase y operación
- Templates de error logging y escalación
- Documentación completa de retry strategies

### Changed
- Sistema de error handling ahora sigue protocolo estructurado
- Subagentes ahora fallan gracefully después de 3 intentos
- Mejora en transparencia de errores para el usuario

---

## [1.1.0] - 2025-12-10

### Added
- **Fase 0: Descubrimiento de Requisitos** (nueva fase)
- Nuevo subagente: `requirements-engineer`
- Nuevo comando slash: `/discover`
- Template de discovery session
- Metodologías estructuradas: 5W1H, JTBD, User Stories
- Documento de salida: `discovery_{feature}.md`

### Changed
- Workflow completo actualizado con fase de descubrimiento
- Sistema ahora detecta requisitos vagos y activa Fase 0
- Mejora en clarificación de requisitos antes de planificación técnica

---

## [1.0.0] - 2025-12-01

### Added
- Sistema de 11 subagentes especializados
- 4 comandos slash: `/ideation`, `/worktree`, `/work`, `/discover`
- 4 fases estructuradas: Descubrimiento, Planificación, Implementación, Validación
- Context Engineering con persistencia en Markdown
- Soporte para Git Worktrees
- Subagentes de desarrollo core:
  - `backend-developer`
  - `frontend-developer`
  - `pydantic-ai-architect`
  - `backend-test-engineer`
  - `frontend-test-engineer`
- Subagentes de diseño UI/UX:
  - `shadcn-ui-architect`
  - `ui-ux-analyzer`
- Subagentes de validación:
  - `qa-criteria-validator`
- Subagentes de investigación:
  - `product-strategist-agent`
  - `research-analyst-agent`
- Documentación completa:
  - `framework-claude-code.md`
  - `optimizacion-contexto-explicado.md`
  - `diagrama-framework-explicado.md`
  - `git-worktrees-guide.md`
  - `error-handling-strategy.md`
  - `context-engineering-deep-dive.md`
  - `referencias.md`
- Estructura de archivos `.claude/`:
  - `sessions/` - Contextos de sesión
  - `doc/` - Planes de implementación
  - `research/` - Resultados de investigación
- Soporte para MCPs: context7, playwright-server, memory, n8n-mcp
- Optimización de tokens: 80-90% de reducción comprobada

### Changed
- Separación clara entre planificación (subagentes) y ejecución (agente principal)

---

## Leyenda de Tipos de Cambios

- **Added**: Nuevas features o funcionalidades
- **Changed**: Cambios en funcionalidades existentes
- **Deprecated**: Features que serán removidas en versiones futuras
- **Removed**: Features removidas
- **Fixed**: Corrección de bugs
- **Security**: Correcciones de vulnerabilidades de seguridad

---

## Proceso de Versionado

Este proyecto utiliza [Semantic Versioning](https://semver.org/lang/es/):

- **MAJOR** version (X.0.0): Cambios incompatibles en la API
- **MINOR** version (0.X.0): Nuevas funcionalidades compatibles con versiones anteriores
- **PATCH** version (0.0.X): Correcciones de bugs compatibles con versiones anteriores

---

## Enlaces

- [Documentación Completa](./docs/framework-claude-code.md)
- [Repositorio](https://github.com/YOUR_USERNAME/claude-context-framework)
- [Issues](https://github.com/YOUR_USERNAME/claude-context-framework/issues)
- [Pull Requests](https://github.com/YOUR_USERNAME/claude-context-framework/pulls)
