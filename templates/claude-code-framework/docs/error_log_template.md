# Error Log: {Feature Name}

**Created:** {fecha}
**Status:** [in_progress | blocked | resolved]

---

## Summary

- **Feature:** {nombre completo de la feature}
- **Current Phase:** [discovery | planning | implementation | validation]
- **Total Failed Attempts:** {número total de intentos}
- **Escalated to User:** [yes | no]
- **Resolution Status:** [pending | in_progress | resolved]

---

## Attempt Log

### [{timestamp}] Attempt 1/3
**Component:** {nombre del agente o componente que falló}
**Action:** {qué se intentaba hacer}
**Result:** FAILED
**Error:** `{mensaje de error exacto}`
**Root Cause:** {hipótesis de la causa raíz}
**Fix Applied:** {qué se intentó para resolver}
**Next Step:** {qué se hará en el siguiente intento}

### [{timestamp}] Attempt 2/3
**Component:** {nombre del agente o componente}
**Action:** {qué se intentaba hacer en este retry}
**Result:** FAILED
**Error:** `{mensaje de error exacto}`
**Root Cause:** {hipótesis actualizada de la causa raíz}
**Fix Applied:** {qué se intentó diferente esta vez}
**Next Step:** {qué se hará en el siguiente intento}

### [{timestamp}] Attempt 3/3
**Component:** {nombre del agente o componente}
**Action:** {último intento manual o investigación}
**Result:** FAILED
**Error:** `{mensaje de error exacto}`
**Root Cause:** {causa raíz confirmada}
**Fix Applied:** {qué se intentó en el último intento}
**Next Step:** ESCALATE - {razón de por qué se debe escalar}

---

## Escalation Record

**Escalated At:** {timestamp}
**Escalated To:** User
**Escalation Reason:** {explicación detallada del blocker}

### Information Requested from User
1. {pregunta específica 1}
2. {pregunta específica 2}
3. {pregunta específica 3}

### Proposed Options
**Option A:** {descripción de opción 1}
- Pros: {ventajas}
- Cons: {desventajas}

**Option B:** {descripción de opción 2}
- Pros: {ventajas}
- Cons: {desventajas}

**Option C:** {descripción de opción 3 si aplica}
- Pros: {ventajas}
- Cons: {desventajas}

### User Response
**Received At:** {timestamp}
**Decision:** {qué decidió el usuario}
**Additional Context:** {contexto adicional proporcionado}

### Resolution Actions
- [ ] {acción 1 para resolver basada en respuesta del usuario}
- [ ] {acción 2}
- [ ] {acción 3}

---

## Resolution

**Resolved At:** {timestamp}
**How It Was Resolved:** {explicación de la solución final}
**Lessons Learned:** {qué aprendimos para evitar este error en el futuro}
**Documentation Updated:** {qué archivos se actualizaron con la solución}

---

## Metrics

- **Time to Escalate:** {minutos desde primer error hasta escalación}
- **Time to Resolve:** {minutos desde escalación hasta resolución}
- **Total Time Lost:** {minutos totales desde primer error hasta resolución}
- **Tokens Consumed:** {estimado de tokens usados en los 3 intentos}
- **Cost Impact:** ${estimado en USD}

---

## Prevention Strategy

### How to Avoid This Error in Future

1. **Check 1:** {qué verificar antes de proceder}
2. **Check 2:** {qué verificar antes de proceder}
3. **Check 3:** {qué verificar antes de proceder}

### Documentation Needed

- [ ] Actualizar README con esta situación
- [ ] Añadir a troubleshooting guide
- [ ] Crear hook de prevención (si aplica)
- [ ] Actualizar agente prompt (si aplica)

---

**Template Version:** 1.0
**Last Updated:** 2025-12-11
