# 05-02-2025

Esta vista tiene la función de mostrar elementos con presencia en tiempo de ejecución.

Distinguimos entre componentes y conectores:
- Componentes -> Procesos, objetos, clientes, servidores... En UML se etiquetan como 
	"<<component">>"
- Conectores -> Caminos de interacción entre los componentes. Ej: Llamada a procedimiento, invocación a servicio, RPC...

Las vistas son ubicuas en la práctica. Casi todos los diagramas de cajas y flechas que se autodenominan "de arquitectura" son vistas de CyC.
Las vistas se utilizan para mostrar cómo funciona el sistema. Especifican la estructura y el comportamiento de los elementos en tiempo de ejecución. También permiten razonar sobre la calidad del sistema en tiempo de ejecución.
![[Arq. Software Vistas de Componente y Conector (CyC) Fig1.png]]

Este diagrama (de representación informal) representa una vista primaria de la arquitectura del sistema en tiempo de ejecución. Además de una leyenda y el diagrama, también se debe tener documentación de soporte. Componentes:
![[Arq. Software Vistas de Componente y Conector (CyC) Fig2.png]]

La representación debe responder a las siguientes preguntas:
- ¿Cuáles son los principales componentes del sistema y cómo interaccionan? 
- ¿Qué partes del sistema están replicadas, y cuántas veces? 
- ¿Cómo progresan los datos por el sistema conforme este se ejecuta?
- ¿Qué protocolos de comunicación se usan? 
- ¿Qué partes del sistema se ejecutan en paralelo? 
- ¿Cuáles serán las prestaciones, fiabilidad o disponibilidad (estimadas) del sistema?

### Diferencias entre diagrama CyC y diagrama de módulos

El diagrama de módulos es más estático, mientras que le de CyC es dinámico (muestra los componentes en tiempo de ejecución).



### Componentes en diagrama CyC

Los componentes son unidades de procesamiento principal y almacenes de datos. Estos tienen puertos para interactuar con otros componentes mediante conectores.

Ej: data: Database -> data es una instancia del componente Database
![[Arq. Software Vistas de Componente y Conector (CyC) Fig3.png]]

### Puertos de los componentes 

Los puertos suelen tener tipos explícitos.
- Definen el comportamiento de dicho puerto en la interacción
- Se deben documentar en la Documentación de soporte
Un componente puede tener varios puertos de 1 o varios tipos. 
En caso de haber "subarquitecturas", se debe documentar la relación entre puertos internos y externos.
![[Arq. Software Vistas de Componente y Conector (CyC) Fig4.png]]En este diagrama, la diferencia es que la instancia de la izquierda tiene un rango de 1 a 5 puertos, mientras que la de la derecha tiene 2 puertos.

### Conectores
Son caminos de interacción entre los componentes. Pueden ser sencillos (pipe) o complejos (bus de comunicaciones).

Tienen roles, que indican cómo los componentes pueden usarlos en sus interacciones. Es la interfaz de un conector.
Un rol define las expectativas sobre un participante en una interacción. Se puede documentar como una especificación de protocolo. 
No todos los conectores son binarios (tener exactamente 2 roles). Pueden tener varios.
![[Arq. Software Vistas de Componente y Conector (CyC) Fig5.png]]

Para asignar un rol al conector se pueden usar las etiquetas de los extremos del conector. Se puede incluir información adicional en una etiqueta (tag) del conector.

Si hay un componente mediador en una interacción entre otros componentes, en vez de considerarlo conector mejor hacerlo como componente.

#### Pregunta: ¿Que diferencias hay entre modelar dos procesos que se comunican por sockets, o dos procesos que se comunican por RPC (p.ej. Sun RPC) en una vista de CyC?
Una comunicación de RPC es distinta que la comunicación de sockets. Lo que cambia es el tipo de conector

### Tipos de relaciones
#### - Acoplamiento (attachment)

Los puertos de los componentes se asocian con los roles de los conectores formando así grafos de componentes y conectores

#### - Delegación de interfaces
![[Arq. Software Vistas de Componente y Conector (CyC) Fig6.png]]

#### Restricciones 
- Los componentes solo pueden acoplarse a conectores (lo mismo al revés)
- Los acoples solo pueden hacerse entre puertos y roles compatibles
- La delegación de interfaces solo es válida entre puertos compatibles (o roles compatibles)
- Los conectores no pueden estar aislados

### Propiedades
Las componentes y conectores deben tener propiedades:
- Nombre y tipo
- Otras dependiendo del tipo (para guiar la implementación o para realizar análisis sobre la vista)
Los puertos y roles también pueden tener propiedades (tasa máxima de peticiones en un puerto).

#### Ejemplos
Fiabilidad (% fallo), prestaciones (tiempo de respuesta, latencia), almacenamiento (memoria necesaria), seguridad (¿requiere autenticación?), concurrencia (¿usa su propio thread?), tasa de peticiones máxima soportada (propiedad del puerto de un servidor)

### Tipos e instancias
En una vista CyC suele haber instancias de componentes y conectores de varios tipos
- Tipo -> Componente o conector no definido completamente.
- Instancia -> Resultado de completar esa definición (conforme al tipo)
Las guías de estilos arquitecturales proporcionan tipos.

### Interfaces
![[Arq. Software Vistas de Componente y Conector (CyC) Fig7.png]]
![[Arq. Software Vistas de Componente y Conector (CyC) Fig8.png]]
![[Arq. Software Vistas de Componente y Conector (CyC) Fig9.png]]
