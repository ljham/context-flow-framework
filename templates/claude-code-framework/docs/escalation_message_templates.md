# Escalation Message Templates

Templates predefinidos para escalar problemas al usuario de manera clara y accionable.

---

## Template 1: Subagente No Puede Completar Plan

```markdown
❌ **No pude completar la planificación de {componente} después de 3 intentos**

### Intentos Realizados

**Intento 1:** {descripción breve}
- Error: {mensaje de error}
- Acción tomada: {qué se intentó}

**Intento 2:** {descripción breve}
- Error: {mensaje de error}
- Acción tomada: {qué se intentó diferente}

**Intento 3:** {descripción breve}
- Error: {mensaje de error}
- Acción tomada: {investigación manual}

### Causa Raíz Identificada

{Tu mejor hipótesis del problema basada en los 3 intentos}

### Necesito Tu Ayuda Con

1. **{Pregunta específica 1}**
   - Context: {por qué necesitas saber esto}

2. **{Pregunta específica 2}**
   - Context: {por qué necesitas saber esto}

### Archivos de Diagnóstico

- Error log completo: `.claude/sessions/error_log_{feature}.md`
- Context session: `.claude/sessions/context_session_{feature}.md`
- {Otros archivos relevantes}

### Opciones que Veo

**Opción A:** {descripción}
- Pros: {ventajas}
- Cons: {desventajas}

**Opción B:** {descripción}
- Pros: {ventajas}
- Cons: {desventajas}

**¿Cuál prefieres, o tienes otra idea?**
```

---

## Template 2: Tests/Linters Fallando Repetidamente

```markdown
❌ **Los tests/linters siguen fallando después de 3 intentos de corrección**

### Error Persistente

```
{stack trace o error message completo}
```

### Archivos Afectados

- `{file1}:{line}` - {descripción del problema}
- `{file2}:{line}` - {descripción del problema}
- `{file3}:{line}` - {descripción del problema}

### Intentos de Corrección

**Intento 1 - Auto-fix:**
- Acción: {qué auto-fixer se corrió}
- Resultado: {qué se corrigió}
- Estado: {cuántos tests aún fallan}

**Intento 2 - Root Cause Fix:**
- Root cause identificada: {qué se identificó}
- Fix aplicado: {qué se corrigió específicamente}
- Estado: {cuántos tests aún fallan}

**Intento 3 - Plan Alignment:**
- Plan revisado: {qué plan se re-leyó}
- Gap identificado: {diferencia entre plan e implementación}
- Ajuste realizado: {qué se cambió}
- Estado: {cuántos tests aún fallan}

### Mi Análisis

{Tu hipótesis de por qué el error persiste después de 3 intentos}

Posibles causas:
1. {hipótesis 1 con evidencia}
2. {hipótesis 2 con evidencia}

### Necesito Tu Input En

1. **¿El enfoque arquitectónico es correcto?**
   - Actualmente usando: {enfoque actual}
   - Alternative approaches: {alternativas que ves}

2. **¿Hay algún requisito que no entendí?**
   - Mi entendimiento: {qué crees que se requiere}
   - Posible gap: {qué podría estar faltando}

3. **¿Prefieres una solución alternativa?**
   - Opción A: {descripción}
   - Opción B: {descripción}

### Archivos de Referencia

- Implementation: {archivos modificados}
- Plans: `.claude/doc/{feature}/`
- Error log: `.claude/sessions/error_log_{feature}.md`
```

---

## Template 3: QA Validation Fallando

```markdown
❌ **La implementación no pasa validación QA después de 3 intentos**

### Criterios que Aún Fallan

- [ ] **{Criterio 1}:** FAIL
  - Expected: {qué debería pasar}
  - Actual: {qué está pasando}
  - Razón: {por qué falla}

- [ ] **{Criterio 2}:** FAIL
  - Expected: {qué debería pasar}
  - Actual: {qué está pasando}
  - Razón: {por qué falla}

### Intentos de Corrección

**Intento 1 - QA Report Fixes:**
- Fixes implementados: {lista de qué se arregló}
- Issues resolved: {cuántos}
- Issues remaining: {cuántos}

**Intento 2 - Acceptance Criteria Review:**
- Criterios revisados en: `.claude/sessions/discovery_{feature}.md`
- Gaps identificados: {qué funcionalidad faltaba}
- Implementación agregada: {qué se añadió}
- Issues remaining: {cuántos}

**Intento 3 - Deep Analysis:**
- Análisis realizado: {qué se investigó}
- Problema identificado: {qué se encontró}
- Limitaciones: {por qué no se pudo resolver automáticamente}

### Hipótesis de Causa Raíz

**Hipótesis 1:** {explicación con evidencia}

**Hipótesis 2:** {explicación con evidencia}

### Opciones que Veo

**Opción A:** {solución 1}
- Qué implicaría: {explicación}
- Pros: {ventajas}
- Cons: {desventajas}
- Effort: {estimado de esfuerzo}

**Opción B:** {solución 2}
- Qué implicaría: {explicación}
- Pros: {ventajas}
- Cons: {desventajas}
- Effort: {estimado de esfuerzo}

**¿Cuál prefieres, o tienes otra solución en mente?**

### Archivos de Referencia

- QA Report: `.claude/doc/{feature}/qa_report.md`
- Discovery: `.claude/sessions/discovery_{feature}.md`
- Implementation: {archivos clave}
```

