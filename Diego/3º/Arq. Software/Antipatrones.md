# Motivación

- Los **patrones** enseñan cómo construir software de forma correcta.
- Los **antipatrones** identifican soluciones erróneas o malas prácticas comunes.
- Son útiles para reconocer código mal diseñado y evitar repetir errores.
- La mayoría de los proyectos fallan en presupuesto o mantenimiento por malas decisiones de diseño.
- Objetivo: reconocer problemas, no culpar.

# Definición de Antipatrón

- Es una solución común que produce consecuencias negativas.
- Causas típicas:
  - Falta de conocimiento.
  - Mal uso de un patrón en un contexto inadecuado.
- Documentación del antipatrón:
  - Nombre, causas, síntomas, consecuencias y solución (refactorización).
- Diferencia con patrones:
  - Un patrón comienza desde cero y aporta una solución.
  - Un antipatrón parte de un problema existente y propone una refactorización.

# Tipos de Antipatrones

- **Antipatrones de desarrollo**: detectables en el código.
- **Antipatrones arquitecturales**: errores de diseño global.
- **Antipatrones de gestión**: problemas en la comunicación y procesos del equipo.

# Refactorización

- Refactorizar = mejorar estructura sin alterar el comportamiento.
- Objetivos:
  - Mejora del mantenimiento.
  - Mejora de la extensibilidad.
- Consejo: refactorizar antes de optimizar código.

# Técnicas comunes de refactorización

## Superclass Abstraction

- Aplicable a clases similares.
- Crea una clase abstracta con métodos comunes.
- Implica:
  - Unificación de firmas de métodos.
  - Creación de superclase abstracta.
  - Migración de implementaciones comunes.

## Conditional Elimination

- Aplica cuando una clase depende mucho de condiciones (`if`, `switch`).
- Crea subclases específicas para cada condición.
- Mueve el comportamiento condicional a las subclases.

## Aggregate Abstraction

- Mejora la estructura entre clases relacionadas.
- Implica cambios en:
  - Relaciones de herencia → agregación.
  - Relación entre componentes → agregados.

# Antipatrón: The Blob (o God Class)

- Una única clase centraliza la lógica del sistema.
- Otras clases solo contienen datos.
- Aparece en diseño procedural aplicado incorrectamente a OOP.
- Indicios:
  - Clase con más de 60 atributos o métodos.
  - Baja cohesión.
  - Muchas responsabilidades.

## Consecuencias

- No se aprovechan beneficios de la POO.
- Difícil de mantener, probar y reutilizar.
- Costosa en memoria.

## Causas

- Requisitos mal asignados.
- Evolución desordenada desde prototipos.
- Falta de conocimientos en diseño OO y arquitectura.
- Falta de abstracción o identificación de componentes.

## Solución (refactorización)

1. Identificar grupos cohesivos de atributos y métodos.
2. Extraerlos a nuevas clases.
3. Reorganizar asociaciones.

Resultado: más clases con lógica distribuida, mayor claridad y cohesión.

## Excepciones

- Puede aceptarse en casos como **wrappers** de sistemas legados donde no se busca particionar el software, sino exponer funciones.

# Antipatrón: Código espagueti:
Ocurre cuando tenemos todo el codigo junto en el main. Solución: separar por responsabilidades en varios ficheros.

# Antipatrón: Data Flow
Cuando vamos mejorando funcionalidades, pero las viejas no las quitamos. 
Consecuencias:
- Codigo demasiado grande
- Codigo que no se utiliza

# Antipatrón: Poltergeist:
Cuando tenemos una clase donde no hay lógica propia (coger mensaje y mandarlo a otro lado). 
- Aumenta la complejidad
- Mayor latencia en la comunicación
