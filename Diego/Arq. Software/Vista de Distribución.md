La vista de distribución establece las relaciones entre la arquitectura del software y los elementos no software, el entorno.
El entorno es donde el software se desarrolla, despliega y ejecuta.

Distinguimos tres Vistas:
## Vista de despliegue
Los elementos de la vista de C&C se asignan al hardware de la plataforma de computación sobre la que se ejecutarán. Tiene elementos software y elementos del entorno (hardware). Una asignación válida asegura que los requisitos (propiedades) expresados por los elementos software son satisfechos por las características de los elementos hardware.
### Propiedades de los elementos software
- Consumo de recursos
- Requisitos de recursos y restricciones a satisfacer
- Condiciones de "disponibilidad"
### Propiedades de los elementos del entorno
- Propiedades de la CPU
- Propiedades de la memoria
- Propiedades de los discos
- Ancho de banda
- Tolerancia a fallos
### ¿Para qué sirve?
- Analizar las prestaciones, disponibilidad, la fiabilidad y la seguridad del sistema 
- Para entender las dependencias en tiempo de ejecución → Pruebas del sistema 
- Para realizar la integración del sistema y confeccionar las pruebas de integración 
- Para estimar el coste del hardware
### Suponemos que sistema desplegado en distintas plataformas y configuraciones, como lo representamos.

La diferencia en la vista de distribución entre sistemas sería poniendo estereotipos a los componentes ( <\<linux server>> ) o notas en los propios nodos.



## Vista de instalación
En grandes sistemas Sw el número de ficheros instalados en el entorno de producción puede ser de cientos. Debemos agruparlos/empaquetarlos para instalarlos en el entorno de producción

Puede ser necesario crear distintas versiones del sistema, como diferentes idiomas (aunque si se gestiona bien, solo cambiará una parte muy pequeña de la vista de instalación), versiones (gratis, comercial..), clientes con versiones concretas.

La vista de instalación asigna elementos al sistema de gestión de ficheros en el entorno de producción y muestra cómo el sistema ya instalado está organizado en ficheros y directorios.

Sirve para desarrolladores, instaladores, y operadores del sistema. 
## Vista de asignación de trabajo

Asigna módulos a equipos o personas que serán responsables de llevarlos a cabo

¿Qué tareas/actividades asignamos?
- Implementar e integrar los módulos
- Pruebas de los módulos
- Gestión de configuraciones
- Código para construir y mantener el sistema
- Implementación de la BBDD

Relacionamos módulos con unidades de trabajo. Muestra las grandes unidades de trabajo y quien las realizará, pero también las herramientas que se utilizarán