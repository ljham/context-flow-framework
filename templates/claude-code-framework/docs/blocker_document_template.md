# Blocker Document: {Feature Name}

**Created:** {fecha}
**Blocker Status:** [active | resolved | workaround_found]
**Severity:** [critical | high | medium | low]
**Phase Blocked:** [discovery | planning | implementation | validation]

---

## Executive Summary

**In One Sentence:**
{Descripción de una línea del blocker}

**Impact:**
{Qué no se puede hacer mientras este blocker existe}

**Estimated Resolution Time:**
{Si se tiene info del usuario: cuánto tomará resolver}

---

## Blocker Details

### What I Was Trying to Do
{Descripción detallada de la tarea que estaba en progreso}

### What Happened Instead
{Qué error o problema ocurrió}

### Attempts Made (Before Escalating)

#### Attempt 1/3
- **Action:** {qué intenté}
- **Result:** {qué pasó}
- **Error:** `{mensaje de error si hay}`

#### Attempt 2/3
- **Action:** {qué intenté diferente}
- **Result:** {qué pasó}
- **Error:** `{mensaje de error si hay}`

#### Attempt 3/3
- **Action:** {último intento o investigación}
- **Result:** {qué pasó}
- **Error:** `{mensaje de error si hay}`

### Root Cause Analysis

**My Hypothesis:**
{Tu mejor entendimiento de por qué está pasando esto}

**Evidence:**
- {evidencia 1 que apoya tu hipótesis}
- {evidencia 2}
- {evidencia 3}

**What's Missing:**
{Qué información o clarificación necesitas para proceder}

---

## Impact Assessment

### Immediate Impact
- **Can Continue?** [yes_with_workaround | yes_partial | no]
- **Features Blocked:** {lista de features que no se pueden avanzar}
- **People Affected:** {quién más está bloqueado por esto}

### Business Impact
- **Priority:** [p0_critical | p1_high | p2_medium | p3_low]
- **User Impact:** {cómo afecta a usuarios finales}
- **Timeline Impact:** {cuánto atrasa el proyecto}

---

## Questions for User

### Critical Questions (Must Answer)
1. **{Pregunta crítica 1}**
   - Why it matters: {por qué esta pregunta es importante}
   - Options I see: {opciones A, B, C si tienes hipótesis}

2. **{Pregunta crítica 2}**
   - Why it matters: {por qué esta pregunta es importante}
   - Options I see: {opciones}

### Nice-to-Know Questions (Optional)
3. **{Pregunta opcional 1}**
   - Why it helps: {cómo ayudaría tener esta info}

---

## Workarounds Considered

### Workaround 1: {nombre del workaround}
**Description:** {qué implica este workaround}
**Pros:**
- {ventaja 1}
- {ventaja 2}

**Cons:**
- {desventaja 1}
- {desventaja 2}

**Recommendation:** [recommended | not_recommended | last_resort]
**Why:** {justificación de la recomendación}

### Workaround 2: {nombre del workaround}
{similar}

### Workaround 3: Wait for User Input
**Description:** Pausar trabajo en esta feature hasta recibir clarificación
**Pros:**
- Evita hacer assumptions incorrectos
- Ahorra tiempo y tokens

**Cons:**
- Bloquea progreso
- Puede atrasar timeline

**Recommendation:** [recommended si no hay workarounds viables]

---

## Proposed Next Steps

### If User Chooses Option A
1. {paso 1}
2. {paso 2}
3. {paso 3}
**Estimated effort:** {tiempo/complejidad}

### If User Chooses Option B
1. {paso 1}
2. {paso 2}
3. {paso 3}
**Estimated effort:** {tiempo/complejidad}

### If User Provides Alternative
{Dejar espacio para documentar decisión alternativa}

---

## Resolution

### User Decision
**Decided At:** {timestamp}
**Decision:** {qué decidió el usuario}
**Rationale:** {por qué se tomó esa decisión}

### Implementation
- [x] {paso 1 implementado}
- [x] {paso 2 implementado}
- [ ] {paso 3 pendiente}

### Verification
- [ ] Blocker resolved
- [ ] Feature can continue
- [ ] Documentation updated
- [ ] Similar situations prevented (hook/guide added)

---

## Post-Mortem

### What Went Wrong
{Análisis honesto de qué causó el blocker}

### What Went Right
{Qué se hizo bien en la detección y escalación}

### How to Prevent in Future
1. **Prevention Step 1:** {qué hacer diferente la próxima vez}
2. **Prevention Step 2:** {qué verificar antes}
3. **Prevention Step 3:** {qué documentación añadir}

### Documentation Updates Needed
- [ ] Update CLAUDE.md with new check
- [ ] Add to troubleshooting guide
- [ ] Create preventive hook
- [ ] Update agent prompt
- [ ] Add to discovery template

---

## References

### Related Files
- Context session: `.claude/sessions/context_session_{feature}.md`
- Error log: `.claude/sessions/error_log_{feature}.md`
- Discovery doc: `.claude/sessions/discovery_{feature}.md`
- Plans: `.claude/doc/{feature}/`

### External Resources
- {link a documentación relevante}
- {link a issue similar en GitHub}
- {link a discusión en Slack/Discord}

---

**Template Version:** 1.0
**Last Updated:** 2025-12-11
