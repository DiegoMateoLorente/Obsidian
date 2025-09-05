# Definición

- Servicios desplegables independientemente y diseñados desde el punto de vista del negocio.
- Encapsulan funcionalidad accesible por red (API REST, RMI, etc.).
- Cada microservicio puede gestionar una parte (almacén, pedidos, envíos).
- Alternativa arquitectural frente a los monolitos.

# Características principales

- Se despliegan de forma independiente.
- Agnósticos a la tecnología.
- Exponen funcionalidad por endpoints.
- Cada uno encapsula su propia BBDD.
- Se desarrolla y actualiza de forma aislada.

# Encapsulamiento

- Ocultan toda la información posible.
- Interfaz externa mínima.
- Lo interno puede cambiar libremente si no se rompe la compatibilidad.
- Baja el acoplamiento, facilita actualizaciones y despliegues.

# Despliegue independiente

- Cambiar un microservicio no afecta al resto.
- Requiere contratos estables y bien definidos.
- Obstáculos: uso de bases de datos compartidas.

# Diseño y dominio

- Usan DDD (Domain Driven Design).
- Se busca que los cambios entre microservicios sean poco frecuentes.
- Hay que coordinar desarrollo y despliegue si participan varios microservicios.

# Gestión del estado

- Sin BBDD compartidas.
- Un microservicio debe preguntar al otro si necesita su información.
- Oculta su implementación y evita dependencias.
- Se garantiza la compatibilidad hacia atrás.

# Tamaño

- No importa tanto en líneas de código.
- Deben ser comprensibles y con interfaces pequeñas.
- Más microservicios implican más complejidad de gestión.

# Flexibilidad

- Permiten responder a cambios futuros.
- Hay que equilibrar coste de arquitectura vs opciones futuras.
- Se recomienda introducirlos de forma progresiva.

# Alineación con la organización

- Ley de Conway: la arquitectura refleja la estructura de comunicación organizativa.
- Se fomentan equipos multidisciplinares, alineados con líneas de negocio.
- Cada equipo tiene la responsabilidad total sobre su servicio.

# Comparativa con monolitos

- Monolito = todo el sistema en un único proceso.
- Monolito modular = único proceso dividido en módulos.
- Monolito distribuido = múltiples servicios desplegados juntos.

## Problemas del monolito

- Equipos que trabajan sobre el mismo código.
- Despliegues no independientes.
- Conflictos de decisión.

## Ventajas del monolito

- Despliegue y pruebas integradas más sencillas.
- Reutilización de código.

# Tecnologías comunes

- Lenguajes: Java, Python, Node.js, etc.
- Frameworks: Spring Boot, Flask, FastAPI, .NET, etc.
- Orquestación: Docker, Kubernetes.
- Comunicación: REST/HTTP, Kafka, RabbitMQ, NATS.
- Balanceo: Eureka, Consul, Istio.
- Seguridad: OAuth2, KeyCloak.
- Observabilidad: ELK stack, Prometheus, Grafana.
- CI/CD: Jenkins, GitHub Actions.

# Ventajas

- Ocultación de información, DDD, y sistemas distribuidos combinados.
- Tecnología heterogénea por microservicio.
- Robustez: fallo de un servicio no afecta a otros.
- Escalabilidad: solo escalo lo necesario.
- Despliegues más frecuentes, rollback sencillo.
- Alineamiento organizativo: mayor productividad.
- Componibilidad: múltiples plataformas.

# Desventajas

- Experiencia del desarrollador: difícil ejecutar muchos servicios localmente.
- Curva de aprendizaje de herramientas y conceptos.
- Costes iniciales mayores (infraestructura y formación).
- Dificultad para informes integrados (datos dispersos).
- Retos en monitorización, seguridad, pruebas, latencia y consistencia.

