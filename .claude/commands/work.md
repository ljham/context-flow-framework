---
description: Iniciar implementación de feature siguiendo el plan
---

Comenzar fase de implementación:

1. **Leer el archivo de sesión de contexto** para obtener el contexto completo de la feature:
   - Verificar `.claude/sessions/context_session_*.md`
   - Entender el plan general y el estado actual

2. **Leer todos los planes de implementación** creados por los subagentes:
   - Verificar el directorio `.claude/doc/{nombre_feature}/`
   - Revisar todos los archivos de plan creados por subagentes
   - Consolidar el entendimiento de lo que necesita implementarse

3. **Implementar el plan paso a paso:**
   - Seguir el plan consolidado de todos los subagentes
   - Implementar cambios en el código
   - Crear nuevos archivos según especificado en los planes
   - Modificar archivos existentes según sea necesario

4. **Actualizar sesión de contexto con el progreso:**
   - Usar checkboxes para marcar tareas completadas
   - Documentar cualquier desviación del plan
   - Anotar cualquier desafío encontrado

5. **Recordar:**
   - Solo tú implementas código - los subagentes solo planifican
   - Consulta la documentación de los subagentes cuando sea necesario
   - Actualiza el archivo de sesión después de cada hito importante
   - Valida con tests antes de considerar completo

6. **Cuando esté completo:**
   - Ejecutar linters y tests
   - Preparar para git commit
   - Considerar delegar a qa-criteria-validator para features de UI
