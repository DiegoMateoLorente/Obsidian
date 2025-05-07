# Contenidos

- Limitaciones de RPC y broker de objetos
- Middleware orientado a mensajes (MOM)
- Brokers de mensajes
- Patrón Observer
- Publish/Subscribe
- Ejemplos prácticos

# Limitaciones de RPC y Broker

- RPC y brokers de objetos son intra-empresa y sincronizados.
- Problemas:
  - Falta de interoperabilidad entre brokers.
  - Difícil apertura a clientes externos.
  - No soportan lógica de enrutamiento.
- Solución: usar middleware orientado a mensajes (MOM).

# Middleware Orientado a Mensajes (MOM)

- Comunicación basada en mensajes estructurados (`<nombre, valor>`).
- Desacopla cliente y servidor.
- Usa colas de mensajes para interacción asíncrona.
- API para envío/recepción:
  - Envío no bloqueante.
  - Recepción puede ser bloqueante, con hilos o callbacks.

# Ventajas de MOM

- Receptor controla cuándo procesar.
- Resiliencia ante fallos: mensajes persistentes en cola.
- Balanceo de carga con colas compartidas.
- Garantiza entrega única y ordenada.
- Soporte para mensajes con prioridades y transacciones.

# Limitaciones de MOM

- Unión punto a punto → estática.
- Sin lógica de enrutamiento ni soporte nativo a la heterogeneidad.
- Difícil de escalar o modificar dinámicamente.

# Ejemplo de limitación: Cadena de suministro

- Involucra múltiples fases y sistemas heterogéneos.
- Cada sistema puede usar diferentes tecnologías.
- Automatización difícil sin integración flexible.
- Añadir nuevas aplicaciones requiere modificar emisores.

# Brokers de Mensajes

- Extienden MOM con lógica de enrutamiento en el middleware.
- Identifican colas destino según:
  - Tipo de mensaje.
  - Contenido.
  - Identidad del emisor.
- Proporcionan adaptadores y transformaciones de mensajes.
- Permiten desacoplamiento completo entre emisores y receptores.

# Lógica de Enrutamiento en Brokers

- Se define como reglas:
  - Condiciones lógicas.
  - Acciones (entrega a ciertas colas).
- Puede estar en el núcleo del broker o en las colas.
- Ventajas:
  - Centralización de la lógica.
  - Facilidad de modificación.
- Inconvenientes:
  - Riesgos de sobrecarga.
  - Dificultad de depuración.

# Patrón Publish/Subscribe (P/S)

- Modelo basado en eventos.
- Productores publican mensajes en el broker.
- Consumidores suscritos reciben los mensajes que les interesan.
- Se basa en el patrón Observer (1 a N).

# Diferencias Observer vs P/S

- Observer:
  - Local, sincrónico, fuerte acoplamiento.
- P/S:
  - Distribuido, asíncrono, bajo acoplamiento.
  - Comunicación multicast.

# Ventajas de P/S

- Desacopla totalmente emisores y receptores.
- Permite añadir nuevos receptores sin alterar emisores.
- Escalable y flexible.

# Filtrado de Mensajes

- Por tipo (topic).
- Por contenido (condiciones sobre parámetros).
  - Ej: `"Cliente = 'ACME' and Cantidad > 1500"`

# Variantes de P/S

- **Push**: el publicador envía todo a los suscriptores.
- **Pull**: el publicador avisa y los suscriptores recuperan.
- Push = menos mensajes, más grandes.
- Pull = más flexible, pero más mensajes.

# Arquitectura P/S

- Usa un conector tipo bus de eventos.
- Componentes tienen puertos de publicación y/o suscripción.
- Se define la lógica de entrega según los eventos.
- Propiedades configurables:
  - Prioridades, orden, fiabilidad, dinámicas de suscripción.

# Ejemplos de uso

- GUIs y MVC (Modelo-Vista-Controlador).
- Plugins que reaccionan a eventos.
- RSS, listas de correo, redes sociales.
- Monitorización (como temperatura de CPU).

# Ejemplo: Monitorización de Temperatura

- Sensor publica eventos: temperatura periódica, máxima, crítica.
- Varios suscriptores reaccionan:
  - Widget gráfico.
  - Alarma sonora.
  - Controlador de apagado.

# Ejemplo en RabbitMQ

- Publicadores envían a centralitas (exchanges) tipo fanout.
- Todas las colas unidas a la exchange reciben el mensaje.
- Puede usarse `topic exchange` para filtrado por tema.
- Soporta múltiples publicadores y suscriptores sin acoplamiento.

# Ejemplo: Web Chat distribuido

- Usuarios publican mensajes de chat.
- Sistema monitoriza mensajes con posibles insultos:
  - Si detecta lenguaje ofensivo, envía mensaje de expulsión.
- Tecnologías:
  - `Node.js` + `socket.io` para clientes web.
  - `RabbitMQ` para back-end asíncrono.

# Detalles del Web Chat

- `server.js`: servidor web con `socket.io`.
- `TextParser.java`: detecta insultos y publica expulsiones.
- `chat_application.html`: interfaz cliente.
- `socketio_server.js`: reenvía mensajes entre navegadores y hacia AMQP.
- `amqpbridge.js`: comunica con RabbitMQ.
- `EmisorPubSub.java` / `ReceptorPubSub.java`: ejemplo básico de fanout en Java.

# Conclusión

- El estilo publicación-suscripción:
  - Promueve modularidad, flexibilidad y escalabilidad.
  - Permite construir sistemas robustos y desacoplados.
  - Ideal para arquitecturas reactivas, eventos y comunicación distribuida.
