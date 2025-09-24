Internet: Conjunto mundial de redes interconectadas con protocolos comunes (TCP/IP) y un direccionamiento universal (IP).
# Direccionamiento
La asignación de direcciones se realiza mediante IANA.
- Tenemos 32 bits con 8 bits por grupo (4 grupos)
- Tenemos tanto identificador de red (primeros x bits) como identificador de nodo (últimos bits después de los x primeros)
Antes, se distinguían las redes por CLASSFUL, que distingue entre clase A (primeros 8 bits), clase B (primeros 16 bits), clase C (primeros 24 bits)...
Es decir, el encaminamiento CLASSFUL determinal a dirección de la red por sus primeros bits.

En el caso del encaminamiento CLASSLESS, este emplea la máscara de bits, que define el tamaño de la res y el número de hosts posibles. A partir de la máscara se pueden emplear subredes (poner un bit extra de máscara para no desaprovechar direcciones que no se van a utilizar) y superredes (quitar un bit por falta de direcciones y necesidad de conseguir más).
Es decir, el encaminamiento CLASSLESS aprovecha mejor las direcciones dado que la máscara es mucho más moldeable que las clases que emplea CLASSFUL

# Encaminamiento
Función: Definir las rutas de camino para llegar a un destino
- Encaminamiento directo: Llegar a un nodo de tu propia red
- Encaminamiento indirecto: Llegar a un nodo que no pertenece a tu red. Tabla de encaminamiento indirecto:
![[Pasted image 20250912111514.png]]

## Comportamiento Host
- Cuando llega una trama que no es para él, la descarta.
- Cuando quiere mandar una trama a un nodo de su red, revisa su tabla ARP para mandarlo directamente
- Cuando quiere mandar una trama a un nodo de otra red, lo manda a su router por defecto.
## Comportamiento Router
- Cuando le llega una trama que no es para él, intenta reenviarlo consultando la tabla de rutas
- Si tiene que reenviar la trama a su propia red, revisa la tabla ARP y lo manda.
- Si no pertenece a su red, consulta la tabla de encaminamiento:
	- Si hay una única entrada, la manda por esa entrada
	- Si hay 2, la manda por aquella con métrica menor
	- Si no hay, la manda al router por defecto.
## Ejemplo Tabla encaminamiento
![[Pasted image 20250912112007.png]]
## Construcción tabla de encaminamiento
- Encaminamiento estático: Se construye la tabla de encaminamiento al principio de la construcción de la red
- Encaminamiento dinámico: Se construye al inicio y se va actualizando cuando hace falta.

# Funcionalidad de IPv4

## 1. No orientado a conexión – *Best effort*
- IP es un protocolo **sin conexión** (connectionless).
  - No hay establecimiento previo (como en TCP).
  - No guarda estado: cada paquete se trata de forma independiente.
- **QoS Best effort**:
  - No hay garantías de entrega.
  - No hay garantías de orden.
  - No hay garantías de tiempo de llegada.
- La fiabilidad se consigue en capas superiores (ejemplo: TCP).

---

## 2. Tamaños de los datagramas
- **Máximo:** 65.535 bytes (limitación por campo de 16 bits).
- **Mínimo:** 28 bytes (20 de cabecera + 8 de fragmento mínimo).
- **Cabecera variable:**
  - Mínimo 20 bytes.
  - Hasta 60 bytes si incluye *Opciones*.

---

## 3. Campos de la cabecera IP (IPv4)
![[Pasted image 20250912113011.png]]

### Fila 1
- **Versión (4 bits):** IPv4 o IPv6.
- **Longitud de cabecera (IHL, 4 bits):** en múltiplos de 4 bytes (mínimo = 20 bytes).
- **Tipo de servicio (TOS, 8 bits):** tipo de servicio deseado.
- **Longitud total (16 bits):** tamaño del datagrama (cabecera + datos).

