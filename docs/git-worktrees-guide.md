# Git Worktrees: Guía Completa para Desarrollo con IA

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [¿Qué son los Git Worktrees?](#qué-son-los-git-worktrees)
3. [Por Qué Usar Worktrees con AI Agents](#por-qué-usar-worktrees-con-ai-agents)
4. [Comandos Esenciales](#comandos-esenciales)
5. [Workflow Completo con Claude Code](#workflow-completo-con-claude-code)
6. [Estructura y Organización](#estructura-y-organización)
7. [Automatización con Slash Commands](#automatización-con-slash-commands)
8. [Troubleshooting Común](#troubleshooting-común)
9. [Mejores Prácticas](#mejores-prácticas)
10. [Referencias](#referencias)

## Introducción

Los **Git Worktrees** permiten tener múltiples directorios de trabajo desde un único repositorio Git, cada uno en una rama diferente. Esto es especialmente poderoso cuando trabajas con AI agents como Claude Code, ya que permite desarrollo paralelo verdadero sin conflictos de contexto.

### El Problema que Resuelven

**Flujo tradicional** (problemático con AI):
```bash
# Trabajando en feature-1
git checkout -b feature-1
# Claude Code trabaja aquí...

# Necesitas cambiar a feature-2
git stash  # ❌ Pierdes contexto
git checkout -b feature-2
# Claude Code ahora confundido con contexto mezclado
```

**Con Worktrees** (óptimo para AI):
```bash
# Feature 1 en su propio directorio
git worktree add ./.trees/feature-1 -b feature-1
cd .trees/feature-1
# Claude Code instance 1 trabaja aquí

# Feature 2 en directorio paralelo (simultáneo)
git worktree add ./.trees/feature-2 -b feature-2
cd .trees/feature-2
# Claude Code instance 2 trabaja aquí

# ✅ Sin conflictos, contextos aislados, trabajo paralelo real
```

## ¿Qué son los Git Worktrees?

### Concepto Básico

Un **worktree** es un directorio de trabajo vinculado a tu repositorio Git principal que te permite tener múltiples ramas checked out simultáneamente.

### Anatomía de un Worktree

```
proyecto/                    # Repositorio principal
├── .git/                   # Git directory principal
│   └── worktrees/          # Metadata de worktrees
│       ├── feature-1/      # Metadata del worktree 1
│       └── feature-2/      # Metadata del worktree 2
├── src/                    # Código en rama main
├── .trees/                 # Directorio para worktrees (convención)
│   ├── feature-1/          # Worktree 1 (rama feature-1)
│   │   ├── src/            # Código independiente
│   │   ├── .git → link     # Link al .git principal
│   │   └── ...
│   └── feature-2/          # Worktree 2 (rama feature-2)
│       ├── src/            # Código independiente
│       └── ...
```

### Diferencia con Branches Normales

| Aspecto | Branches Tradicionales | Worktrees |
|---------|------------------------|-----------|
| **Checkout** | Solo una rama a la vez | Múltiples ramas simultáneas |
| **Directorio** | Mismo directorio de trabajo | Directorios separados |
| **Context switching** | `git checkout` (lento) | `cd` (instantáneo) |
| **Conflictos** | Posibles al cambiar | Imposibles (aislados) |
| **AI agents** | Contexto mezclado | Contexto limpio por feature |

## Por Qué Usar Worktrees con AI Agents

### 1. Aislamiento de Contexto 🏝️

Los AI agents funcionan mejor con contextos limpios y enfocados.

**Problema sin worktrees**:
```
Claude trabajando en feature de Auth:
  - Ve archivos de Dashboard (irrelevantes)
  - Ve cambios mezclados de otras features
  - Contexto contaminado → Confusión → Errores
```

**Solución con worktrees**:
```
.trees/feature-auth/:
  - Solo archivos relacionados con Auth
  - Solo commits de Auth
  - Contexto limpio → Precisión → Calidad
```

### 2. Desarrollo Paralelo Real ⚡

**Caso de uso del video** (News Aggregator + Kanban Dashboard):

```bash
# Terminal 1: Claude Code trabajando en aggregator
cd .trees/feature-news-aggregator
claude
# Implementa scraping, parsing, storage...

# Terminal 2 (simultáneo): Claude Code trabajando en dashboard
cd .trees/feature-kanban-dashboard
claude
# Implementa UI, drag-and-drop, state management...

# ✅ Sin interferencia, 2x velocidad
```

### 3. No Más Context Switching 🚫

```bash
# Sin worktrees (lento y problemático):
git stash               # 5 segundos
git checkout feature-2  # 10 segundos
# Claude Code pierde contexto, necesita reorientarse
git checkout feature-1  # 10 segundos
git stash pop           # 5 segundos
# Total: 30+ segundos, contexto perdido

# Con worktrees (instantáneo):
cd .trees/feature-2     # 0.1 segundos
# Claude Code mantiene contexto completo
cd .trees/feature-1     # 0.1 segundos
# Total: 0.2 segundos, contexto preservado
```

### 4. Testing y Code Review Facilitado ✅

```bash
# Revisar PR mientras sigues desarrollando
git worktree add .trees/review-pr-123 pr-123
cd .trees/review-pr-123
# Claude Code revisa el código del PR

# Mientras tanto en .trees/feature-current/
# Sigues desarrollando sin interrupciones
```

## Comandos Esenciales

### Crear Worktree

```bash
# Sintaxis básica
git worktree add <path> -b <nueva-rama>

# Ejemplos
git worktree add ./.trees/feature-auth -b feature-auth
git worktree add ./.trees/hotfix-bug-123 -b hotfix/bug-123

# Desde rama existente
git worktree add ./.trees/review-pr main

# Desde tag
git worktree add ./.trees/release-1.0 v1.0.0
```

### Listar Worktrees

```bash
# Ver todos los worktrees
git worktree list

# Salida ejemplo:
# /home/user/proyecto           abc1234 [main]
# /home/user/proyecto/.trees/feature-auth   def5678 [feature-auth]
# /home/user/proyecto/.trees/review-pr      ghi9012 [pr-123]
```

### Eliminar Worktree

```bash
# Paso 1: Salir del directorio del worktree
cd /home/user/proyecto  # Volver al repo principal

# Paso 2: Remover el worktree
git worktree remove ./.trees/feature-auth

# O forzar remoción (si hay cambios sin commit)
git worktree remove --force ./.trees/feature-auth

# Limpiar worktrees huérfanos
git worktree prune
```

### Mover Worktree

```bash
# Mover a nueva ubicación
git worktree move ./.trees/feature-auth ./.trees/auth-v2
```

### Reparar Worktree

```bash
# Si moviste manualmente el directorio
git worktree repair

# Reparar worktree específico
git worktree repair ./.trees/feature-auth
```

## Workflow Completo con Claude Code

### Fase 1: Setup Inicial

```bash
# 1. Crear estructura de directorios (una vez)
mkdir -p .trees

# 2. Agregar a .gitignore (una vez)
echo ".trees/" >> .gitignore
git add .gitignore
git commit -m "chore: add .trees/ to gitignore"
```

### Fase 2: Iniciar Nueva Feature

**Opción A: Manual**
```bash
# 1. Crear worktree desde main
git worktree add ./.trees/feature-auth -b feature-auth

# 2. Entrar al worktree
cd .trees/feature-auth

# 3. Iniciar Claude Code en plan mode
claude --permission-mode plan

# 4. Claude crea context_session automáticamente
# .claude/sessions/context_session_auth.md
```

**Opción B: Con Slash Command (Automatizado)**
```bash
# Desde el repo principal
claude

# Dentro de Claude Code:
/worktree auth

# El comando automáticamente:
# - Crea el worktree
# - Cambia al directorio
# - Activa plan mode
# - Crea context_session
```

### Fase 3: Desarrollo

```bash
# En .trees/feature-auth/
claude

# Claude Code trabaja con contexto aislado:
# - Solo ve archivos de esta feature
# - Solo ve commits de esta rama
# - Session file específico de esta feature
```

### Fase 4: Completar Feature

```bash
# 1. Asegurarte de que todo está committed
git status

# 2. Volver al repo principal
cd ../..  # De .trees/feature-auth/ a raíz

# 3. Mergear la feature
git checkout main
git merge feature-auth

# 4. Push al remote
git push origin main
git push origin feature-auth

# 5. Eliminar el worktree (ya mergeado)
git worktree remove ./.trees/feature-auth

# 6. Eliminar rama remota (opcional)
git push origin --delete feature-auth
```

### Fase 5: Limpieza

```bash
# Verificar worktrees activos
git worktree list

# Remover worktrees completados
git worktree remove ./.trees/feature-auth
git worktree remove ./.trees/feature-dashboard

# Limpiar referencias huérfanas
git worktree prune

# Eliminar ramas locales mergeadas
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
```

## Estructura y Organización

### Convención de Nombres

```bash
# ✅ RECOMENDADO: Prefijo descriptivo
.trees/feature-{nombre}     # Features nuevas
.trees/hotfix-{issue}       # Hotfixes urgentes
.trees/refactor-{area}      # Refactorizaciones
.trees/experiment-{idea}    # Experimentos
.trees/review-pr-{numero}   # Code reviews

# Ejemplos
.trees/feature-user-auth
.trees/feature-payment-gateway
.trees/hotfix-memory-leak
.trees/refactor-api-layer
.trees/experiment-new-algorithm
.trees/review-pr-456
```

### Estructura de Proyecto Completa

```
my-project/
├── .git/                       # Git principal
│   └── worktrees/              # Metadata de worktrees
├── .claude/                    # Config Claude Code
│   ├── CLAUDE.md
│   ├── settings.json
│   ├── commands/
│   │   └── worktree.md        # ← Comando /worktree
│   ├── sessions/              # ← Session files de features
│   │   ├── context_session_auth.md
│   │   └── context_session_dashboard.md
│   └── doc/                   # ← Planes de features
│       ├── auth/
│       └── dashboard/
├── src/                        # Código main branch
├── .trees/                     # ⭐ Worktrees (gitignored)
│   ├── feature-auth/          # Worktree 1
│   │   ├── .claude/           # Comparte config (symlink)
│   │   ├── src/               # Código independiente
│   │   └── ...
│   └── feature-dashboard/     # Worktree 2
│       ├── .claude/
│       ├── src/
│       └── ...
├── .gitignore                 # Incluye .trees/
└── README.md
```

### Gitignore Recomendado

```gitignore
# Git Worktrees
.trees/

# Claude Code (temporal)
.claude/sessions/
.claude/doc/
.claude/research/
.claude/CLAUDE.local.md
.claude/settings.local.json

# Mantener config compartida (NO ignorar)
# .claude/CLAUDE.md
# .claude/settings.json
# .claude/agents/
# .claude/commands/
```

## Automatización con Slash Commands

### Comando /worktree

Ya implementado en `.claude/commands/worktree.md`:

```markdown
---
description: Crear Git worktree para feature aislada y activar plan mode
argument-hint: [nombre-feature]
---

Nombre de la feature: #$ARGUMENTS

Ejecutar los siguientes pasos:

1. Crear git worktree:
   git worktree add ./.trees/feature-$ARGUMENTS -b feature-$ARGUMENTS

2. Cambiar al directorio del worktree:
   cd .trees/feature-$ARGUMENTS

3. Activar plan mode (automático)

4. Crear archivo de sesión de contexto:
   .claude/sessions/context_session_$ARGUMENTS.md

5. Determinar qué agentes usar y si pueden paralelizarse

6. Listo para comenzar a planificar la implementación
```

### Uso

```bash
# Dentro de Claude Code
claude

# Crear worktree para nueva feature
/worktree user-authentication

# Claude automáticamente:
# 1. Crea .trees/feature-user-authentication/
# 2. Cambia a ese directorio
# 3. Activa plan mode
# 4. Crea context_session_user-authentication.md
# 5. Pregunta qué subagentes necesitas
```

### Script Complementario (Bash)

Puedes crear un script para gestión avanzada:

```bash
#!/bin/bash
# .claude/scripts/worktree-manager.sh

cmd=$1
feature=$2

case $cmd in
  create)
    git worktree add ./.trees/feature-$feature -b feature-$feature
    cd .trees/feature-$feature
    echo "✓ Worktree creado: .trees/feature-$feature"
    ;;

  list)
    echo "Worktrees activos:"
    git worktree list
    ;;

  clean)
    echo "Limpiando worktrees mergeados..."
    for worktree in .trees/*/; do
      branch=$(basename $worktree)
      if git branch --merged main | grep -q $branch; then
        git worktree remove .trees/$branch
        echo "✓ Removido: $branch (ya mergeado)"
      fi
    done
    git worktree prune
    ;;

  remove)
    git worktree remove ./.trees/feature-$feature
    echo "✓ Removido: feature-$feature"
    ;;

  *)
    echo "Uso: $0 {create|list|clean|remove} [feature-name]"
    exit 1
    ;;
esac
```

Uso:
```bash
# Crear
bash .claude/scripts/worktree-manager.sh create auth

# Listar
bash .claude/scripts/worktree-manager.sh list

# Limpiar mergeados
bash .claude/scripts/worktree-manager.sh clean

# Remover específico
bash .claude/scripts/worktree-manager.sh remove auth
```

## Troubleshooting Común

### Problema 1: "Cannot remove a locked working tree"

**Causa**: Worktree está actualmente en uso (tienes una terminal abierta en él).

**Solución**:
```bash
# Cerrar todas las terminales en ese worktree
# Luego:
git worktree remove --force ./.trees/feature-auth
```

### Problema 2: "Worktree path already exists"

**Causa**: El directorio ya existe de una creación previa.

**Solución**:
```bash
# Eliminar directorio manualmente
rm -rf .trees/feature-auth

# Limpiar referencias
git worktree prune

# Recrear
git worktree add ./.trees/feature-auth -b feature-auth
```

### Problema 3: "Branch 'feature-auth' already exists"

**Causa**: La rama ya existe de una creación previa.

**Solución**:
```bash
# Opción A: Usar rama existente
git worktree add ./.trees/feature-auth feature-auth

# Opción B: Eliminar rama y recrear
git branch -D feature-auth
git worktree add ./.trees/feature-auth -b feature-auth
```

### Problema 4: Worktree movido manualmente

**Síntoma**: `git worktree list` muestra path incorrecto.

**Solución**:
```bash
# Reparar automáticamente
git worktree repair

# O especificar nuevo path
git worktree repair ./.trees/nueva-ubicacion
```

### Problema 5: Compartir configuración entre worktrees

**Problema**: Cada worktree tiene su propia copia de archivos.

**Solución**: Usar symlinks para archivos de configuración compartidos.

```bash
# Desde el worktree
cd .trees/feature-auth

# Eliminar config local
rm -rf .claude/settings.json .claude/CLAUDE.md

# Crear symlinks al principal
ln -s ../../.claude/settings.json .claude/settings.json
ln -s ../../.claude/CLAUDE.md .claude/CLAUDE.md

# Ahora comparten la misma config
```

### Problema 6: Rendimiento lento con muchos worktrees

**Causa**: Git necesita trackear múltiples directorios de trabajo.

**Solución**:
```bash
# Limpiar worktrees inactivos regularmente
git worktree prune

# Mantener máximo 3-5 worktrees activos simultáneamente
git worktree list | wc -l  # Ver cantidad actual
```

## Mejores Prácticas

### 1. Estructura Consistente

```bash
# ✅ SIEMPRE usar .trees/ como contenedor
.trees/feature-auth/
.trees/feature-dashboard/

# ❌ NO dispersar worktrees
../auth-worktree/
~/worktrees/project-dashboard/
```

### 2. Nombrado Descriptivo

```bash
# ✅ BIEN: Nombres claros y consistentes
.trees/feature-user-authentication
.trees/feature-payment-integration
.trees/hotfix-memory-leak-issue-456

# ❌ MAL: Nombres vagos
.trees/temp
.trees/test
.trees/new-stuff
```

### 3. Limpieza Regular

```bash
# Cada semana o después de mergear features
git worktree prune
git worktree list  # Verificar cuáles siguen activos
```

### 4. Commits Frecuentes en Worktrees

```bash
# Commit frecuentemente para evitar pérdida de trabajo
cd .trees/feature-auth
git add .
git commit -m "wip: progress on auth service"
```

### 5. Un Worktree por Feature

```bash
# ✅ Aislamiento claro
.trees/feature-auth/        → Solo auth
.trees/feature-dashboard/   → Solo dashboard

# ❌ Evitar trabajo cruzado
# NO trabajes en auth desde worktree de dashboard
```

### 6. Usar con Plan Mode de Claude

```bash
# Workflow recomendado:
/worktree nueva-feature    # Crea worktree + activa plan mode
# Claude planifica en contexto aislado
/work                       # Implementa el plan
# Claude implementa sin interferencia
```

### 7. Backup Antes de Remover

```bash
# Si no estás seguro, hacer backup
git worktree list
git log feature-auth  # Verificar commits
git push origin feature-auth  # Backup en remote

# Luego sí remover
git worktree remove ./.trees/feature-auth
```

### 8. Testing en Worktrees

```bash
# Cada worktree puede tener su propio virtualenv/node_modules
cd .trees/feature-auth
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Tests aislados sin afectar main
pytest
```

## Casos de Uso Avanzados

### Caso 1: Desarrollo Paralelo Multi-Feature

```bash
# Tres features en paralelo con 3 Claude Code instances
git worktree add ./.trees/feature-auth -b feature-auth
git worktree add ./.trees/feature-api -b feature-api
git worktree add ./.trees/feature-ui -b feature-ui

# Terminal 1
cd .trees/feature-auth && claude

# Terminal 2
cd .trees/feature-api && claude

# Terminal 3
cd .trees/feature-ui && claude
```

### Caso 2: Testing de PRs sin Interrumpir Desarrollo

```bash
# PR llega mientras trabajas
git worktree add ./.trees/review-pr-789 pr-789

# Revisar PR en worktree separado
cd .trees/review-pr-789
npm test
# Dejar comentarios...

# Mientras tanto sigues en .trees/feature-current/
# Sin interrupciones
```

### Caso 3: Bisect sin Perder Progreso

```bash
# Necesitas hacer git bisect para encontrar bug
git worktree add ./.trees/bisect-bug main

cd .trees/bisect-bug
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
# ... bisecting ...

# Tu trabajo en .trees/feature-auth/ sigue intacto
```

### Caso 4: Múltiples Versiones en Producción

```bash
# Mantener soporte de múltiples versiones
git worktree add ./.trees/v1.x v1.x-branch
git worktree add ./.trees/v2.x v2.x-branch
git worktree add ./.trees/v3.x main

# Aplicar hotfix a v1.x
cd .trees/v1.x
# Fix + test + deploy

# Sin afectar v2.x o v3.x
```

## Comparación con Alternativas

### vs. Git Stash

| Aspecto | Git Stash | Git Worktrees |
|---------|-----------|---------------|
| **Cambio de contexto** | Lento (stash/pop) | Instantáneo (cd) |
| **Trabajo paralelo** | Imposible | Natural |
| **Contexto AI** | Se pierde | Se preserva |
| **Complejidad** | Baja | Media |

### vs. Git Clone Multiple

| Aspecto | Multiple Clones | Worktrees |
|---------|-----------------|-----------|
| **Espacio disco** | Alto (duplica .git) | Bajo (comparte .git) |
| **Sincronización** | Manual | Automática |
| **Performance** | Lento | Rápido |
| **Mantenimiento** | Difícil | Fácil |

### vs. Git Branches Normales

| Aspecto | Branches | Worktrees |
|---------|----------|-----------|
| **Setup** | Muy simple | Simple |
| **Switching** | git checkout | cd |
| **Simultaneidad** | No | Sí |
| **AI agents** | Confusión | Óptimo |

## Referencias

### Artículos y Guías (2024-2025)

1. **Steve Kinney - Git Worktrees for Parallel AI Development**
   - https://stevekinney.com/courses/ai-development/git-worktrees
   - Casos de uso específicos con AI

2. **Geeky Gadgets - Git Worktrees with Claude Code**
   - https://www.geeky-gadgets.com/how-to-use-git-worktrees-with-claude-code-for-seamless-multitasking/
   - Integración específica con Claude Code

3. **Nick Mitchinson - Multi-Feature Development with AI Agents**
   - https://www.nrmitchi.com/2025/10/using-git-worktrees-for-multi-feature-development-with-ai-agents/
   - Workflows avanzados

4. **Agent Interviews - Parallel AI Coding**
   - https://docs.agentinterviews.com/blog/parallel-ai-coding-with-gitworktrees/
   - Best practices para equipos

5. **DevDynamics - Git Worktree Workflows**
   - https://devdynamics.ai/blog/understanding-git-worktree-to-fast-track-software-development-process/
   - Optimización de procesos

### Documentación Oficial

- **Git Documentation - git-worktree**
  - https://git-scm.com/docs/git-worktree
  - Referencia completa de comandos

### Videos

- **Framework de Claude Code con Worktrees**
  - https://youtu.be/NJ6sO_0BoTA
  - Caso de uso real: News Aggregator + Kanban

---

**Última actualización**: 2025-01-15
**Autor**: Framework de Optimización de Contexto
**Basado en**: https://youtu.be/NJ6sO_0BoTA
