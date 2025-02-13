# 05-02-2025

Es responsabilidad del arquitecto de software: 
- Entender para cada componente sus necesidades de interacción 
- Identificar para cada una de esas interacciones sus atributos relevantes
- A la vista de lo anterior seleccionar los conectores candidatos 
- Asesorar los trade-offs de cada candidato
- Analizar las propiedades clave de la interacción entre el componente y el conector

Los conectores:
- Son elementos arquitecturales independientes de la aplicación que desarrollamos
- Soportan dos principios básicos: abstracción y separation of concerns .![[Arq. Software Conectores Fig1.png]]
Documentación general del comportamiento de una "pipe":
- Pasa streams de datos sin formato.
- Es unidireccional
- 1 emisor, 1 receptor
- No almacena datos
- Envía datos 1 vez, si no llegan se pierden.

### Categorías de conectores:

- Comunicación:
	- Transfieren información entre los componentes
- Coordinación:
	- Dan soporte a la transferencia de control
- Conversión:
	- Permiten que componentes que no han sido diseñados para interactuar puedan hacerlo. Posibilitan la interacción entre componentes heterogéneos.
- Facilitación:
	- Proveen servicios para facilitar y optimizar la interacción entre los componentes.

Con las categorías no tenemos suficiente información para modelar y analizar los conectores de nuestra arquitectura, ni tampoco crear nuevos conectores.

#### Tipos de conectores

Tenemos 8 tipos: 
- Llamada a procedimiento
- Evento
- Acceso a datos
- Linkage
- Stream
- Mediador
- Adaptador
- Distribuidor

A partir de ellos podemos construir conectores compuestos más complejos.

#### Llamada a procedimiento:

Es la más común. Tiene parámetros y valores de retorno. Tiene diferentes técnicas de invocación. Ejemplo: Fork y exec en Unix, o RPC.
![[Arq. Software Conectores Fig2.png]]

#### Eventos

El flujo se desencadena por la ocurrencia de un evento. Cuando el conector “percibe” el evento genera mensajes (notificaciones) para los componentes implicados y les pasa el flujo de control. Obviamente tiene comunicación asíncrona.

El evento puede llevar información: tiempo y lugar de ocurrencia, datos...
![[Arq. Software Conectores Fig3.png]]

#### Acceso a datos

Permiten que los componentes accedan a fuentes de datos mantenidas por un componente. Los datos pueden estar almacenados de manera persistente o temporal.

Pueden realizar conversión de formatos si difieren el formato almacenado del formato solicitado

Ejemplo: 
- Persistente: Acceso SQL a BBDD.
- Temporal: Acceso a pila de memoria o cacheo de información.
![[Arq. Software Conectores Fig4.png]]

#### Stream

Su función es transferir grandes cantidades de información entre procesos autónomos. Se combinan con los conectores de acceso a datos y eventos.

Ejemplos: Sockets, pipes Unix
![[Arq. Software Conectores Fig5.png]]

#### Linkage

Permite unir componentes durante su interacción. Habilitan “canales” para comunicación y coordinación que son utilizados por otros conectores.

Granularidad: Nivel de detalle para establecer la unión
- Unidad -> Componente depende de otro
- Sintáctica -> Relaciones entre variables, procedimientos, funciones, etc. dentro de los componentes unidos
- Semántica -> Cómo deben interaccionar los componentes unidos
![[Arq. Software Conectores Fig6.png]]



#### Mediadores

Destacan por la coordinación y facilitación de acciones.

Ejemplo: Un sistema multithread necesita memoria compartida y control de concurrencia

Los componentes saben que existen otros componentes pero no conocen su estado. Este conector organiza la ejecución del sistema, resuelve conflictos, balanceo de carga
![[Arq. Software Conectores Fig7.png]]

#### Adaptadores

Hacen que interoperen componentes en entornos heterogéneos. Aplican conversión para que interactuen  componentes que no han sido diseñados para ello.
![[Arq. Software Conectores Fig8.png]]

#### Distribución

Identifican caminos de interacción entre componentes y después establecen comunicación y coordinación

Ejemplos: DNS, routing, diferentes servicios de red...
![[Arq. Software Conectores Fig9.png]]