### Fila 2
- **Identificación (16 bits):** número que identifica al datagrama (útil para reensamblar fragmentos).
- **Flags (3 bits):** controlan la fragmentación.
  - `DF`: Don’t Fragment.
  - `MF`: More Fragments.
- **Offset de fragmento (13 bits):** posición del fragmento dentro del datagrama original.

### Fila 3
- **TTL (Time To Live, 8 bits):** número de saltos máximo (decrementa en cada router).
- **Protocolo (8 bits):** indica el protocolo de capa superior (TCP=6, UDP=17, ICMP=1...).
- **Checksum de cabecera (16 bits):** verificación de errores en la cabecera.

### Fila 4
- **Dirección IP de origen (32 bits).**
- **Dirección IP de destino (32 bits).**

### Fila 5+
- **Opciones (0–40 bytes):** información adicional (seguridad, timestamp, etc.).
- **Padding:** relleno para ajustar a múltiplos de 32 bits.

---

## 4. Funcionamiento general
1. El host genera el **datagrama IP** con la cabecera rellenada.
2. Cada router:
   - Lee la cabecera.
   - Decrementa el TTL.
   - Si fragmenta, ajusta identificación, offset y flags.
3. El destino usa la cabecera para recomponer y entregar los datos a la capa superior.

---


# NAT
Su función es traducir unas direcciones IP en otrasa de acuerdo a una tabla de equivalencias.
## ¿Para qué?
Hay varias razones:
- Limitación del número de IPs públicas.
- Por razones de seguridad: una dirección pública puede enmascarar una topología interna.
- Por pura gestión. Por ejemplo, el ISP (Internet Service Provider) asigna una IP pública al router de nuestra casa. Después, el router asigna dinámicamente direcciones IP privadas a todos los equipos de casa conectados ese router.
## Tipos de NAT
![[Pasted image 20250912114321.png]]
![[Pasted image 20250912114908.png]]
![[Pasted image 20250912114917.png]]
![[Pasted image 20250912114943.png]]
![[Pasted image 20250912114954.png]]
# Protocolos de encaminamiento
Hay una serie de algoritmos que permiten calcular el camino óptimo y, por tanto, definir la interfaz de salida.
Cuando el encaminamiento es dinámico y la topología de la red cambia, al tiempo que tardan los router en recalcular las rutas y actualizar las tablas de encaminamiento le llamamos "Tiempo de convergencia".
Los protocolos consisten en propagar la información del encaminamiento y con la información recibida calcular el camino a cada destino. El hecho de que haya protocolos implicaría que todos los router del mundo tuvieran que usar el mismo (no es así). Por lo tanto, podemos utilizar Sistemas Autónomos (SA), donde los router que estén dentro usan el mismo protocolo de encaminamiento y los router que estén fuera pueden usar uno distinto.
![[Pasted image 20250919104315.png]]
La información de encaminamiento entre 2 AS oculta la info del encaminamiento interno, por lo que los caminos pueden no ser los óptimos. Por ello, existen acuerdos bilaterales para establecer caminos directos entre los AS (incluso se pueden poner puntos de interconexión neutros para unir más de 2 AS).
![[Pasted image 20250919110421.png]]
## Protocolo RIP
Consiste en que cada x tiempo periódigo (30 segundos parece ser), el router manda info a todos sus vecino con su tabla completa (destino, coste), donde coste es el número de saltos hasta llegar al destino (el máximo es 16). Es ideal para redes pequeñas, pero es poco escalable
![[Pasted image 20250919110839.png]]
![[Pasted image 20250919111945.png]]
Ejemplo:
![[Pasted image 20250919111846.png]]
![[Pasted image 20250919111904.png]]
![[Pasted image 20250919111916.png]]
![[Pasted image 20250919112004.png]]
![[Pasted image 20250919112057.png]]
![[Pasted image 20250919112109.png]]
![[Pasted image 20250919112121.png]]
## Protocolo OSPF
No entra aparentemente. Se va a utilziar el otro en las practicas para capturar las tramas, osea que probablemente no sea de mucha importancia para el examen.