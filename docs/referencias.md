# Referencias Completas del Framework

## Fuente Principal

### Video de YouTube
- **Título:** Deep Dive en Cloud Code
- **URL:** https://youtu.be/NJ6sO_0BoTA
- **Autor:** [Autor del canal]
- **Duración:** ~40 minutos
- **Descripción:** Explicación completa del framework de optimización de contexto con subagentes y worktrees para Claude Code en proyectos de producción

### Materiales del Video

**Transcripción:**
- Archivo: `anexos/transcripcion-video.md`
- Contenido: Transcripción completa del video con timestamps implícitos

**Diagramas Visuales:**
1. `anexos/framework.png` - Diagrama de flujo de trabajo de 5 fases
2. `anexos/optimizacion-contexto.png` - Diagrama de los 5 beneficios clave

**Archivos de Configuración:**
1. `anexos/CLAUDE.md` - Workflow rules y configuración de subagentes
2. `anexos/worktree.md` - Comando slash para Git worktrees
3. `anexos/ideation.md` - Comando slash para research de producto
4. `anexos/pydantic-ai-architect.md` - Ejemplo real de subagente especializado

## Documentación Oficial de Claude Code

### Core Documentation

- **Subagentes:**
  https://code.claude.com/docs/en/sub-agents.md
  Documentación oficial sobre creación y uso de subagentes

- **Hooks:**
  https://code.claude.com/docs/en/hooks.md
  Referencia completa de hooks para eventos del ciclo de vida

- **Guía de Hooks:**
  https://code.claude.com/docs/en/hooks-guide.md
  Guía práctica para implementar hooks

- **Slash Commands:**
  https://code.claude.com/docs/en/slash-commands.md
  Cómo crear comandos personalizados reutilizables

- **Settings:**
  https://code.claude.com/docs/en/settings.md
  Configuración global y local de Claude Code

- **Memoria (CLAUDE.md):**
  https://code.claude.com/docs/en/memory.md
  Sistema de memoria jerárquica con archivos CLAUDE.md

- **Workflows Comunes:**
  https://code.claude.com/docs/en/common-workflows.md
  Patrones recomendados de uso

- **Mapa de Documentación:**
  https://code.claude.com/docs/en/claude_code_docs_map.md
  Índice completo de toda la documentación

## Context Engineering

### Artículos Fundamentales

1. **Effective Context Engineering for AI Agents - Anthropic**
   https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
   Artículo oficial de Anthropic sobre context engineering
   *Conceptos clave: Write, Select, Compress, Isolate*

2. **Context Engineering - LangChain Blog**
   https://blog.langchain.com/context-engineering-for-agents/
   Aplicación de context engineering en sistemas de agentes
   *Conceptos clave: Multi-agent systems, context management*

3. **Context Engineering Guide - Prompt Engineering Guide**
   https://www.promptingguide.ai/guides/context-engineering-guide
   Guía práctica de context engineering
   *Conceptos clave: Optimization strategies, best practices*

4. **Context Engineering for Reliable AI Agents - Kubiya**
   https://www.kubiya.ai/blog/context-engineering-ai-agents
   Context engineering en producción
   *Conceptos clave: Reliability, production patterns*

5. **Context Engineering - LlamaIndex**
   https://www.llamaindex.ai/blog/context-engineering-what-it-is-and-techniques-to-consider
   Técnicas avanzadas de context engineering
   *Conceptos clave: RAG, context windows, optimization*

6. **Context Engineering for AI Agents - DataCamp**
   https://www.datacamp.com/blog/context-engineering
   Tutorial educativo de context engineering
   *Conceptos clave: Practical examples, code samples*

### Papers y Recursos Técnicos

7. **Context Engineering for Agents - Research Blog**
   https://rlancemartin.github.io/2025/06/23/context_engineering/
   Investigación académica sobre context engineering

8. **Architecting efficient context-aware multi-agent framework**
   https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/
   Google Developers Blog - Multi-agent en producción

9. **Cutting Through the Noise: Smarter Context Management**
   https://blog.jetbrains.com/research/2025/12/efficient-context-management/
   JetBrains Research - Gestión eficiente de contexto

10. **Context Engineering: Lessons from Building Manus**
    https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus
    Lecciones de construcción de sistema de agentes real

## Git Worktrees

### Guías y Tutoriales

1. **Using Git Worktrees for Parallel AI Development - Steve Kinney**
   https://stevekinney.com/courses/ai-development/git-worktrees
   Curso completo sobre worktrees con AI tools
   *Conceptos clave: Parallel development, AI integration*

2. **Git Worktrees and Claude Code: Guide for Developers in 2025**
   https://www.geeky-gadgets.com/how-to-use-git-worktrees-with-claude-code-for-seamless-multitasking/
   Guía específica para Claude Code + Worktrees
   *Conceptos clave: Seamless multitasking, workflow optimization*

3. **Using Git Worktrees for Multi-Feature Development with AI Agents**
   https://www.nrmitchi.com/2025/10/using-git-worktrees-for-multi-feature-development-with-ai-agents/
   Desarrollo multi-feature con AI agents
   *Conceptos clave: Feature isolation, parallel workflows*

