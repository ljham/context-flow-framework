---
description: Crear Git worktree para feature aislada y activar plan mode
argument-hint: [nombre-feature]
---

<feature_plan>
Nombre de la feature: #$ARGUMENTS
</feature_plan>

Ejecutar los siguientes pasos:

1. Crear git worktree:
   ```bash
   git worktree add ./.trees/feature-$ARGUMENTS -b feature-$ARGUMENTS
   ```

2. Cambiar al directorio del worktree:
   ```bash
   cd .trees/feature-$ARGUMENTS
   ```

3. Activar plan mode (se hará automáticamente cuando el usuario ejecute este comando)

4. Crear archivo de sesión de contexto:
   `.claude/sessions/context_session_$ARGUMENTS.md`

5. Determinar qué agentes deberían usarse y si pueden paralelizarse basándose en los requisitos de la feature

6. Listo para comenzar a planificar la implementación de la feature
