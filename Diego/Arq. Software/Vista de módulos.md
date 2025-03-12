# 28-01-2025
Vista de módulos: La que describe estructuras que implementan el sistema (los dichos módulos) y las relaciones de los mismos

Módulo: Unidad de implementación (parte del código fuente) que proporciona un conjunto coherente de responsabilidades, que definen las funcionalidades del módulo y qué conocimiento  mantiene.

### Principios del módulo:

- Ocultación de la información (Principios de Parnas) -> Es necesario proporcionar la información al desarrollador para implementar responsabilidades del módulo, pero LIMITAR todo lo que sea extra a ello.
- Acoplamiento -> Nos dice grado de interdependencia entre los módulos del sistema.
	- Alto acoplamiento implica conexión estrecha y cambios en un módulo afectan a otros. 
	- Bajo acoplamiento, son independientes y cambios no afectan a otros módulos.
	- Tipos de acoplamiento: 
		- Control: Los módulos se pasan información de control.
		- Externo: Los módulos dependen de módulos externos al proyecto
		- Común: Los módulos comparten información (estructuras de datos globales)
		- Contenido: Un módulo puede modificar la información de otro/s módulos
		- etc.

Acoplamiento de arquitectura es importante, nos interesa que sea bajo, mejora la mantenibilidad del sistema, facilita modularidad y pruebas, se pueden comprobar funcionalidades de forma aislada, y la escalabilidad ya que añadir nuevos módulos será más sencillo.

- Cohesión -> Medida en la que los elementos dentro de un módulo trabajan juntos para un único propósito, fin común. 
	- Alta cohesión -> Los elementos del módulo están estrechamente relacionados y consiguen un único propósito
	- Baja cohesión -> Los elementos del módulo tienen poca relación entre sí y abordan múltiples propósitos
	- Tipos de cohesión:
		- Funcional: el módulo realiza una única tarea.
		- Secuencial: existe un flujo de datos entre las distintas partes del módulo
		- Temporal: todas las tareas del módulo se ejecutan en el mismo espacio de tiempo
		- Comunicación: dos módulos operan con la misma entrada o contribuyen a producir la misma salida
		- el módulo realiza tareas relacionadas lógicamente, no funcionalmente
		- etc.

Nos interesa que la cohesión sea alta, proporciona una mejor legibilidad para entender el código y mejora la fiabilidad.

### Relaciones entre módulos

Tres tipos: Es parte de, Dependencia, Es un

#### Es parte de
Lo conocemos como orientación a objetos como agregación o composición.

El módulo AGREGADO (el todo) está formado por sub_módulos (componentes) que también son módulos.

#### Dependencia
Tiene tres tipos:
- Uso: Módulo A usa al módulo B, da lugar a "estilo de uso"
- Permite su uso: Da lugar al "estilo de capas"
- Transversal: Da lugar al "estilo de aspectos"

#### Es un (Is a)
Como la especialización/generalización, es lo que conocemos como herencia, da lugar a "estilo de generalización"


### Propiedades de los módulos

Es lo que debemos documentar, sirven para implementación y también análisis del sistema. 

##### Que documentar:
- Nombre:
	- Debe ser significativo
	- Refleja posición en la jerarquía de descomposición
- Responsabilidades:
	- El nivel de detalle dependerá de lo que se considere necesario 
	- Responsabilidad: La tarea o función principal a realizar por el módulo, que debe ser clara. Puede tener interacciones (reflejadas también en el esquema, datos a manejar,  etc)
- Interfaces:
	- Internas: Solo empleadas por los hermanos, el padre no conoce estas
	- Externas, relación con el padre, tienen encapsulación (padre proporciona interfaces y mapea peticiones a los hijos)
- Información de implementación:
	- PESE A NO PERTENECER A LA ARQUITECTURA, ES UN BUEN SITIO PARA GUARDAR INFO SOBRE ESTA. Ejemplos:
	- Lista de ficheros que forman parte
	- Información para probar el módulo
	- Restricciones de implementación / comunicación
	- Fechas limite, otros datos relevantes


### Para que sirve todo esto
Dividir el diseño de forma modular es una forma de gestionar un equipo y repartir el trabajo de forma separada, sin pisarse entre los miembros o subgrupos de un proyecto.

Realizar un plan de acción para la construcción del software, análisis de impacto, seguimiento de los requisitos, explicar funcionalidad del sistema y estructura del código fuente, estructura de información persistente (modelo de datos, los datos que maneja nuestro sistema).

Diagrama de paquetes ejemplo en el que se ve esta modularidad y como modificar admin.client tendrá que probarse en los que tienen relación de tipo USE

![[Arq. Software Vista de Módulos Fig1.png]]


### Como documentar la vista
- Notación informal
- Matriz de dependencias
- UML diagrams
- Diagrama entidad-relación

##### Notaciones informales

- Gráfica, cajas y lineas. Ejemplo: ![[Arq. Software Vista de Módulos Fig2.png]]

- UML (lenguaje de modelado unificado)
	Lenguaje que permite modelar TODO, propósito general, con lo que está bien porque brinda un estándar pero no siempre va a ser el mejor para especificar claramente algo muy concreto.
	Ejemplo: ![[Arq. Software Vista de Módulos Fig3.png]] ![[Arq. Software Vista de Módulos Fig4.png]] ![[Arq. Software Vista de Módulos Fig5.png]]
	

### Puntos a discutir

##### Cual debería ser el nivel de detalle en la vista de módulos:

Aquel nivel de detalle que sea suficiente para que los lectores comprendan estructura y responsabilidades del sistema.

##### Cual es la diferencia entre las responsabilidades de un módulo y los requisitos que debe satisfacer

Responsabilidades: La tarea principal del módulo, lo que debe hacer 
Requisitos: Necesidades del módulo, expectativas (que es lo que debe cumplir)

##### Todos los módulos agregados son módulos
Si, la diferencia es el nivel de abstracción, los módulos pueden estar formados por más módulos

##### Mostrarías las librerías de las que dependen tus módulos como módulos en tu vista de módulos
Si no es necesario que se conozca, por ejemplo si el sistema está muy ligado o depende totalmente de una librería, no se debe especificar las librerías de las que dependan.

##### Qué tipos de dependencias pueden expresarse en la vista de módulos
Puesto arriba lol (Figura 3,4,5)

##### Qué podemos decir sobre el flujo de control en la vista de módulos? ¿sobre el flujo de datos?

Flujo de control: Una vista de módulos no dice nada sobre el flujo de control. Flujo de control de una aplicación software es el orden en el que se ejecutan las cosas dentro de el software.  

Flujo de datos: Flujo de datos es como se mueve la información. Con las dependencias se puede ver "un poco" como se mueve la información pero de manera general con las vistas NO se puede ver mucha información sobre el flujo de datos.

### Resumen
1. La vista de módulos es arquitectural → Los módulos nos dicen cómo se descompone el sistema en unidades de implementación
2. Los módulos se relacionan entre sí
3. Plan de acción para el código fuente y el modelo de datos
4. Tendremos al menos una vista de módulos
5. La propiedad de responsabilidad es la más importante
6. Documentar la interfaz del módulo
7. Esta vista tiene correspondencia con la de C&C