4. **Parallel AI Coding with Git Worktrees and Custom Claude Code Commands**
   https://docs.agentinterviews.com/blog/parallel-ai-coding-with-gitworktrees/
   Comandos custom para workflow paralelo
   *Conceptos clave: Automation, custom commands*

5. **Git Worktree: Manage Git Workflow Efficiently - DevDynamics**
   https://devdynamics.ai/blog/understanding-git-worktree-to-fast-track-software-development-process/
   Gestión eficiente con worktrees
   *Conceptos clave: Fast-tracking development, efficiency*

### Recursos Adicionales

6. **Git Worktree Reference and Best Practices**
   https://gist.github.com/induratized/49cdedace4a200fa8ae32db9ba3e9a44
   Referencia técnica completa (GitHub Gist)

7. **Git Worktrees: Boost Productivity with Parallel Branching**
   https://devot.team/blog/git-worktrees
   Best practices para productividad

8. **Using Git Worktrees for Concurrent Development - Ken Muse**
   https://www.kenmuse.com/blog/using-git-worktrees-for-concurrent-development/
   Desarrollo concurrente con worktrees

9. **How to Use Git Worktree: Step-by-Step Example**
   https://www.graphapp.ai/blog/how-to-use-git-worktree-a-step-by-step-example
   Tutorial paso a paso

10. **Using Git Worktrees for Multiple Working Directories**
    https://www.geeksforgeeks.org/git/using-git-worktrees-for-multiple-working-directories/
    Guía educativa de GeeksforGeeks

## Repositorios de Referencia

### Claude Code Templates
- **URL:** https://github.com/danielavila/claude-code-templates
  (Mencionado en el video como fuente de inspiración)
- **Contenido:** Comandos, hooks, agentes de ejemplo
- **Nota:** Adaptar a necesidades específicas, no copiar directamente

## Herramientas y Tecnologías Mencionadas

### Model Context Protocol (MCP)
- **Documentación:** https://modelcontextprotocol.io/
- **Servidores MCP utilizados en el video:**
  - Playwright - Testing y UI interaction
  - Context7 - Documentación actualizada de librerías
  - Sequential Thinking - Razonamiento extendido

### Pydantic AI
- **Documentación:** https://ai.pydantic.dev/
- **Repositorio:** https://github.com/pydantic/pydantic-ai
- **Uso en el video:** Framework para agentes con Pydantic

### Poetry (Python Package Manager)
- **Documentación:** https://python-poetry.org/
- **Uso:** Reemplazo de pip mencionado en el video

## Conceptos Teóricos

### Context Rot
- **Definición:** Degradación de la calidad de respuestas al aumentar tokens en contexto
- **Paper:** "Lost in the Middle: How Language Models Use Long Contexts"
  https://arxiv.org/abs/2307.03172

### Prompt Engineering
- **Guía Oficial:** https://www.promptingguide.ai/
- **Anthropic Docs:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering

### Multi-Agent Systems
- **Survey Paper:** "A Survey on Multi-Agent Deep Reinforcement Learning"
- **Recursos:** LangGraph documentation

## Comparaciones Mencionadas

### Claude Code vs Cursor
- **Claude Code:** https://code.claude.com/
- **Cursor:** https://cursor.sh/
- **Uso sugerido:** Complementario, no excluyente

### Otros CLIs Mencionados
- Gemini CLI
- Quinn CLI
- Cursor Agent

## Arquitecturas de Software

### Arquitectura Hexagonal
- **Artículo:** "Hexagonal Architecture" - Alistair Cockburn
- **Uso en el video:** Ejemplo de boilerplate del autor

### MVC (Model-View-Controller)
- **Documentación:** Patrón clásico mencionado como contraste

## Comunidad y Soporte

### Discord del Autor
- **Mención:** Video invita a unirse a comunidad Discord
- **Contenido:** Discusiones sobre el framework, soporte

### GitHub Issues
- **Claude Code:** Reportar problemas y feedback
- **URL:** https://github.com/anthropics/claude-code/issues

## Notas de Versión

**Fecha de Creación de Este Framework:** Enero 2025
**Versión de Claude Code:** Compatible con versiones 2024-2025
**Última Actualización de Referencias:** 2025-01-15

---

## Cómo Citar Este Framework

Si usas este framework en tus proyectos o presentaciones:

```
Framework de Optimización de Contexto para Claude Code
Video: https://youtu.be/NJ6sO_0BoTA
Implementación: [Tu repo]
Basado en: Context Engineering + Git Worktrees + Sub-Agent Architecture
```

## Contribuciones Adicionales

Si encuentras recursos adicionales valiosos, considera:
1. Agregarlos a este archivo
2. Compartirlos con la comunidad
3. Documentar cómo los usaste en tu proyecto

---

**Última actualización:** 2025-01-15
**Mantenedor:** [Tu nombre]
**Licencia:** MIT (adaptar según tu proyecto)
