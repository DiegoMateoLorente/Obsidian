# Objetivo

- El **Broker** es un patrón arquitectural para **sistemas distribuidos**.
- Desacopla componentes que interactúan mediante **invocaciones de servicios remotos**.
- Coordina, dirige peticiones, transmite resultados y excepciones.

# Ejemplo

- Sistema de Información al Ciudadano (CIS) con servicios sobre restaurantes, hoteles, transporte, etc.
- Clientes (usuarios móviles) acceden a los servicios sin conocer su ubicación.
- Los servicios deben estar **desacoplados** entre sí y respecto al cliente.

# Contexto

- Entorno distribuido y heterogéneo:
  - Lenguajes de programación diversos
  - Distintos tipos de servidores
  - Diferentes sistemas operativos

# Descripción del problema

- Evitar aplicaciones monolíticas: queremos aplicaciones **flexibles, mantenibles y extensibles**.
- El sistema debe permitir **cambios en tiempo de ejecución** y ocultar la localización de los servicios.
- No debe depender de tecnologías concretas → **portabilidad e interoperabilidad**.

# Problemas identificados

- Si los componentes gestionan directamente la comunicación:
  - Aumenta la complejidad
  - Se pierde portabilidad
  - Se requiere conocer la localización de los servicios

# Solución

- Introducir un **Broker** como intermediario:
  - Los servidores se **registran** en el Broker
  - Los clientes invocan servicios a través del Broker
- Tareas del Broker:
  - Registrar servicios
  - Localizar servidores
  - Transmitir peticiones y resultados
  - Activar servidores si están inactivos

# Ventajas del Broker

- Abstracción de la comunicación → el cliente sólo invoca métodos
- Los objetos pueden añadirse, cambiarse o eliminarse dinámicamente
- Transforma un sistema distribuido en uno más **transparente y modular**

# Estructura del patrón

- Componentes:
  - **Clientes**
  - **Servidores**
  - **Broker**
  - **Proxies de cliente**
  - **Proxies de servidor**
  - **Puentes** (opcionales)

# Componentes detallados

## Servidores

- Ofrecen servicios a través de interfaces (IDL o binarias)
- Registrados en el Broker
- Tipos:
  - Específicos de dominio (ej. mapas)
  - Generales (ej. pagos online)

## Clientes

- Envían peticiones al Broker, no al servidor directamente
- No necesitan conocer la ubicación del servidor

## Broker

- Encargado de transmitir mensajes entre cliente y servidor
- Ofrece APIs para registro/invocación
- Puede activar servidores inactivos
- Tipos:
  - **Indirecto**: media durante toda la interacción
  - **Directo**: solo intermedia al inicio

# Proxies

## Cliente

- Simulan que el servicio es local
- Hacen *marshaling* y *unmarshaling* de datos

## Servidor

- Interceptan y traducen peticiones
- Simulan que el cliente está localmente

# Puentes (opcional)

- Permiten **interoperar** Brokers heterogéneos
- Encapsulan diferencias de red, sistema operativo o protocolo
- Se comunican mediante protocolos comunes



# Dinámica del patrón
![[Pasted image 20250506115107.png]]

## Escenario I: Registro de un servidor

- El servidor se registra en el Broker
![[Pasted image 20250506115125.png]]

## Escenario II: Petición desde cliente

- El cliente realiza una invocación (síncrona o asíncrona)
![[Pasted image 20250506115200.png]]

## Escenario III: Comunicación entre Brokers

- Brokers colaboran usando **puentes**
![[Pasted image 20250506115221.png]]

# Consecuencias del patrón

## Beneficios

- **Transparencia en la localización** de servidores
- **Interoperabilidad** entre Brokers (mediante puentes)
- **Reutilización** de servicios existentes
- **Extensibilidad** y **reemplazo** de componentes sin afectar al resto

## Inconvenientes

- **Rendimiento menor** que arquitecturas sin intermediarios
- **Menor tolerancia a fallos**: si falla un Broker o servidor, afecta a todos
- **Depuración compleja**: el Broker introduce indirectas difíciles de seguir
