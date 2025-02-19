# 19-02-2025
## Atributos
### Mantenibilidad
Es la facilidad con la que un sistema puede ser modificado después de su implementación inicial.
### Escalabilidad
Capacidad de un sistema para manejar un crecimiento (en número de usuarios, volumen de datos, complejidad de las funcionalidades...)
Un sistema escalable es aquel que puede adaptarse a cambios sin perder rendimiento.
### Flexibilidad
Capacidad de un sistema para adaptarse a nuevas necesidades o cambios. Dichas necesidades son dinámicas, es decir, evolucionan con el tiempo
Un sistema flexible es aquel que puede evolucionar sin requerir excesivo tiempo o esfuerzo.
### Rendimiento
Es la eficiencia del sistema en términos de uso de recursos (memoria, CPU, tiempo de respuesta...)
Existen compromisos con otros atributos (flexibilidad, escalabilidad, etc.)
Un sistema con buen rendimiento es aquel que hace un uso óptimo de los recursos disponibles.
### Seguridad
Capacidad del sistema para protegerse contra accesos no autorizados, ataques y garantizar la integridad y confidencialidad de los datos. 
También es conocida como la triada CIA (confidentiality, integrity, availability).
Es un principio fundamental, sobre todo si se manejan datos sensibles.
Un sistema seguro es aquel que incorpora medidas para protección de la información
### Otros principios
![[Arq. Software Principios de Diseño Fig1.png]]
## Principios SOLID
### Responsabilidad única
![[Pasted image 20250219152100.png]]
CuentaBancaria ahí tiene varias responsabilidades.
#### Consecuencias
- Difícil de mantener (Cambios en la autenticación o en los reportes afectan el código relacionado con las transacciones)
#### Solución
La mejor solución es crear varias clases, cada una con una responsabilidad, en vez de una sola. Ventajas: Más reutilizable, escalable, mayor mantenibilidad, etc.
### Abierto/Cerrado
![[Arq. Software Principios de Diseño Fig3.png]]
Cada vez que agregamos un nuevo método de pago, debemos modificar la clase ProcesadorPagos
#### Consecuencias
- Código propenso a errores y difícil de mantener.
#### Solución
![[Arq. Software Principios de Diseño Fig4.png]]
![[Arq. Software Principios de Diseño Fig5.png]]
### Sustitución de Liskov
![[Arq. Software Principios de Diseño Fig6.png]]
El pingüino no puede volar.
#### Consecuencias
- Código fráagil, acoplamiento innecesario, dificultad de manener,dificultad de extensión, etc.
#### Solución
![[Arq. Software Principios de Diseño Fig7.png]]
Código más robusto, mayor mantenibilidad, más extendible, etc.
### Segregación de interfaces
![[Arq. Software Principios de Diseño Fig8.png]]
Una clase no debe verse obligada a implementar métodos que no necesita (robot no come ni descansa)
#### Consecuencias
- Código inflado, frágil, mayor complejidad, etc.
#### Solución
![[Arq. Software Principios de Diseño Fig9.png]]
### Inversión de dependencias
![[Arq. Software Principios de Diseño Fig10.png]]
Depender de abstracciones en vez de implementaciones concretas
#### Consecuencias
- Alto acoplamiento, dificultad de entendimiento, código frágil  y poco reutilizable, etc.
#### Solución
![[Arq. Software Principios de Diseño Fig11.png]]

