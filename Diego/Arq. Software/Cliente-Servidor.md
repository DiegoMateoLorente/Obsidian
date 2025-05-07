# Arquitectura por capas

Un sistema se divide en tres componentes principales:
- **Cliente**: usuario o programa que accede a través de la capa de presentación.
- **Lógica de aplicación**: Determina lo que el sistema hace. Establece la lógica de negocio.
- **Gestor de recursos**: gestiona almacenamiento y acceso a datos (e.g., bases de datos).

---

# Juego de cajas y flechas
![[Pasted image 20250506023545.png]]
- Cada caja representa una parte del sistema
- Más cajas → mayor modularidad, reutilización y distribución.
- Pero también → más flechas, más sesiones, más complejidad.
- Hay que buscar equilibrio entre flexibilidad y rendimiento.

---

# Diseño descendente (Top-Down)

1. Definir canales de acceso y plataformas cliente.
2. Definir formatos y protocolos de presentación.
3. Definir la funcionalidad necesaria en la lógica de aplicación.
4. Definir fuentes y organización de los datos.

---

# Diseño ascendente (Bottom-Up)

- Se parte de componentes existentes (sistemas legados).
- Se integran mediante *wrappers* y middleware.
- Es útil cuando los sistemas ya existen y no se pueden reemplazar.

---

# Arquitectura de una capa (1-tier)

- Sistema centralizado: presentación, lógica y datos en el mismo servidor.
- Ejemplo: mainframes.
- Ventajas: mayor rendimiento, control centralizado, sin cambios de contexto.

---

# Arquitectura de dos capas (2-tier: cliente/servidor)

- La presentación se mueve al cliente.
- Ventajas:
  - Personalización del cliente.
  - Aprovechamiento del hardware del cliente.
  - Introducción de APIs.
- Inconvenientes:
  - Límite de conexiones.
  - Sin encapsulamiento de errores.
  - Baja escalabilidad.
  - El cliente debe integrarlo todo.

---

# API en cliente/servidor

- Introducción del concepto de servicio e interfaces.
- Se definen APIs para acceder a los servicios del servidor.
- Importancia de la estandarización para interoperabilidad.

---

# Aspectos técnicos del modelo 2-tier

- Ventajas:
  - Despliegue sencillo.
  - Mejor uso de recursos del cliente.
  - Diseño del servidor optimizable.
- Inconvenientes:
  - Sobrecarga en el servidor.
  - Dificultad en manejar errores o fallos.
  - Clientes demasiado acoplados.

---

# Integración de sistemas

- En 2-tier, la integración recae en el cliente.
- Dificulta reutilización, portabilidad y mantenimiento.

---

# Arquitectura de tres capas (3-tier: middleware)

- Separación clara entre presentación, lógica y datos.
- Middleware actúa como capa intermedia.
- Ventajas:
  - Escalabilidad, portabilidad, reutilización.
  - Menor complejidad para el cliente.

---

# Middleware

- Introduce lógica de negocio común.
- Ventajas:
  - Reducción de interfaces visibles para el cliente.
  - Acceso transparente a sistemas subyacentes.
  - Localización de recursos y orquestación de resultados.
- Inconvenientes:
  - Capa adicional.
  - Software complejo.
  - Problemas de estándares e integración entre middlewares.

---

# Sistema middleware de tres capas

- Ejemplo de sistema con clientes externos e internos.
- Lógica distribuida, control de acceso, gestión de recursos.

---

# Arquitectura N-tier (conexión a la Web)

- Expansión del modelo 3-tier con capa web.
- Introducción de servidores de aplicaciones.
- El navegador web se integra como parte de la capa de presentación.

---

# Sistemas N-tier en la práctica

- Infraestructura distribuida: LANs, firewalls, bases de datos, clusters.
- Middleware y gateways permiten interacción entre clientes internos y remotos.

---

# Interacción bloqueante o síncrona

- Cliente espera la respuesta del servidor antes de continuar.
- Requiere que ambos estén activos simultáneamente.
- Problemas:
  - Sobrecarga.
  - Alta probabilidad de fallos.
  - Difícil diagnóstico de errores.

---

# Sobrecarga del sincronismo

- Mantener sesiones consume recursos.
- Uso de *connection pooling* para mitigar esto.
- Se necesita un sistema de gestión de contexto por llamada para hacer conocer el contexto de la interacción.

---

# Fallos en llamadas síncronas

- Distintos puntos de fallo posibles (antes, durante o después de la ejecución).
- Complicado identificar el origen del fallo.
- Problemas se agravan en cadenas de llamadas entre servidores.

---

# Dos soluciones

## Soporte mejorado
- Transacciones.
- Replicación de servicios.
- Balanceo de carga.

## Interacción asíncrona
- El cliente no espera respuesta inmediata.
- Uso de colas persistentes o invocaciones no bloqueantes (tipo batch).

---

# Colas de mensajes

- Excelente complemento al modelo síncrono.
- Ventajas:
  - Modularidad.
  - Flexibilidad para sistemas heterogéneos.
  - Abstracción de sesiones de comunicación.
