# Estrategia de Manejo de Fallos: Guía Completa

**Versión:** 1.2
**Última Actualización:** 2025-12-11

---

## Introducción

Este documento explica la **Estrategia de Manejo de Fallos** implementada en el Framework de Optimización de Contexto v1.2.

### El Problema Que Resuelve

**Antes de v1.2:**
```
Feature entra en error → Retry infinito → 1M tokens → $3,000 USD → Usuario abandona
```

**Con v1.2:**
```
Feature entra en error → 3 intentos → Escalación inteligente → $5 USD → Usuario confía
```

### Filosofía: Regla de los 3 Intentos

> **"Intenta resolver automáticamente hasta 3 veces. Si fallas 3 veces, escala al usuario con diagnóstico completo."**

**NUNCA hagas reintentos infinitos. SIEMPRE hay un límite.**

---

## ¿Cuándo Se Aplica?

Esta estrategia se aplica en **TODAS** las fases del workflow:

| Fase | Componente | Tipo de Fallo |
|------|-----------|---------------|
| **Fase 0** | requirements-engineer | Usuario no proporciona info clara |
| **Fase 1** | Subagentes planificadores | No pueden crear plan completo |
| **Fase 2** | Agente principal | Tests/linters fallan repetidamente |
| **Fase 3** | qa-criteria-validator | Validación falla con issues críticos |

---

## Anatomía de los 3 Intentos

### Intento 1: Retry Directo
**Objetivo:** Resolver errores transitorios

```markdown
Verificaciones:
- ¿El archivo existe?
- ¿Tengo permisos de acceso?
- ¿Fue un timeout aleatorio?

Acción:
- Re-ejecutar exactamente igual
- Sin cambios de parámetros

Documentar:
- Qué se intentó
- Qué error ocurrió
```

### Intento 2: Retry con Ajustes
**Objetivo:** Resolver con más contexto o parámetros diferentes

```markdown
Verificaciones:
- ¿Qué puedo hacer diferente?
- ¿Necesito más contexto?
- ¿Hay ejemplos previos que ayuden?

Acción:
- Ajustar parámetros
- Añadir contexto adicional
- Probar enfoque alternativo

Documentar:
- Qué cambió respecto a intento 1
- Por qué se hizo ese cambio
- Resultado
```

### Intento 3: Investigación Profunda
**Objetivo:** Entender el problema real antes de escalar

```markdown
Verificaciones:
- ¿Cuál es la VERDADERA causa raíz?
- ¿Es problema técnico o de requisitos?
- ¿Hay alguna solución parcial?

Acción:
- Investigación manual
- Análisis de root cause
- Crear solución parcial si es posible

Documentar:
- Root cause identificado
- Por qué no se pudo resolver automáticamente
- Qué información se necesita del usuario
```

---

## Ejemplos Prácticos por Fase

### Ejemplo 1: Subagente No Puede Crear Plan

**Escenario:** backend-developer falla al crear plan

```markdown
🔴 INTENTO 1/3 (Retry Directo)

ERROR: FileNotFoundError: context_session_user-auth.md

CAUSA: Archivo de contexto no existe todavía

ACCIÓN:
1. Verificar que la ruta es correcta
2. Crear context_session_user-auth.md con contexto inicial
3. Retry backend-developer

RESULTADO: ✅ SUCCESS - Plan creado

DOCUMENTACIÓN:
### Error Log - backend-developer
**[2025-12-11 14:30] Attempt 1/3**
- Error: context_session file not found
- Fix: Created file with initial context
- Status: RESOLVED
```

**Si el intento 1 hubiera fallado:**

```markdown
🟡 INTENTO 2/3 (Retry con Contexto)

ERROR: Timeout después de 120s

CAUSA: Requisitos muy complejos, contexto insuficiente

ACCIÓN:
1. Leer discovery_user-auth.md para más detalles
2. Añadir ejemplos de planes similares previos
3. Pasar más contexto explícito al subagente
4. Retry backend-developer

RESULTADO: Depende...
```

**Si el intento 2 también fallara:**