---

## Template 4: Información Insuficiente en Discovery

```markdown
⚠️ **Documento de discovery parcial creado - Información insuficiente para proceder**

### Lo Que Tengo Claro

✅ {aspecto 1 que está claro}
✅ {aspecto 2 que está claro}
✅ {aspecto 3 que está claro}

### Lo Que Necesito Clarificar

❌ **Blocker 1:** {descripción del gap de información}
- Pregunta(s) intentada(s): {qué pregunté}
- Respuesta recibida: {qué respondiste}
- Clarificación necesaria: {qué necesito saber exactamente}
- Impact si no se clarifica: {por qué es importante}

❌ **Blocker 2:** {similar}

❌ **Blocker 3:** {similar}

### Rondas de Preguntas Realizadas

**Ronda 1 (5W1H básico):** {resumen de qué se preguntó}
**Ronda 2 (Preguntas específicas):** {resumen con ejemplos concretos}
**Ronda 3 (Escenarios propuestos):** {escenarios A, B, C propuestos}

Resultado: Información aún insuficiente para proceder a planificación técnica.

### Opciones

**Opción A: Responder Preguntas Específicas**
{Lista de preguntas puntuales que resolverían los blockers}

**Opción B: Proceder con Assumptions (Riesgoso)**
Si procedemos con estos assumptions:
- Assumption 1: {qué asumiríamos}
- Assumption 2: {qué asumiríamos}
- **Riesgo:** {qué puede salir mal}

**Opción C: Cambiar el Scope**
{Sugerir un scope más limitado y claro}

**¿Qué prefieres?**

### Archivo Creado

- Discovery parcial: `.claude/sessions/discovery_{feature}.md` [PARTIAL]
- Ver sección "What We DON'T Know" para details
```

---

## Template 5: Timeout/Performance Issue

```markdown
⏱️ **Operación excedió timeout después de 2 intentos**

### Operación Afectada

- **Component:** {qué componente/agente}
- **Action:** {qué estaba haciendo}
- **Expected duration:** {tiempo esperado}
- **Actual duration:** {tiempo real}
- **Timeout threshold:** {límite configurado}

### Intentos Realizados

**Intento 1:**
- Started: {timestamp}
- Ended: {timestamp} (TIMEOUT)
- Duration: {minutos}
- Progress made: {qué se logró hacer}

**Intento 2:**
- Started: {timestamp}
- Ended: {timestamp} (TIMEOUT)
- Duration: {minutos}
- Progress made: {qué se logró hacer}
- Optimization attempted: {qué se intentó mejorar}

### Posibles Causas

1. **{Causa 1}:** {explicación}
   - Evidence: {qué sugiere esto}

2. **{Causa 2}:** {explicación}
   - Evidence: {qué sugiere esto}

### Opciones

**Opción A: Aumentar Timeout**
- New timeout: {valor propuesto}
- Trade-off: Más tiempo de espera pero puede completarse
- Recommendation: [recommended | not_recommended]

**Opción B: Simplificar Tarea**
- Reduced scope: {qué se quitaría}
- Trade-off: Menos completo pero completable
- Recommendation: [recommended | not_recommended]

**Opción C: Dividir en Sub-Tareas**
- Sub-task 1: {descripción}
- Sub-task 2: {descripción}
- Trade-off: Más iteraciones pero progreso incremental
- Recommendation: [recommended | not_recommended]

**¿Qué enfoque prefieres?**
```

---

## Template 6: Dependency/External Service Failure

```markdown
🔗 **Fallo de dependencia externa después de 3 intentos**

### Dependencia Afectada

- **Service:** {qué servicio/API/MCP/library}
- **Operation:** {qué se intentaba hacer}
- **Required for:** {por qué se necesita}

### Error Details

```
{error message completo del servicio externo}
```

### Intentos de Conexión

**Intento 1:**
- Timestamp: {cuando}
- Error: {mensaje}
- Response code: {si aplica}
- Retry after: {si aplica}

**Intento 2:**
- Timestamp: {cuando}
- Error: {mensaje}
- Response code: {si aplica}
- Changes made: {qué se intentó diferente}

**Intento 3:**
- Timestamp: {cuando}
- Error: {mensaje}
- Response code: {si aplica}
- Fallback attempted: {si se intentó alternativa}

### Status Check

- [x] Checked service status page: {resultado}
- [x] Verified credentials/API keys: {resultado}
- [x] Tested with minimal request: {resultado}
- [x] Checked network connectivity: {resultado}

### Impact

**Features Blocked:**
- {feature 1 que depende de esto}
- {feature 2 que depende de esto}

**Workarounds Available:**
- {workaround 1 si existe}
- {workaround 2 si existe}

### Recommended Action

{Tu recomendación basada en el análisis}

**Options:**
A. Wait for service to recover ({timeline estimate})
B. Use workaround ({descripción y trade-offs})
C. Skip this dependency for now ({impacto de hacerlo})

**¿Qué prefieres hacer?**
```

---

**Template Version:** 1.0
**Last Updated:** 2025-12-11

**Usage Notes:**
- Copy template apropiado cuando escales
- Reemplaza todos los {placeholders} con info específica
- Ajusta formato según necesidad específica
- Siempre incluye archivos de diagnóstico relevantes
- Siempre proporciona opciones concretas al usuario
