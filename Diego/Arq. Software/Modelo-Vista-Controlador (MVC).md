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
El modelo notifica (a vistas y a algunos controladores) sus cambios de estado (tipicamente mediante Observer)
# Vistas
#rellenar 
# Controladores
#rellenar 
# Diagrama de clases MVC
![[Arq. Software Modelo-Vista-Controlador Fig1.png]]


# Resto #rellenar con lo que falte del pdf