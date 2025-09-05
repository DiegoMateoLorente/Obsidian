# 28-01-2025
### Estilo de descomposición
Se emplea para representar la relación "es parte de".

Descomponemos el sistema en unidades de implementación, describe la organización del código en módulos y sub módulos, dividiendo responsabilidades.

Es un buen punto para comenzar a diseñar una arquitectura, se basa en divide y vencerás.

DOCUMENTAR EL CRITERIO UTILIZADO PARA DESCOMPONER ES ESENCIAL.

Heurística: Un módulo es suficientemente pequeño si puede ser desechado e implementarlo de nuevo en caso de que el programador(es) abandone(n) el proyecto.

Para que sirve el estilo de descomposición:
Es uno de los primeros documentos que se da a una nueva incorporación a un proyecto,  debe servir para:
- Entender como se distribuyen las responsabilidades del proyecto
- Navegar la estructura de proyecto
- Localizar cambios y distribuir trabajo

##### Restricciones:
- Un módulo puede pertenecer como máximo a un agregado, y no puede haber bucles
- La notación puede ser informal o con UML.

Relación con otras vistas se basa en componentes y conectores. Debemos conocer la transformación entre la vista de descomposición y al menos una de C&C. La transformación es muchos a muchos. Está completamente relacionado con el estilo de asignación de trabajo de la vista de distribución

### Estilos de uso

Lo utilizamos para representar dependencias, relación de uso. Nos dice para un módulo que otros módulos usa. A usa B si para funcionar correctamente depende de una implementación de B.

No existen restricciones, pero la existencia de bucles, cadenas largas de dependencia o muchos módulos que dependen de otro supone que es difícil mantener el sistema controlado de forma incremental.

Sirve para planificar el desarrollo por incremento gradual, pruebas, y estimar los efectos de los cambios.

### Arquitectura por capas:
Visto en ingeniería del software, técnica que sirve para obtener módulos con poco acoplamiento. Agrupamos en una capa módulos que proporcionan un conjunto de servicios relacionados. Dividen completamente el software, ejemplo: capas OSI.

Define interfaces para los servicios públicos y oculta información. Las capas de arriba pueden emplear las inferiores, pero las de abajo NO pueden emplear las de arriba.

Si solo permitimos inmediatamente inferiores, arquitectura cerrada. Si se permiten saltos de más de 1 capa, arquitectura abierta.

##### Restricciones: 
Al menos dos capas, cada módulo pertenece a una capa y NO hay dependencias circulares.

Ejemplo:
![[Arq. Software Vista de Módulos (Estilos) Fig1.png]]

##### Ventajas de trabajar por capas: 
- Equipos con diferentes capacidades trabajarán en ellas
- Reutilizar capas
- Módulos evolucionan juntos en el tiempo
- Modificabilidad, portabilidad, mantenibilidad

##### Para que sirve:
- Cada capa aplica el principio de "ocultar información", que implica que cambios inferiores NO afectan a las de arriba y permanecen ocultos, promueve portabilidad
- PERO cambios en rendimiento si que se propagan
- Las interfaces portables no son dependientes de la plataforma

Ejemplo: arquitectura unix ![[Arq. Software Vista de Módulos (Estilos) Fig2.png]]

La idea de las capas es crear y comunicar la arquitectura, manejar la complejidad de grandes sistemas promoviendo mantenibilidad.

También hay que documentar las propiedades de las capas, describir para que sirve, la lista de módulos y cómo se implementa, y sobre todo JUSTIFICAR POR QUÉ, es muy importante justificarlo todo.

UML no proporciona un constructor explícito por capas, se representa como paquete estereotipado con \<\<layer>>
También puede hacerse con notación informal

##### Relación con otros estilos
Con la vista de descomposición ,establecer correspondencia entre módulos de vistas (capas y descomposición)
- Dos submódulos de un mismo módulo pueden pertenecer a capas diferentes
- Un módulo puede aparecer en más de una capa (1 módulo de vista de descomposición al pasarlo a vista de capas, puede salir en varias capas).

### Aspectos
No trabajaremos con ellos, se puede leer para una idea general ([Vista de módulos - estilos](https://moodle.unizar.es/add/pluginfile.php/13727501/mod_resource/content/3/AS_VistaModulosEstilos.pdf), página 28-35)


#### Crearías un modelo de datos utilizando el diagrama ER para un sistema que no tenga BBDD. En qué situaciones y por qué.
Si tienes que guardar/emplear datos de algún tipo, de forma estructurada (o semiestructurada, de alguna forma) deberemos tener una. También puede ser empleada en sistemas de microservicios.

#### Cuál es la relación de un diagrama de clases UML con los estilos vistos. Muestra descomposición, uso, generalización,…
Estilo de descomposición, de uso (aunque no tan habitual, demasiados cruces de flechas que compliquen el esquema) y de generalización-> diagrama de clases

#### Si nuestro sistema tiene módulos COTS. En qué vista los mostramos y por qué (COTS = commercial off the shelf) 
En la que hemos visto ahora (módulos) no, pero en las que nos quedan por ver PUEDE que si (generalmente vista de componentes o de despliegue).

#### Discutir generalización y herencia
- Herencia: Clase hija hereda comportamientos de la clase padre. Es un tipo específico de generalización, centrada en POO.
- Generalización: la emplearemos EN MÓDULOS, proceso de abstraer características comunes.

#### Pensad un sistema que no pueda ser descrito por capas. Si no es por capas ¿qué nos dice esto sobre la relación “puede usar”
Sistemas que tenga mucha interdependencias entre módulos, ya que por las características de la arquitectura de capas descritas previamente no podremos emplearla.