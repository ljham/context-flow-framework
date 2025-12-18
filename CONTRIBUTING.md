# Guía de Contribución

¡Gracias por tu interés en contribuir al Context Optimization Framework! Este documento te guiará en el proceso de contribución.

---

## Tabla de Contenidos

- [Cómo Contribuir](#cómo-contribuir)
- [Desarrollo Local](#desarrollo-local)
- [Estructura de Subagentes](#estructura-de-subagentes)
- [Testing y Validación](#testing-y-validación)
- [Proceso de Pull Request](#proceso-de-pull-request)
- [Code of Conduct](#code-of-conduct)
- [Proceso de Release](#proceso-de-release)

---

## Cómo Contribuir

Hay muchas formas de contribuir al proyecto:

### 1. Reportar Bugs

- Usa [GitHub Issues](https://github.com/ljham/context-flow-framework/issues/new)
- Incluye:
  - Descripción clara del problema
  - Pasos para reproducir
  - Comportamiento esperado vs comportamiento actual
  - Versión del framework y Claude Code
  - Logs relevantes (si aplica)

### 2. Proponer Nuevas Features

- Abre un [GitHub Discussion](https://github.com/ljham/context-flow-framework/discussions) primero
- Explica:
  - Qué problema resuelve
  - Cómo beneficiaría a los usuarios
  - Propuesta de implementación (opcional)

### 3. Mejorar Documentación

- Corregir typos, mejorar claridad, añadir ejemplos
- Los archivos de documentación están en `docs/`
- Pull requests de documentación son siempre bienvenidos

### 4. Crear Nuevos Subagentes

- Ver sección [Estructura de Subagentes](#estructura-de-subagentes)
- Asegúrate de que el subagente tenga un propósito único
- Incluye documentación completa

### 5. Mejorar Subagentes Existentes

- Los 11 subagentes actuales están en `.claude/agents/`
- Mejoras pueden incluir:
  - Metodologías más detalladas
  - Mejores prompts
  - Protocolos de error handling
  - Ejemplos adicionales

---

## Desarrollo Local

### 1. Fork y Clone

```bash
# Fork el repositorio en GitHub primero
git clone https://github.com/ljham/context-flow-framework.git
cd claude-context-framework
```

### 2. Instalar el Plugin Localmente

```bash
# Opción A: Link simbólico (desarrollo activo)
ln -s $(pwd)/.claude ~/.claude/plugins/context-optimization-framework

# Opción B: Instalar desde directorio local
cd tu-proyecto-test
claude plugins install /path/to/claude-context-framework
```

### 3. Probar Cambios

```bash
# Crear proyecto de prueba
mkdir test-framework
cd test-framework

# Probar comandos
claude
> /discover "test feature"
> /worktree test-feature
> /work
```

### 4. Verificar Estructura

```bash
# Verificar que todos los archivos están en su lugar
ls .claude/agents/    # Deberían aparecer 11 agentes
ls .claude/commands/  # Deberían aparecer 4 comandos
ls docs/              # Deberían aparecer 7 archivos de documentación
```

---

## Estructura de Subagentes

### Anatomía de un Subagente

Cada subagente en `.claude/agents/` sigue esta estructura:

```markdown
---
subagent_type: nombre-del-subagente
description: Descripción breve (1-2 líneas)
model: claude-sonnet-4-5 (o haiku para tareas simples)
---

# Nombre del Subagente

## Propósito

Descripción detallada de qué hace este subagente y cuándo debe ser usado.

## Cuándo Usar Este Subagente

- Caso de uso 1
- Caso de uso 2
- Caso de uso 3

## Metodología

### Fase 1: Análisis
Pasos detallados...

### Fase 2: Planificación
Pasos detallados...

### Fase 3: Documentación
Pasos detallados...

## Archivo de Salida

**Ubicación:** `.claude/doc/{feature}/nombre_del_plan.md`

**Formato esperado:**
\```markdown
# Plan de {Componente}

## Resumen Ejecutivo
...

## Detalles Técnicos
...

## Checklist de Implementación
- [ ] Tarea 1
- [ ] Tarea 2
\```

## Reglas Importantes

1. **NUNCA implementar código** - Solo planificar
2. **NUNCA correr builds o servidores** - Solo diseñar
3. **SIEMPRE crear archivo de salida** - En `.claude/doc/{feature}/`
4. **SIEMPRE actualizar context_session** - Añadir resumen del plan

## Protocolo de Error Handling

[Regla de 3 intentos, escalación, etc.]

## Ejemplos

### Ejemplo 1: Feature Simple
\```
Input: User authentication
Output: Plan detallado en backend_plan.md
\```

### Ejemplo 2: Feature Compleja
\```
Input: Real-time chat con IA
Output: Plan detallado con arquitectura y dependencias
\```
```

### Crear un Nuevo Subagente

1. **Copia un subagente existente** como template
2. **Define el propósito único** - ¿Qué hace que otros no hacen?
3. **Especifica cuándo usarlo** - Triggers claros
4. **Documenta la metodología** - Pasos concretos
5. **Define el output** - Formato y ubicación exactos
6. **Añade ejemplos** - Casos de uso reales
7. **Actualiza `.claude/plugin.json`** - Añade a la lista de agents

### Checklist para Nuevos Subagentes

- [ ] Propósito único y bien definido
- [ ] Descripción clara en frontmatter
- [ ] Sección "Cuándo Usar Este Subagente"
- [ ] Metodología paso a paso
- [ ] Archivo de salida especificado
- [ ] Reglas de "NUNCA implementar código"
- [ ] Protocolo de error handling
- [ ] Al menos 2 ejemplos
- [ ] Añadido a `plugin.json`
- [ ] Documentado en `README.md`

---

## Testing y Validación

### Validación Manual

1. **Instalar localmente**
2. **Crear proyecto test**
3. **Probar cada comando slash**
4. **Verificar que los subagentes crean sus archivos**
5. **Revisar que el agente principal puede leer los planes**

### Checklist de Validación

```bash
# 1. Verificar estructura
[ ] .claude/plugin.json existe y es válido JSON
[ ] Todos los agents listados en plugin.json existen
[ ] Todos los commands listados en plugin.json existen
[ ] Documentación en docs/ está completa

# 2. Probar workflows
[ ] /discover crea discovery_{feature}.md
[ ] /ideation crea archivos en .claude/research/
[ ] /worktree crea worktree en .trees/
[ ] /work puede leer todos los planes

# 3. Probar subagentes (en plan mode)
[ ] backend-developer crea backend_plan.md
[ ] frontend-developer crea frontend_plan.md
[ ] pydantic-ai-architect crea pydantic_agents_plan.md
[ ] shadcn-ui-architect crea ui_plan.md
[ ] ui-ux-analyzer crea ux_analysis.md
[ ] backend-test-engineer crea testing_plan.md
[ ] frontend-test-engineer crea frontend_testing_plan.md
[ ] qa-criteria-validator crea qa_report.md
[ ] requirements-engineer crea discovery_{feature}.md
[ ] product-strategist-agent crea {name}_strategy.md
[ ] research-analyst-agent crea {topic}_research.md
```

### Testing de Comandos

```bash
# Test /discover
/discover "necesito un sistema de notificaciones"
# Verifica: .claude/sessions/discovery_notificaciones.md existe

# Test /worktree
/worktree test-feature
# Verifica: .trees/test-feature/ existe y es un worktree válido

# Test /ideation
/ideation "plataforma de e-learning con IA"
# Verifica: .claude/research/{nombre}_strategy.md existe
```

---

## Proceso de Pull Request

### Antes de Abrir un PR

1. **Fork y crea una branch**
   ```bash
   git checkout -b feature/nombre-descriptivo
   # o
   git checkout -b fix/descripcion-del-bug
   ```

2. **Haz tus cambios** siguiendo las guías de arriba

3. **Prueba localmente** con el checklist de validación

4. **Commit con mensajes descriptivos**
   ```bash
   git commit -m "feat: añadir subagente para testing E2E"
   git commit -m "fix: corregir path en backend-developer"
   git commit -m "docs: mejorar ejemplos en README"
   ```

### Formato de Commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` Nueva feature
- `fix:` Corrección de bug
- `docs:` Solo documentación
- `refactor:` Cambios de código que no añaden features ni corrigen bugs
- `test:` Añadir o corregir tests
- `chore:` Cambios en build process, dependencies, etc.

### Abrir el PR

1. **Push tu branch**
   ```bash
   git push origin feature/nombre-descriptivo
   ```

2. **Abre PR en GitHub**
   - Título descriptivo
   - Descripción completa:
     - ¿Qué cambia?
     - ¿Por qué?
     - ¿Cómo probarlo?
   - Referencia issues relacionados: "Closes #123"

3. **Revisión**
   - Responde a comentarios constructivamente
   - Haz cambios solicitados
   - Push actualizaciones al mismo branch

4. **Merge**
   - Un maintainer mergeará cuando esté aprobado
   - Usamos squash merge para mantener historial limpio

---

## Code of Conduct

### Nuestros Estándares

**Comportamiento Positivo:**
- Usar lenguaje inclusivo y acogedor
- Respetar puntos de vista y experiencias diferentes
- Aceptar críticas constructivas con gracia
- Enfocarse en lo mejor para la comunidad
- Mostrar empatía hacia otros miembros

**Comportamiento Inaceptable:**
- Lenguaje o imágenes sexualizadas
- Trolling, comentarios insultantes o ataques personales
- Acoso público o privado
- Publicar información privada de otros sin permiso
- Conducta que razonablemente se considere inapropiada

### Enforcement

Instancias de comportamiento inaceptable pueden reportarse a context-framework@example.com. Todas las quejas serán revisadas e investigadas.

---

## Proceso de Release

### Versionado

Seguimos [Semantic Versioning](https://semver.org/):

- **MAJOR** (X.0.0): Breaking changes
- **MINOR** (0.X.0): Nuevas features compatibles
- **PATCH** (0.0.X): Bug fixes compatibles

### Crear un Release

Solo maintainers pueden crear releases:

1. **Actualizar `CHANGELOG.md`**
   ```markdown
   ## [X.Y.Z] - YYYY-MM-DD
   ### Added
   - Feature 1
   ### Fixed
   - Bug 1
   ```

2. **Actualizar versión en `plugin.json`**
   ```json
   "version": "X.Y.Z"
   ```

3. **Commit y tag**
   ```bash
   git add CHANGELOG.md .claude/plugin.json
   git commit -m "chore: release vX.Y.Z"
   git tag -a vX.Y.Z -m "Release vX.Y.Z"
   git push origin main --tags
   ```

4. **Crear GitHub Release**
   ```bash
   gh release create vX.Y.Z \
     --title "vX.Y.Z: Título del Release" \
     --notes-file CHANGELOG.md
   ```

### Ciclo de Releases

- **Patches:** Según sea necesario (bugs críticos)
- **Minor:** Cada 2-4 semanas (nuevas features)
- **Major:** Según sea necesario (breaking changes)

---

## Preguntas Frecuentes

### ¿Puedo contribuir si soy principiante?

¡Absolutamente! Contribuciones de documentación, reportes de bugs, y mejoras pequeñas son perfectas para empezar.

### ¿Necesito conocimientos de Python?

Para usar el framework, no. Para contribuir código, ayuda pero no es estrictamente necesario para mejorar subagentes o documentación.

### ¿Cómo sé si mi idea de subagente es buena?

Abre un Discussion en GitHub. La comunidad te ayudará a validar la idea y refinarla.

### ¿Cuánto tiempo toma que mi PR sea revisado?

Usualmente 2-7 días. Si es urgente (bug crítico), menciona en el PR.

---

## Contacto

- **GitHub Issues:** https://github.com/ljham/context-flow-framework/issues
- **GitHub Discussions:** https://github.com/ljham/context-flow-framework/discussions
- **Email:** context-framework@example.com

---

¡Gracias por contribuir al Context Optimization Framework! 🎉
