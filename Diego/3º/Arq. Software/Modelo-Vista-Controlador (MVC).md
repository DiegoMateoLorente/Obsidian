# Presentación
- Surgido a finales de los 70 y desarrollado por Trygve Reenskaugk para Smalltalkk
- Por un lado, une la información y la funcionalidad (lógica de negocio) de la aplicación (Modelo)
- Por otro lado, trata la gestión de la interacción con el usuario
	- Procesar entrada -> Controlador
	- Procesar salida -> Vista
# Contexto 
- Para el desarrollo de aplicaciones interactivas, se necesitaba un GUI flexible (con múltiples vistas para atender necesidades y portando la aplicación a diferentes plataformas -> Modelo reutilizable)
# ¿Cuándo se usa?
- Hay datos y funcionalidad que se quieren ver y manipular desde distintos tipos de interfaz de usuario
- O bien hay ciertos datos que se quieren representar de formas distintas simultáneamente (queriendo percibir cambios en los datos al instante (mediante Observer))
# Definiciones
- Modelo: datos y funcionalidad (lógica de negocio) del sistema, independientes de cómo se muestran o manipulan
- Vista: presenta la información del modelo al usuario
- Controlador: procesa las entradas del usuario y las dirige al modelo (excepcionalmente a las vistas)
- Mecanismo de propagación del cambio Garantiza la consistencia entre la interfaz de usuario y el modelo
# Modelo
Encapsula todos los datos y ofrece las operaciones para su acceso y procesamiento. Los controladores y las vistas acceden a las operaciones (API) del modelo.
El modelo notifica (a vistas y a algunos controladores) sus cambios de estado (tipicamente mediante Observer).
# Vistas
Las vistas presentan datos y resultados del modelo al usuario y se actualizan cuando el modelo notifica cambios.
Típicamente cada vista está asociada con un controlador
Las vistas permiten al controlador manipular “algo” la GUI directamente, cuando no varía el modelo (p.ej. scrolling).
# Controladores
Los controladores reciben las entradas del usuario como eventos (el cómo las recibe es variable) y los convierte en peticiones para el modelo (o para la vista asociada).
Un controlador se suscribe al modelo si su comportamiento puede depender del mismo (p.ej. deshabilitar alguna opción de menú tras un cambio en los datos).
# Diagrama de clases MVC
![[Arq. Software Modelo-Vista-Controlador Fig1.png]]

# Vista de CyC del patrón MVC
![[Arq. Software Modelo-Vista-Controlador Fig2.png]]
## Documentación de puertos
- Manipulación de Vista: operaciones relacionadas con cambios en la GUI y presentación de datos dependientes de entradas del usuario 
- Acceso a Datos: acceso a la API del Modelo (p.ej. getters) 
- Acceso y Manipulación de Datos: acceso a la API del Modelo, consulta y modificación de los datos del Modelo (p.ej. getters y setters), validación de ciertos datos (p.ej. ¿es este número un DNI válido?), ... 
- Manejo de Eventos: operaciones que reciben y procesan los eventos debidos a entradas del usuario (ratón, teclado...) 
- Mostrar: mostrar en el monitor
## Elementos
- Componente Vista
- Componente Modelo
- Componente Controlador
- Coenctores:
	- Los habituales entre los componentes que se usen (p.ej. llamadas entre objetos)
	- Conector pub-sub para suscribirse y para notificar los cambios en el modelo.
# Versiones MVC
La explicada hasta ahora es la del Buschmann et al. (la “clásica”).
Existen más versiones:
## Modelo-Vista (DocumentoVista)
Se usa en aplicaciones GUI. La aplicación es una “ventana”, con menús y barras de herramientas, que manipula documentos.
Se unen las responsabilidades de Vista y Controlador en un único módulo (El Modelo se queda igual).
## Modelo-Vista-Presentador
Ambas variantes del MVP (explicados a continuación) permiten crear “tests” automáticos más fácilmente, porque la lógica de la interacción con la GUI se separa de su representación gráfica.
- Vista Pasiva: el Presentador es más complejo, pero puedes crear tests automáticos para todos los casos.
- Presentador Supervisor: el Presentador es más simple porque la Vista gestiona algunas actualizaciones, pero sólo puedes probar automáticamente (al menos de forma sencilla) los casos que implementas en el Presentador (que son los complejos y los que es más importante que pruebes).
### Modelo-Vista-Presentadoro (Vista Pasiva)
Al igual que MV(DV) se utiliza en aplicaciones con GUI.
El Modelo es igual que en MVC. El Presentador:
- Responde a los eventos de usuario (como haría un Controlador), 
- Es responsable de actualizar el modelo, 
- Tiene la lógica “de pintado” que estaba en la Vista (p.e. el formateo de un texto) 
- Es responsable de actualizar la Vista 
- Sigue siendo independiente de la tecnología GUI
La Vista es pasiva; ya no se actualiza leyendo del Modelo, la actualiza el Presentador
### Modelo-Vista-Presentador (Presentador Supervisor)
Exactamente igual que el de Vista Pasiva, pero el Presentador Supervisor lleva a cabo parte de la lógica de la Vista y por tanto parte de la sincronización entre Modelo y Vista. La Vista se actualiza leyendo del Modelo, pero solo las partes más sencillas. Las partes más complejas las actualiza el Presentador Supervisor.
## MVC para Web
Surge en la primera década de los 2000. 
- El Controlador es el punto de entrada a la aplicación, no la Vista 
- La Vista no presenta directamente “algo” al usuario. La vista “ensambla” HTML/JS/CSS para que lo muestre el Browser 
- El HTML/JS captura los eventos (clic de botones) que desencadenarán nuevas acciones vía XMLHttpRequest
Más adelante, surge el patrón MVC para la web moderna.
- Surge el SPA (Single Page App), donde ahora la latencia es mucho menor ya que la página, en vez de recargarse, se reescribe en cada actualización. Además, incluye lógica (en JS) para hacer peticiones a la API HTTP contra recursos que son servidos por “API Controllers”, respondiendo normalmente en JSON.