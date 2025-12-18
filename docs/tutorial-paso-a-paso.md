# Tutorial Paso a Paso: Framework de Claude Code

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Requisitos Previos](#requisitos-previos)
3. [Setup Inicial](#setup-inicial)
4. [Tu Primer Workflow Completo](#tu-primer-workflow-completo)
5. [Comandos Slash en Acción](#comandos-slash-en-acción)
6. [Trabajo con Subagentes](#trabajo-con-subagentes)
7. [Git Worktrees en Práctica](#git-worktrees-en-práctica)
8. [Optimización de Contexto](#optimización-de-contexto)
9. [Troubleshooting](#troubleshooting)
10. [Próximos Pasos](#próximos-pasos)

## Introducción

Este tutorial te guiará paso a paso para implementar el **Framework de Optimización de Contexto** para Claude Code en tu proyecto. Al final, serás capaz de:

- ✅ Configurar el framework desde cero
- ✅ Usar las 3 fases de workflow (Planificación → Implementación → Validación)
- ✅ Trabajar con subagentes especializados
- ✅ Optimizar el uso de tokens hasta en un 90%
- ✅ Desarrollar múltiples features en paralelo con worktrees

### ¿Qué Construiremos?

Como ejemplo práctico, implementaremos un **Sistema de Autenticación** completo:
- Backend con JWT
- Endpoints REST
- Tests automatizados
- Documentación generada automáticamente

Este es el mismo tipo de proyecto que el framework puede manejar en 1 día vs 3 días sin optimización.

### Tiempo Estimado

- Setup inicial: 15-20 minutos
- Primer workflow completo: 30-45 minutos
- Experimentación adicional: A tu ritmo

## Requisitos Previos

### Software Necesario

1. **Git** (v2.25+)
   ```bash
   git --version
   ```

2. **Claude Code** (última versión)
   ```bash
   # Instalación (si no lo tienes)
   npm install -g @anthropic/claude-code

   # Verificar instalación
   claude --version
   ```

3. **Python** (3.11+) - Para este tutorial
   ```bash
   python --version
   ```

4. **Poetry** - Gestor de paquetes Python
   ```bash
   # Instalación
   curl -sSL https://install.python-poetry.org | python3 -

   # Verificar
   poetry --version
   ```

### Conocimientos Recomendados

- ✅ Básicos de Git (commit, branch, merge)
- ✅ Terminal/Bash básico
- ✅ Conceptos de desarrollo de software
- ⚠️ NO necesitas ser experto en IA o prompts

## Setup Inicial

### Paso 1: Crear Proyecto Nuevo

```bash
# Crear directorio del proyecto
mkdir mi-proyecto-claude
cd mi-proyecto-claude

# Inicializar Git
git init
git branch -M main

# Crear estructura básica
mkdir -p src tests docs

# Inicializar Poetry (Python)
poetry init --no-interaction --name mi-proyecto

# Primer commit
git add .
git commit -m "chore: initial project setup"
```

### Paso 2: Crear Estructura .claude/

```bash
# Crear directorios necesarios
mkdir -p .claude/{agents,commands,sessions,doc,research,hooks}

# Verificar estructura
tree .claude -L 1
# .claude/
# ├── agents/
# ├── commands/
# ├── sessions/
# ├── doc/
# ├── research/
# └── hooks/
```

### Paso 3: Crear CLAUDE.md

Crea el archivo `.claude/CLAUDE.md` con el siguiente contenido:

```markdown
# Mi Proyecto - Configuración de Claude Code

## Descripción del Proyecto

Este proyecto implementa el Framework de Optimización de Contexto para desarrollo eficiente con IA.

## Stack Tecnológico

- **Lenguaje:** Python 3.11+
- **Gestor de Paquetes:** Poetry (NO pip)
- **Framework:** [Tu framework aquí]
- **Control de Versiones:** Git con Worktrees

## REGLAS DE WORKFLOW

### Fase 1: Planificación

**DEBE HACER:**
- Al inicio de una feature, DEBES crear `.claude/sessions/context_session_{nombre_feature}.md`
- DEBES identificar qué subagentes necesitas
- Intenta ejecutar subagentes en paralelo si es posible

**Proceso:**
1. Usuario proporciona solicitud de feature
2. Activar plan mode
3. Crear archivo de sesión de contexto
4. Identificar y delegar a subagentes EN PARALELO
5. Consolidar planes

### Fase 2: Implementación

**DEBE HACER:**
- Antes de trabajar, DEBES leer `.claude/sessions/context_session_{nombre_feature}.md`
- Después de trabajar, DEBES actualizar el archivo de sesión

**Proceso:**
1. Leer sesión de contexto
2. Leer planes de `.claude/doc/{nombre_feature}/`
3. Implementar código
4. Actualizar progreso con checkboxes

### Fase 3: Validación

**Proceso:**
1. Ejecutar tests
2. Corregir errores
3. git commit cuando esté completo

## Subagentes

[Agregaremos subagentes más adelante]

## Reglas Importantes

- Estamos usando **poetry** NO pip
- Siempre leer sesión de contexto antes de trabajar
- Solo el agente principal implementa código
```

### Paso 4: Crear settings.json

Crea `.claude/settings.json`:

```json
{
  "permissions": {
    "defaultMode": "default",
    "allow": [
      "Bash(git *)",
      "Bash(poetry *)",
      "Bash(pytest *)"
    ]
  },
  "model": "claude-sonnet-4-5",
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo '✓ Archivo modificado'"
          }
        ]
      }
    ]
  }
}
```

### Paso 5: Actualizar .gitignore

Crea o actualiza `.gitignore`:

```gitignore
# Claude Code - archivos temporales
.claude/sessions/
.claude/doc/
.claude/research/
.claude/settings.local.json
.claude/CLAUDE.local.md

# Git Worktrees
.trees/

# Python
__pycache__/
*.py[cod]
*$py.class
.venv/
venv/
*.egg-info/

# Poetry
poetry.lock

# IDE
.vscode/
.idea/
```

### Paso 6: Verificar Setup

```bash
# Verificar estructura completa
tree -a -L 2 -I '.git|__pycache__|*.pyc'

# Debería mostrar:
# .
# ├── .claude/
# │   ├── CLAUDE.md
# │   ├── settings.json
# │   ├── agents/
# │   ├── commands/
# │   ├── sessions/
# │   ├── doc/
# │   ├── research/
# │   └── hooks/
# ├── .gitignore
# ├── src/
# ├── tests/
# ├── docs/
# └── pyproject.toml

# Commit el setup
git add .
git commit -m "feat: add Claude Code framework configuration"
```

¡Perfecto! Ya tienes el setup básico completo. 🎉

## Tu Primer Workflow Completo

Ahora vamos a implementar una feature completa usando las 3 fases del framework.

### Feature: Sistema de Autenticación Simple

Implementaremos un módulo de autenticación con:
- Función de login
- Validación de credenciales
- Tests básicos

### Fase 1: Planificación

**Paso 1: Iniciar Claude Code en Plan Mode**

```bash
# Desde la raíz del proyecto
claude --permission-mode plan
```

**Paso 2: Crear Context Session (Manual - primera vez)**

Dentro de Claude Code, pedirás:

```
Usuario: "Necesito implementar un sistema de autenticación simple con login y validación de credenciales. Crea el archivo de sesión de contexto para esta feature."
```

Claude creará automáticamente `.claude/sessions/context_session_auth.md`:

```markdown
## Feature: Sistema de Autenticación Simple
**Estado**: Planificación
**Fecha inicio**: 2025-01-15

### Objetivo
Implementar módulo de autenticación básico con login y validación.

### Componentes Necesarios
- [ ] Función de login
- [ ] Validación de credenciales
- [ ] Hash de contraseñas (bcrypt)
- [ ] Tests unitarios

### Plan General
1. Crear src/auth/service.py con lógica de autenticación
2. Implementar validación segura de passwords
3. Escribir tests en tests/test_auth.py

### Decisiones
- Usar bcrypt para hashing (estándar de industria)
- Validación simple (username + password)
- Sin JWT por ahora (feature futura)
```

**Paso 3: Salir de Plan Mode**

```
Usuario: "El plan se ve bien, procede con la implementación"
```

Claude saldrá automáticamente de plan mode.

### Fase 2: Implementación

**Paso 1: Leer Context Session**

Claude automáticamente leerá `.claude/sessions/context_session_auth.md` antes de empezar.

**Paso 2: Implementar Código**

```
Usuario: "Implementa el sistema de autenticación según el plan"
```

Claude creará los archivos:

**`src/auth/service.py`**:
```python
"""Servicio de autenticación."""
import bcrypt
from typing import Optional


class AuthService:
    """Servicio para manejar autenticación de usuarios."""

    def __init__(self):
        """Inicializa el servicio de autenticación."""
        # En producción, esto vendría de una base de datos
        self.users_db = {
            "admin": self._hash_password("admin123"),
            "user": self._hash_password("user123")
        }

    def _hash_password(self, password: str) -> bytes:
        """
        Hash de password usando bcrypt.

        Args:
            password: Password en texto plano

        Returns:
            Password hasheado
        """
        return bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())

    def login(self, username: str, password: str) -> bool:
        """
        Valida credenciales de usuario.

        Args:
            username: Nombre de usuario
            password: Password en texto plano

        Returns:
            True si las credenciales son válidas, False en caso contrario
        """
        if username not in self.users_db:
            return False

        stored_hash = self.users_db[username]
        return bcrypt.checkpw(password.encode('utf-8'), stored_hash)


def authenticate_user(username: str, password: str) -> Optional[str]:
    """
    Función helper para autenticar un usuario.

    Args:
        username: Nombre de usuario
        password: Password

    Returns:
        Username si la autenticación es exitosa, None si falla
    """
    service = AuthService()
    if service.login(username, password):
        return username
    return None
```

**`src/auth/__init__.py`**:
```python
"""Módulo de autenticación."""
from .service import AuthService, authenticate_user

__all__ = ['AuthService', 'authenticate_user']
```

**`tests/test_auth.py`**:
```python
"""Tests para el módulo de autenticación."""
import pytest
from src.auth import AuthService, authenticate_user


class TestAuthService:
    """Tests para AuthService."""

    def test_login_success(self):
        """Test login exitoso con credenciales válidas."""
        service = AuthService()
        assert service.login("admin", "admin123") is True

    def test_login_failure_wrong_password(self):
        """Test login falla con password incorrecto."""
        service = AuthService()
        assert service.login("admin", "wrongpass") is False

    def test_login_failure_user_not_exists(self):
        """Test login falla con usuario inexistente."""
        service = AuthService()
        assert service.login("noexiste", "anypass") is False


def test_authenticate_user_success():
    """Test función helper authenticate_user con éxito."""
    result = authenticate_user("admin", "admin123")
    assert result == "admin"


def test_authenticate_user_failure():
    """Test función helper authenticate_user falla."""
    result = authenticate_user("admin", "wrongpass")
    assert result is None
```

**Paso 3: Actualizar Context Session**

Claude actualiza automáticamente `.claude/sessions/context_session_auth.md`:

```markdown
## Feature: Sistema de Autenticación Simple
**Estado**: Implementación completa ✓
**Fecha inicio**: 2025-01-15
**Fecha fin**: 2025-01-15

### Objetivo
Implementar módulo de autenticación básico con login y validación.

### Componentes Implementados
- [x] Función de login
- [x] Validación de credenciales
- [x] Hash de contraseñas (bcrypt)
- [x] Tests unitarios

### Archivos Creados
1. `src/auth/service.py` - Lógica de autenticación
2. `src/auth/__init__.py` - Exports del módulo
3. `tests/test_auth.py` - Tests completos

### Decisiones Implementadas
- ✓ Usado bcrypt para hashing
- ✓ Validación simple implementada
- ✓ 5 tests escritos (todos pasan)
```

### Fase 3: Validación

**Paso 1: Instalar Dependencias**

```bash
# Agregar bcrypt y pytest
poetry add bcrypt
poetry add --group dev pytest

# Instalar
poetry install
```

**Paso 2: Ejecutar Tests**

```
Usuario: "Ejecuta los tests para validar la implementación"
```

```bash
# Claude ejecuta:
poetry run pytest tests/test_auth.py -v

# Salida:
# tests/test_auth.py::TestAuthService::test_login_success PASSED
# tests/test_auth.py::TestAuthService::test_login_failure_wrong_password PASSED
# tests/test_auth.py::TestAuthService::test_login_failure_user_not_exists PASSED
# tests/test_auth.py::test_authenticate_user_success PASSED
# tests/test_auth.py::test_authenticate_user_failure PASSED
# ========================= 5 passed in 0.24s =========================
```

**Paso 3: Commit Final**

```
Usuario: "Los tests pasan. Haz el commit final de la feature."
```

```bash
# Claude ejecuta:
git add .
git commit -m "feat: implement authentication system with bcrypt

- Add AuthService with login validation
- Implement password hashing with bcrypt
- Add comprehensive test suite (5 tests)
- All tests passing

🤖 Generated with Claude Code
Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

¡Completaste tu primer workflow completo! 🎉

### Análisis del Resultado

**Archivos generados automáticamente:**
- `src/auth/service.py` (65 líneas)
- `src/auth/__init__.py` (3 líneas)
- `tests/test_auth.py` (45 líneas)
- `.claude/sessions/context_session_auth.md` (documentación)

**Beneficios observados:**
- ✅ Documentación automática (context session)
- ✅ Código testeable desde el inicio
- ✅ Contexto limpio y enfocado
- ✅ Tests pasan al primer intento

## Comandos Slash en Acción

Ahora vamos a hacer el workflow aún más eficiente usando comandos slash.

### Crear Comando /worktree

Ya tienes `.claude/commands/worktree.md` de la configuración inicial. Vamos a usarlo.

### Caso de Uso: Nueva Feature en Worktree Aislado

**Paso 1: Usar el comando**

```bash
# Iniciar Claude Code
claude

# Dentro de Claude:
/worktree api-endpoints
```

Claude automáticamente:
1. ✅ Crea `.trees/feature-api-endpoints/`
2. ✅ Cambia a ese directorio
3. ✅ Activa plan mode
4. ✅ Crea `context_session_api-endpoints.md`

**Paso 2: Planificar la feature**

```
Usuario: "Planifica la implementación de endpoints REST para el sistema de auth: /login, /logout, /me"
```

Claude crea el plan en el context session y sugiere subagentes.

**Paso 3: Implementar**

```
/work
```

Claude lee el plan e implementa los endpoints.

**Paso 4: Volver al main**

```bash
# Desde terminal
cd ../..  # Volver a raíz del proyecto

# Mergear cuando esté listo
git checkout main
git merge feature-api-endpoints

# Limpiar worktree
git worktree remove ./.trees/feature-api-endpoints
```

### Crear Comando /ideation

Ya tienes `.claude/commands/ideation.md`. Úsalo para research de mercado.

**Ejemplo:**

```
/ideation "Sistema de autenticación con OAuth2 y redes sociales"
```

Claude investigará y creará `.claude/research/oauth2_social_auth.md` con:
- Análisis de mercado
- Competidores
- Opciones tecnológicas
- Recomendaciones

## Trabajo con Subagentes

Ahora vamos a crear tu primer subagente especializado.

### Crear Subagente: backend-developer

Crea `.claude/agents/backend-developer.md`:

```markdown
---
name: backend-developer
description: Usa este agente cuando necesites planificar implementación de lógica de negocio del backend. El agente investigará y creará un plan detallado, pero NO implementará código.

Examples:
- <example>
  Context: Usuario necesita API REST
  user: "Necesito implementar endpoints REST para autenticación"
  assistant: "Voy a delegar al backend-developer para que cree un plan detallado"
  <commentary>
  Backend-developer investigará mejores prácticas, creará estructura de endpoints, y documentará el plan.
  </commentary>
  </example>

tools: Read, Grep, Glob, WebFetch, WebSearch
model: inherit
color: blue
---

## Áreas de Experiencia Principal

1. **APIs REST**: Diseño de endpoints, códigos HTTP, versionado
2. **Lógica de Negocio**: Servicios, validación, reglas de negocio
3. **Persistencia**: Modelos, schemas, migraciones
4. **Seguridad**: Autenticación, autorización, sanitización

## Metodología de Implementación

1. **Análisis de Requisitos**
   - Entender objetivo de la feature
   - Identificar entidades y relaciones
   - Definir casos de uso

2. **Diseño de Arquitectura**
   - Estructura de directorios
   - Separación de responsabilidades
   - Patrones a aplicar

3. **Planificación Detallada**
   - Listar archivos a crear
   - Definir interfaces y contratos
   - Especificar dependencias

## Objetivo

Tu objetivo es proponer un plan de implementación detallado para nuestro código base y proyecto actual.
**NUNCA hagas la implementación real, solo propón el plan de implementación**
Guarda el plan de implementación en `.claude/doc/{nombre_feature}/backend_plan.md`

## Flujo de Trabajo Principal

1. **Fase de Investigación**
   - Buscar documentación actualizada si es necesario
   - Revisar implementación actual del proyecto
   - Identificar patrones existentes

2. **Fase de Planificación**
   - Crear estructura del plan
   - Documentar decisiones
   - Listar pasos de implementación

## Formato de Salida

Tu mensaje final DEBE incluir la ruta del archivo del plan de implementación que creaste.
Ejemplo: "He creado un plan en `.claude/doc/auth/backend_plan.md`, por favor léelo antes de proceder"

## Reglas

- NUNCA hagas la implementación real, ni corras build o dev
- Tu objetivo es solo investigar y planificar
- Estamos usando **poetry** NO pip
- Antes de trabajar, DEBES leer `.claude/sessions/context_session_{nombre_feature}.md`
- Antes de terminar, DEBES crear `.claude/doc/{nombre_feature}/backend_plan.md`
- Toma en cuenta la implementación actual del proyecto
```

### Usar el Subagente

```bash
claude --permission-mode plan
```

```
Usuario: "Necesito implementar una API REST completa para el sistema de autenticación con endpoints /register, /login, /refresh, /logout. Delega al backend-developer para que cree el plan."
```

Claude delegará al subagente backend-developer, que:
1. ✅ Lee el context session
2. ✅ Investiga mejores prácticas de APIs REST
3. ✅ Crea plan detallado en `.claude/doc/auth-api/backend_plan.md`
4. ✅ Retorna resumen al agente principal

Luego el agente principal:
1. ✅ Lee el plan del subagente
2. ✅ Implementa el código siguiendo el plan
3. ✅ Actualiza el context session

## Git Worktrees en Práctica

Vamos a desarrollar dos features en paralelo usando worktrees.

### Escenario: Auth API + User Dashboard

**Paso 1: Crear Dos Worktrees**

```bash
# Feature 1: API REST
git worktree add ./.trees/feature-auth-api -b feature-auth-api

# Feature 2: Dashboard de Usuario
git worktree add ./.trees/feature-user-dashboard -b feature-user-dashboard
```

**Paso 2: Trabajar en Paralelo**

**Terminal 1 - API REST:**
```bash
cd .trees/feature-auth-api
claude --permission-mode plan

# Dentro de Claude:
# "Implementa la API REST de autenticación..."
```

**Terminal 2 (simultáneo) - Dashboard:**
```bash
cd .trees/feature-user-dashboard
claude --permission-mode plan

# Dentro de Claude:
# "Implementa el dashboard de usuario..."
```

Ambas instancias de Claude trabajan **sin interferencia** porque:
- ✅ Contextos completamente aislados
- ✅ Archivos independientes
- ✅ Sin conflictos de merge durante desarrollo

**Paso 3: Mergear Cuando Estén Listas**

```bash
# Volver a raíz
cd ../..

# Mergear API (terminó primero)
git checkout main
git merge feature-auth-api
git push

# Mergear Dashboard (terminó después)
git merge feature-user-dashboard
git push

# Limpiar worktrees
git worktree remove ./.trees/feature-auth-api
git worktree remove ./.trees/feature-user-dashboard
```

**Ahorro de tiempo**: 2 features en el tiempo de 1.

## Optimización de Contexto

Veamos cómo el framework optimiza el uso de tokens.

### Experimento: Con vs Sin Framework

**Sin Framework (tradicional):**

```
Usuario: "Implementa sistema de auth"
Claude: "Aquí está el código completo..." [3000 tokens]

Usuario: "Agrega refresh tokens"
Claude: "Aquí está el código actualizado completo..." [3500 tokens]

Usuario: "Agrega rate limiting"
Claude: "Aquí está todo el código de nuevo..." [4000 tokens]

Total contexto: ~50,000 tokens
Tiempo: 3-4 horas
Costo: ~$4.00
```

**Con Framework (optimizado):**

```
Usuario: "Implementa sistema de auth"
Claude:
  1. Crea context_session_auth.md [200 tokens]
  2. Delega a backend-developer
  3. Lee plan [500 tokens]
  4. Implementa código

Usuario: "Agrega refresh tokens"
Claude:
  1. Lee context_session [200 tokens]
  2. Actualiza plan [100 tokens]
  3. Implementa cambios incrementales

Usuario: "Agrega rate limiting"
Claude:
  1. Lee context_session [200 tokens]
  2. Actualiza plan [100 tokens]
  3. Implementa feature adicional

Total contexto: ~5,000 tokens
Tiempo: 1 hora
Costo: ~$0.40
```

**Ahorro: 90% tokens, 75% tiempo, 90% costo** 📊

### Verificar Optimización

Puedes verificar el ahorro real en tu proyecto:

```bash
# Ver tamaño de archivos de contexto
du -h .claude/sessions/
du -h .claude/doc/

# Típicamente:
# sessions/: 10-50 KB (vs 500+ KB en chat history)
# doc/: 20-100 KB (vs código completo en contexto)
```

## Troubleshooting

### Problema 1: Claude no lee el context session

**Síntoma**: Claude actúa como si no supiera del trabajo previo.

**Solución**:
```
Usuario: "Antes de continuar, lee el archivo .claude/sessions/context_session_auth.md y confirma que entiendes el estado actual de la feature."
```

### Problema 2: Subagente intenta implementar código

**Síntoma**: Subagente escribe código en lugar de solo planificar.

**Solución**: Verificar que el archivo del subagente incluye:
```markdown
## Reglas
- NUNCA hagas la implementación real
- Tu objetivo es solo investigar y planificar
```

### Problema 3: Worktree no se puede eliminar

**Síntoma**: `git worktree remove` falla.

**Solución**:
```bash
# Salir del directorio del worktree primero
cd ../..

# Luego remover
git worktree remove ./.trees/feature-nombre

# Si persiste, forzar
git worktree remove --force ./.trees/feature-nombre
```

### Problema 4: Context session desactualizado

**Síntoma**: El context session no refleja el trabajo actual.

**Solución**:
```
Usuario: "Actualiza el context session con el progreso actual: completamos X, Y, Z. Falta implementar W."
```

### Problema 5: Demasiados worktrees activos

**Síntoma**: Confusión sobre qué worktree usar.

**Solución**:
```bash
# Listar todos
git worktree list

# Remover los que no usas
git worktree remove ./.trees/feature-old-1
git worktree remove ./.trees/feature-old-2

# Mantener máximo 3-5 activos
```

## Próximos Pasos

### Nivel Intermedio

1. **Crear más subagentes**:
   - frontend-developer
   - qa-criteria-validator
   - ui-ux-analyzer

2. **Automatizar más con comandos slash**:
   - /test - Ejecutar suite de tests
   - /lint - Ejecutar linters
   - /docs - Generar documentación

3. **Experimentar con hooks**:
   - Pre-commit hooks
   - Post-tool-use hooks para logging

### Nivel Avanzado

1. **Desarrollo paralelo masivo**:
   - 3-4 features simultáneas en worktrees
   - Coordinación entre features

2. **Integración con CI/CD**:
   - Tests automáticos en cada worktree
   - Deployment desde worktrees

3. **Plantillas de proyecto**:
   - Crear template reutilizable
   - Setup automático para nuevos proyectos

### Recursos Adicionales

Continúa aprendiendo con:

- 📚 `docs/framework-claude-code.md` - Documentación completa
- 📊 `docs/optimizacion-contexto-explicado.md` - Métricas y beneficios
- 🔧 `docs/context-engineering-deep-dive.md` - Técnicas avanzadas
- 🌳 `docs/git-worktrees-guide.md` - Guía completa de worktrees
- 🎬 [Video original](https://youtu.be/NJ6sO_0BoTA) - Caso de estudio real

## Conclusión

¡Felicitaciones! 🎉 Has completado el tutorial y ahora sabes:

- ✅ Configurar el framework desde cero
- ✅ Usar las 3 fases de workflow
- ✅ Trabajar con subagentes
- ✅ Optimizar contexto (ahorro de 90%)
- ✅ Desarrollar en paralelo con worktrees
- ✅ Usar comandos slash para eficiencia

### Tu Siguiente Feature

Pon en práctica lo aprendido:

```bash
# Inicia Claude Code
claude

# Usa el framework
/worktree tu-nueva-feature

# ¡Y construye increíble! 🚀
```

---

**Última actualización**: 2025-01-15
**Autor**: Framework de Optimización de Contexto
**Basado en**: https://youtu.be/NJ6sO_0BoTA

**¿Preguntas o problemas?** Revisa la sección de Troubleshooting o consulta la documentación completa en `docs/`.