```markdown
🟠 INTENTO 3/3 (Investigación Manual)

ERROR: Plan incompleto - falta definición de database schema

CAUSA RAÍZ: Discovery document no capturó requisitos de DB

ACCIÓN:
1. Agente principal lee todos los archivos disponibles
2. Intenta crear plan básico con lo que SÍ está claro
3. Documenta específicamente qué falta
4. ESCALA AL USUARIO

MENSAJE DE ESCALACIÓN:
❌ No pude completar la planificación de backend después de 3 intentos.

**Intentos realizados:**
1. File not found → Fixed: Created context file
2. Timeout → Fixed: Added more context
3. Incomplete requirements → BLOCKER

**Causa raíz:**
El discovery document no especifica el schema de database necesario.

**Necesito tu ayuda con:**
- ¿Qué campos necesita la tabla `users`?
- ¿Hay relaciones con otras tablas?
- ¿Qué índices son necesarios?

**Plan parcial creado:**
Ver `.claude/doc/user-auth/backend_plan.md` (endpoints definidos, DB schema pendiente)

**Archivos:**
- Error log: `.claude/sessions/error_log_user-auth.md`
```

---

### Ejemplo 2: Tests Fallando en Implementación

**Escenario:** Tests fallan después de implementar feature

```markdown
🔴 INTENTO 1/3 (Auto-Fix Común)

ERROR:
```
test_user_login FAILED
ImportError: cannot import name 'jwt' from 'utils'
```

ACCIÓN:
1. Ejecutar `ruff --fix`
2. Añadir import faltante: `from utils.auth import jwt`
3. Re-run tests

RESULTADO: ✅ SUCCESS - Tests pasan

DOCUMENTACIÓN:
No es necesario escalar, se resolvió con auto-fix.
```

**Si el intento 1 fallara:**

```markdown
🟡 INTENTO 2/3 (Root Cause Analysis)

ERROR:
```
test_user_login FAILED
AssertionError: Expected 200, got 401
```

ANÁLISIS:
- Test espera token JWT válido
- Token está siendo rechazado por signature inválida
- Root cause: JWT_SECRET no está configurado en test environment

ACCIÓN:
1. Añadir `JWT_SECRET="test-secret"` a conftest.py
2. Re-run tests

RESULTADO: Depende...
```

**Si el intento 2 también fallara:**

```markdown
🟠 INTENTO 3/3 (Plan Alignment)

ERROR:
```
test_user_login FAILED (aún)
AssertionError: Expected user_id in response, got None
```

ANÁLISIS PROFUNDO:
1. Re-leer backend_plan.md
2. Plan especifica: "response debe incluir user_id"
3. Implementación actual: solo devuelve token
4. GAP IDENTIFICADO: Falta user_id en response

ACCIÓN:
1. Actualizar response model para incluir user_id
2. Re-run tests

RESULTADO: Si PASS → ✅ Done
          Si FAIL → Escalar al usuario

ESCALACIÓN (si falla):
❌ Los tests siguen fallando después de 3 intentos de corrección.

**Error persistente:**
```
AssertionError: Expected user_id in response, got None
```

**Archivos afectados:**
- src/auth/routes.py:45 - login endpoint
- tests/test_auth.py:30 - test_user_login

**Intentos:**
1. Auto-fix: Fixed imports
2. Root cause: Added JWT_SECRET config
3. Plan alignment: Added user_id to response (STILL FAILING)

**Mi análisis:**
El user_id se está pasando correctamente al token, pero no se incluye en el response JSON.
Posiblemente el serializer está filtrando el campo.

**Necesito tu input:**
- ¿El enfoque de incluir user_id en response es correcto?
- ¿O prefieres que el cliente lo extraiga del JWT?
- ¿Hay concerns de seguridad que deba considerar?
```

---

### Ejemplo 3: QA Validation Fallando

**Escenario:** Criterios de aceptación no se cumplen

```markdown
🔴 INTENTO 1/3 (Implementar Fixes de QA Report)

QA REPORT ISSUES:
- CRITICAL: Login form no valida email format
- HIGH: Password no muestra requisitos
- MEDIUM: Loading spinner falta en submit

ACCIÓN:
1. Añadir validación de email con regex
2. Añadir tooltip con requisitos de password
3. Añadir loading spinner
4. Re-ejecutar qa-criteria-validator

RESULTADO: Depende de QA re-validation...
```

**Si intento 1 falla (aún hay issues):**