### Resumen
#### Principios SOLID: 
S – Single Responsibility Principle 
O – Open/Closed Principle 
L – Liskov Substitution Principle 
I – Interface Segregation Principle 
D – Dependency Inversion Principle
Son fundamentales en la arquitectura y el desarrollo del software
![[Pasted image 20250219154241.png]]
![[Pasted image 20250219154251.png]]
![[Pasted image 20250219154259.png]]
![[Pasted image 20250219154308.png]]
![[Pasted image 20250219154314.png]]
## Principios de diseño orientado a objetos
### Don't Repeat Yourself (DRY)
Evitar la duplicación de código
#### Ejemplo
- En lugar de copiar y pegar una función en varios módulos, se crea un método reutilizable en una clase común
### Keep It Simple, Stupid (KISS)
Mantener el código lo más simple posible. Evitar complejidad innecesaria diseñando soluciones simples y directas.
### You Ain't Gonna Need It (YAGNI)
No implementar aquello que no se necesite. Evitar implementar funcionalidades innecesarias que podrían nunca usarse.
### Principio de mínimo conocimiento
Limitar la cantidad de información que una clase conoce sobre otras clases.
#### Ejemplo
- En lugar de usuario.getCuenta().getSaldo(), usa usuario.obtenerSaldo() → evita encadenar llamadas a métodos de objetos internos
## Otros principios de diseño
### General Responsibility Assignment Software Patterns (GRASP)
Proporcionan guías sobre cómo asignar responsabilidades dentro de un sistema
### Command Query Separation
Separa métodos que modifican el estado del sistema (commands) de aquellos que solo lo consultan (queries)
### Alta Cohesión, Bajo Acoplamiento
- Alta cohesión: agrupa funcionalidades relacionadas en un mismo módulo 
- Bajo acoplamiento: evita dependencias innecesarias entre módulos
## Principios para código limpio y mantenible
### Tell, Don't Ask (TDA)
Dile al objeto qué hacer en lugar de preguntar En lugar de recuperar datos de un objeto para operar sobre ellos en otro lugar, encapsula la lógica en el objeto mismo.
### Single Level of Abstraction Principle
Mantén cada método a un solo nivel de abstracción
![[Pasted image 20250219161138.png]]
### Principios para diseño de APIs
### Diseña la API antes de implementarla
Define primero la API antes de desarrollar la lógica interna del sistema Permite que los equipos frontend, backend y clientes externos trabajen en paralelo con una especificación bien definida
### Principios de REST
Aplica las convenciones de REST para diseñar APIs coherentes y fáciles de entender Hace que las APIs sean más predecibles, reutilizables y fáciles de consumir.
#### Principios clave:
- Uso de recursos y verbos HTTP adecuados (GET, POST, PUT, DELETE)
- HATEOAS (Hypermedia As The Engine Of Application State) para guiar la interacción
- Stateless: cada petición debe contener toda la información necesaria
- Uso de códigos de estado HTTP correctos (e.g., 200 OK, 404 Not Found, 201 Created)
### APIs consistentes
Diseña APIs que mantengan una estructura homogénea en toda la aplicación Facilita la integración Reduce la curva de aprendizaje para los consumidores de la API
### Idempotencia
Garantiza que múltiples llamadas a la misma operación produzcan el mismo resultado Evita efectos secundarios no deseados en sistemas distribuidos
### Versionado de APIs
Garantiza que múltiples llamadas a la misma operación produzcan el mismo resultado Evita efectos secundarios no deseados en sistemas distribuidos.
### Paginación y filtros
Permite la manipulación eficiente de grandes volúmenes de datos Evita respuestas de la API demasiado grandes, mejorando el rendimiento
### Control de tasa y regulación
Protege la API contra abusos y sobrecarga de tráfico Evita ataques de denegación de servicio Garantiza equidad entre los usuarios. Ejemplo: Limitar número de peticiones por minuto.
### Seguridad en APIs
Protege la API contra ataques y accesos no autorizados Evita fugas de información y ataques como SQL Injection, XSS, CSRF. Ejemplo: OAuth, JWT, Validación de entrada: filtrar y sanitizar datos del usuario.
## Software seguro y protección de datos
### Seguridad desde el diseño
Integra la seguridad en todas las etapas del desarrollo de software, en lugar de añadirla después como un “parche” Minimiza vulnerabilidades explotables en producción
#### Principios clave
- Mínimo privilegio: sólo dar los permisos estrictamente necesarios
- Defensa en profundidad: implementar múltiples capas de seguridad para evitar un único punto de fallo
- Seguridad por defecto: un sistema debe ser seguro sin requerir configuraciones adicionales
- Fallar de forma segura: un fallo no debe comprometer la seguridad del sistema
- Cifrado de datos sensibles: usar algoritmos de cifrado, tanto en tránsito como en reposo
- Validación estricta de entrada: evitar inyecciones SQL, XSS y otros ataques
- Registro y monitoreo: detección proactiva de intentos de ataque mediante logs y auditorías
### Privacidad desde el diseño
Garantiza que la privacidad y la protección de datos estén integradas en el diseño del sistema desde el principio, en lugar de ser añadidas como una solución tardía Cumplimiento con normativas como GDPR y CCPA, entre otras Reducción del impacto de brechas de datos
#### Principios clave
- Minimización de datos: sólo recolectar la cantidad mínima de información necesaria
- Anonimización/Pseudonimización: reducir el riesgo al separar datos personales de identificadores
- Transparencia: informar a los usuarios sobre cómo se recopilan, procesan y almacenan sus datos
- Control del usuario: permitir que los usuarios gestionen sus datos (eliminar, modificar, descargar)
- Seguridad de datos: proteger la información contra accesos no autorizados mediante cifrado y control de acceso
- Almacenamiento limitado: no conservar datos personales más tiempo del necesario