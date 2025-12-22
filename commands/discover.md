---
description: Descubrir y refinar requisitos antes de planificación técnica
argument-hint: [idea o solicitud inicial]
---

<initial_request>
#$ARGUMENTS
</initial_request>

**Fase 0: Descubrimiento de Requisitos**

Ejecutar los siguientes pasos para transformar la idea en requisitos claros:

0. **Auto-instalación del Framework (si es necesario):**
   - Verificar si existe `.claude/CLAUDE.md` en el proyecto actual
   - Si NO existe:
     ```bash
     mkdir -p .claude
     cp ${CLAUDE_PLUGIN_ROOT}/templates/CLAUDE.template.md .claude/CLAUDE.md
     ```
   - Informar al usuario:
     "✅ Framework context-flow instalado en este proyecto (.claude/CLAUDE.md)"
   - Si SÍ existe, continuar normalmente sin mensajes

1. **Delegar al subagente `requirements-engineer`:**
   - Pasar la solicitud inicial del usuario
   - El subagente usará técnicas estructuradas:
     - 5W1H (Why, What, Who, When, Where, How)
     - Jobs-to-be-Done framework
     - User Stories con criterios de aceptación
     - Scope clarification (in/out)
     - Success metrics

2. **El subagente creará:**
   - Archivo de descubrimiento: `.claude/sessions/discovery_{nombre_feature}.md`
   - Problema statement claro
   - User stories completas
   - Criterios de aceptación medibles
   - Definición de scope

3. **Validación con usuario:**
   - Revisar el documento de descubrimiento
   - Confirmar que refleja correctamente la necesidad
   - Ajustar si es necesario

4. **Checkpoint de transición:**
   Una vez validado el documento de descubrimiento:
   - ✅ Problema claramente articulado
   - ✅ Criterios de aceptación medibles
   - ✅ Scope bien definido
   - ✅ Usuario confirma que está completo

   **Entonces estás listo para:**
   ```bash
   /worktree {nombre-feature}
   ```
   Esto activará la Fase 1: Planificación Técnica

## Cuándo Usar Este Comando

Usa `/discover` cuando:
- La solicitud es vaga o ambigua
- No estás seguro de qué problema se está resolviendo
- Necesitas clarificar requisitos antes de planificar
- Quieres validar que entiendes correctamente la necesidad

## Cuándo Saltar Este Comando

Puedes ir directo a `/worktree` si:
- Los requisitos ya están completamente claros
- Ya tienes user stories y criterios de aceptación definidos
- Es una feature muy simple y obvia

## Flujo Completo

```
/discover "necesito manejar usuarios"
    ↓
[requirements-engineer hace preguntas]
    ↓
discovery_{feature}.md creado
    ↓
Usuario valida ✓
    ↓
/worktree {feature}
    ↓
Plan Mode (Fase 1)
```

## Ejemplo de Uso

```bash
# Solicitud vaga
/discover "quiero un dashboard"

# El requirements-engineer preguntará:
# - ¿Qué métricas necesitas ver?
# - ¿Quién usará este dashboard?
# - ¿Actualización en tiempo real o estática?
# - ¿Exportable? ¿Filtrable?
# etc.

# Resultado:
# .claude/sessions/discovery_dashboard.md con requisitos claros

# Entonces:
/worktree dashboard-analytics
```

## Beneficios

1. **Reduce retrabajo** - Implementar la feature correcta desde el inicio
2. **Criterios claros** - QA validation tiene objetivos medibles
3. **Mejor estimación** - Planificación técnica más precisa
4. **Documentación rica** - Historia completa del "por qué"
5. **Alineación** - Usuario y equipo en la misma página