```markdown
🟡 INTENTO 2/3 (Revisar Criterios Originales)

QA RE-VALIDATION:
- ✅ CRITICAL resolved: Email validation working
- ✅ HIGH resolved: Password requirements shown
- ✅ MEDIUM resolved: Loading spinner added
- ❌ NEW CRITICAL: Form submits twice on Enter key

ANÁLISIS:
- Re-leer discovery_user-auth.md
- Criterio original: "Form debe prevenir double-submit"
- Implementación: No tiene protección contra double-submit

ACCIÓN:
1. Añadir disabled state mientras loading
2. Prevenir submit durante loading
3. Re-ejecutar QA

RESULTADO: Depende...
```

**Si intento 2 también falla:**

```markdown
🟠 INTENTO 3/3 (Análisis Profundo)

QA STILL FAILING:
- ❌ Form aún permite double-submit en casos edge

INVESTIGACIÓN:
- Disabled state funciona en click
- Pero Enter key bypasses disabled
- Problema de diseño: disabled no previene keyboard submit

ROOT CAUSE:
Architectural issue - necesita event.preventDefault() además de disabled

OPCIONES:
A. Añadir preventDefault en onKeyDown
B. Usar debounce de 500ms
C. Redesign con form library (Formik, React Hook Form)

ESCALACIÓN:
❌ La implementación no pasa validación QA después de 3 intentos.

**Criterios que fallan:**
- [ ] Prevenir double-submit: FAIL
  - Funciona con mouse click
  - Falla con Enter key
  - Root cause: disabled no previene keyboard events

**Hipótesis:**
1. Necesitamos preventDefault en onKeyDown además de disabled
2. O usar debounce para manejar casos edge
3. O usar form library robusta que maneje esto nativamente

**Opciones:**
A. Add preventDefault (quick fix, 15min)
   - Pros: Rápido, soluciona el issue
   - Cons: Más boilerplate, fácil de olvidar en futuros forms

B. Use debounce (medium effort, 30min)
   - Pros: Funciona para click y keyboard
   - Cons: UX delay de 500ms

C. Migrate to React Hook Form (2-3 hours)
   - Pros: Solución robusta, mejor UX general
   - Cons: Requiere refactor de varios components

**¿Cuál prefieres?**
```

---

## Tracking y Documentación

### Archivos Generados

Cada error genera documentación estructurada:

```
.claude/sessions/
├── context_session_{feature}.md      # Incluye Error Log inline
├── error_log_{feature}.md            # Log detallado (opcional, si muy complejo)
└── blocker_{feature}.md              # Si hay blocker real

.claude/doc/{feature}/
└── {component}_plan.md               # Plan puede ser parcial si hubo blocker
```

### Formato de Error Log en Context Session

```markdown
# Context Session: User Authentication

## Status
- Phase: Planning
- Status: BLOCKED
- Blocker: Database schema requirements unclear

## Error Log

### [2025-12-11 14:30:00] backend-developer - Attempt 1/3
- **Action:** Generate backend plan
- **Result:** FAILED
- **Error:** `FileNotFoundError: context_session_user-auth.md`
- **Fix Applied:** Created context_session with initial info
- **Next:** Retry with file in place

### [2025-12-11 14:32:00] backend-developer - Attempt 2/3
- **Action:** Generate backend plan (retry)
- **Result:** FAILED
- **Error:** Timeout after 120s
- **Fix Applied:** Added detailed examples and explicit file paths
- **Next:** Retry with enhanced context

### [2025-12-11 14:35:00] Main Agent Manual - Attempt 3/3
- **Action:** Manual plan creation
- **Result:** PARTIAL SUCCESS
- **Issue:** Incomplete requirements - missing DB schema
- **Plan Created:** Partial (endpoints defined, DB pending)
- **Next:** ESCALATE to user

## Escalation

**Escalated At:** 2025-12-11 14:36:00
**Reason:** Cannot design DB schema without user input on requirements
**Questions for User:**
1. What fields does `users` table need?
2. Any relations to other tables?
3. Required indexes?

**User Response:** [pending]
```

---

## Timeouts por Fase

**Prevención de loops infinitos en el tiempo:**

