# Objetivo

- Desarrollar software adaptable a cambios en los requisitos.
- Separar los requisitos funcionales en:
  - Un núcleo mínimo e inmutable.
  - Extensiones adaptables a usuarios o contextos concretos.
- Crear una base extensible y portable para soportar nuevas funcionalidades.

# Ejemplo

- Sistema operativo que:
  - Se ejecuta en múltiples plataformas (portabilidad).
  - Integra nuevas tecnologías fácilmente (adaptabilidad).
  - Ejecuta aplicaciones de otros SO (extensibilidad).
- Cada sistema operativo se emula mediante un "servidor" que representa una vista del núcleo.

# Contexto

- Útil cuando:
  - Varias aplicaciones comparten una funcionalidad base.
  - Se necesita soportar múltiples plataformas o estándares desde una misma base.

# Descripción del problema

- Se requiere una plataforma capaz de:
  - Integrar múltiples tecnologías.
  - Evolucionar con el tiempo.
  - Ser portable, adaptable y extensible.
- Necesidad de múltiples "vistas" del núcleo para distintas aplicaciones.

# Solución del problema

- Encapsular la funcionalidad básica en un componente **microkernel**.
- El microkernel gestiona recursos y facilita la comunicación entre componentes.
- Las funcionalidades adicionales se implementan como:
  - **Servidores internos** (subsistemas).
  - **Servidores externos** (personalidades).

# Estructura del patrón

- Cinco componentes principales:
  - Microkernel
  - Servidores internos
  - Servidores externos
  - Clientes
  - Adaptadores

# Microkernel

- Componente más importante.
- Incluye solo los servicios esenciales:
  - Comunicación, gestión de procesos y memoria, I/O, etc.
- Encapsula dependencias hardware/software.
- Proporciona mecanismos básicos sobre los que construir políticas.

# Servidores internos

- Subsistemas que extienden el microkernel.
- Ejemplos: sistema de ficheros, autenticación, drivers gráficos.
- Pueden ser procesos activos o librerías pasivas.
- Solo accesibles a través del microkernel.

# Servidores externos

- "Personalidades" que implementan vistas específicas del dominio.
- Usan el microkernel para implementar políticas propias.
- Ejemplos:
  - Diferentes maneras de gestionar procesos en Unix vs Windows.
- Ejecutan en procesos separados y ofrecen APIs específicas.

# Aplicaciones cliente

- Se asocian a un único servidor externo.
- Deben acceder a él solo a través de su API.
- Para evitar dependencias con el microkernel, se usa un **adaptador**.

# Adaptadores

- Interfaz entre cliente y servidor externo.
- Simulan la API del servidor.
- Usan los servicios del microkernel.
- Ofrecen portabilidad: el cliente no sabe si se ejecuta sobre el sistema nativo o el microkernel.
- Pueden ser librerías estáticas o dinámicas.

# Dinámica del patrón

## Escenario I: Cliente a Servidor Externo

1. El cliente llama al adaptador.
2. El adaptador solicita al microkernel un canal hacia el servidor externo.
3. Se establece la comunicación y se transmite la petición.
4. El servidor procesa y responde.
5. El adaptador devuelve la respuesta al cliente.

## Escenario II: Servidor Externo a Interno

1. El servidor externo envía la petición al microkernel.
2. Este la redirige al servidor interno.
3. El interno responde y el microkernel devuelve la respuesta al servidor externo.

# Consecuencias del patrón

## Beneficios

- Portabilidad.
- Flexibilidad y extensibilidad.
- Separación entre mecanismo y política.
- Escalabilidad y fiabilidad.
- Transparencia frente a la plataforma.

## Inconvenientes

- Menor rendimiento (por la intermediación).
- Mayor complejidad en diseño e implementación.

