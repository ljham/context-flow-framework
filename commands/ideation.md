---
description: Research de producto y mercado para una nueva idea/feature
argument-hint: [descripción de la feature]
---

Basándose en la descripción de la feature proporcionada por el usuario:

<feature_description>
#$ARGUMENTS
</feature_description>

1. Usar el subagente `product-strategist-agent` para:
   - Evaluar la idea
   - Identificar segmentos clave de usuarios
   - Definir una propuesta de valor clara
   - Proponer opciones viables

2. Usar el subagente `research-analyst-agent` para:
   - Buscar productos similares
   - Comparar funcionalidades y posicionamiento
   - Identificar brechas de mercado
   - Analizar panorama competitivo

3. Consolidar los hallazgos y guardar el resultado en `.claude/research/{nombre_investigacion}.md`

4. Proporcionar un resumen de:
   - Oportunidad de mercado
   - Competidores clave
   - Enfoque recomendado
   - Próximos pasos