| Fase | Operación | Timeout | After 2 Timeouts |
|------|-----------|---------|------------------|
| 0 | requirements-engineer | 10 min | Escalate |
| 1 | Subagente planificador | 5 min | Escalate |
| 1 | Consolidación de planes | 2 min | Manual review |
| 2 | Implementación (por archivo) | 3 min | Check complexity |
| 2 | Tests/linters (por run) | 2 min | Investigate |
| 3 | QA validation | 5 min | Escalate |

**Regla:** Si una operación excede su timeout **2 veces consecutivas**, escala automáticamente.

---

## Anti-Patterns a Evitar

### ❌ Loop Infinito Sin Límite

```python
# MAL - Puede correr por siempre
while tests_failing:
    fix_errors()
    run_tests()
```

### ✅ Loop Limitado

```python
# BIEN - Máximo 3 intentos
max_attempts = 3
for attempt in range(1, max_attempts + 1):
    fix_errors()
    if run_tests():
        break
    if attempt == max_attempts:
        escalate_to_user(
            error=get_last_error(),
            attempts=get_attempt_history()
        )
```

### ❌ Escalar Sin Diagnóstico

```
# MAL
"❌ Falló. No pude hacerlo."
```

### ✅ Escalar Con Contexto Completo

```
# BIEN
"❌ Tests fallando después de 3 intentos.

**Error persistente:**
{stack trace}

**Intentos:**
1. Auto-fix imports
2. Root cause: JWT config
3. Plan alignment: response model

**Mi análisis:**
{hipótesis con evidencia}

**Necesito:**
{preguntas específicas}

**Opciones:**
A. {opción 1 con pros/cons}
B. {opción 2 con pros/cons}
"
```

---

## Templates Disponibles

### 1. Error Log Template
**Archivo:** `templates/claude-code-framework/docs/error_log_template.md`

**Usar cuando:** Necesitas tracking detallado de múltiples intentos en situación compleja.

### 2. Blocker Document Template
**Archivo:** `templates/claude-code-framework/docs/blocker_document_template.md`

**Usar cuando:** Hay un blocker real que requiere decisión o información del usuario.

### 3. Escalation Message Templates
**Archivo:** `templates/claude-code-framework/docs/escalation_message_templates.md`

**Usar cuando:** Necesitas escalar al usuario, copia el template apropiado.

---

## Métricas de Éxito

**Objetivos con esta estrategia:**

| Métrica | Sin Estrategia | Con Estrategia | Mejora |
|---------|----------------|----------------|--------|
| **Loops infinitos** | Común | 0% | 100% |
| **Cost overruns** | Frecuente ($100+) | Raro ($5-10) | 90%+ |
| **User frustration** | Alto | Bajo | 80%+ |
| **Time to resolution** | Variable (horas) | Predecible (minutos) | 70%+ |
| **Quality of escalation** | Vaga | Accionable | 100% |

---

## FAQ

### ¿Qué pasa si el problema se resuelve en intento 1?
✅ Perfecto, documenta brevemente y continúa. No es necesario hacer 3 intentos si el primero funciona.

### ¿Puedo hacer más de 3 intentos?
❌ NO. Escala después de 3. Si el usuario da nueva información, puedes empezar un NUEVO ciclo de 3 intentos.

### ¿Qué si necesito 4 intentos porque casi funciona en el 3ro?
❌ NO. Escala. "Casi funciona" después de 3 intentos significa que hay un problema más profundo que requiere input del usuario.

### ¿Debo documentar TODOS los intentos?
✅ SÍ, al menos brevemente. Ayuda al usuario y a futuro debugging.

### ¿Qué si el usuario no responde la escalación?
⏸️ Pausa el trabajo en esa feature. Documenta el blocker y mueve a otra tarea.

---

## Próximos Pasos

Con la estrategia implementada, ahora puedes:

1. ✅ Ejecutar features sin riesgo de loops infinitos
2. ✅ Escalar de manera profesional y accionable
3. ✅ Mantener costs predecibles
4. ✅ Generar documentación rica de errores

**Para implementar en tu proyecto:**

1. Asegúrate de tener Framework v1.2+
2. Lee `CLAUDE.md` - sección "ESTRATEGIA DE MANEJO DE FALLOS"
3. Revisa los prompts actualizados de los agentes
4. Usa los templates cuando necesites escalar
5. Monitorea las métricas de reintentos

---

**Versión del Documento:** 1.0
**Framework Version:** 1.2
**Última Actualización:** 2025-12-11
