# Documentación Cliente/Servidor (C/S)

## Estilos arquitecturales relacionados

- Filtro y Tubería (Pipe-and-Filter)
- Cliente-Servidor (Client-Server)
- Peer-to-Peer (P2P)
- Arquitectura Orientada a Servicios (SOA)
- Publicador-Subscriptor (Publish-Subscribe)
- Datos Compartidos (Shared-Data)

---

## Tipos e instancias

- Las guías arquitecturales definen **tipos abstractos** de componentes y conectores.
- Estos tipos deben especializarse para un sistema concreto.
- La especialización puede ser:
  - del **dominio del problema**
  - o **tecnológica**

---

## Objetivo del documento

- Presentar una guía de estilo arquitectural para la **vista de Componentes y Conectores (C&C)** en sistemas C/S.
- Introducir:
  - Tipos abstractos de componentes y conectores
  - Reglas de combinación entre ellos

---

## Clasificación de arquitecturas

Basada en el **modelo computacional**:

- **Call-return**: interacción síncrona
- **Data flow**: computación guiada por el flujo de datos
- **Event-based**: interacción asincrónica
- **Repositorio**: uso de datos compartidos

Adicionalmente, consideraremos también categorías transversales: **niveles o tiers**

---

## Estilo Call-Return

- Componentes proveedores de servicios que otros pueden invocar.
- Los conectores gestionan:
  - envío de peticiones
  - retorno de resultados

---

## Modelo Cliente-Servidor

- Arquitectura dividida en **capas lógicas** y **niveles**
- Flujo de datos y control bien definido
- Define tipos abstractos de:
  - **Componentes**: cliente, servidor, mixtos
  - **Conectores**: `request-reply`

---

## Elementos del modelo C/S

### Componentes
- **Clientes**: consumen servicios
- **Servidores**: proveen servicios
- Pueden coexistir servidores centralizados o distribuidos

### Conectores
- Tipo: `request-reply`
- Atributos a documentar:
  - Protocolo (soporte de múltiples servicios)
  - Local vs remoto
  - Encriptación
  - Manejo de errores
  - Inicio/cierre de conexiones
  - Soporte de sesiones
  - Descubrimiento de servidores
  - Middleware usado (si aplica)

---

## Puertos

- Los componentes tienen **puertos** que describen los servicios que ofrecen o requieren.
- Atributos a documentar:
  - Número de clientes soportados
  - Prestaciones (tasa de invocación)

---

## Características del estilo C/S

- Flujo **asimétrico**: el cliente inicia la interacción.
- Invocación del servicio es **síncrona**.
- Variantes:
  - Conectores donde el **servidor inicia** ciertas acciones en el cliente.

Ventajas:
- **Modificabilidad y reutilización** (por servicios bien definidos)
- **Escalabilidad y disponibilidad** de componentes

---

## Tipos de análisis

- **Seguridad**: control de acceso a servicios
- **Prestaciones**: capacidad de respuesta de los servidores
- **Fiabilidad**: capacidad de recuperación ante fallos

---

## Ejemplo de conector

- Cliente abre conexión (IP + puerto).
- Envía una petición no bloqueante → mensaje "Please wait...".
- Invoca operación en el servidor.
- Los mensajes de envío/recepción son pasos separados.
- Conector maneja:
  - timeouts
  - errores
  - correlación de mensajes
- Hoy en día es más común el uso de **HTTP** que de **sockets TCP**.

---

## Documentación de niveles (tiers)

- Aplica también fuera del modelo C/S.
- Agrupación jerárquica de componentes por función o naturaleza (siguiendo algún criterio).
- Cada grupo (agrupación) se denomina **nivel**.
- Restricción:
  - Solo se permiten conectores dentro del mismo nivel o entre niveles **adyacentes**.

---

