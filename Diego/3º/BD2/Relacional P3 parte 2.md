# V1
**DOMINIOS**
tpEsPremium = Boolean
tpNombre = cadena(50)
tpApellido = cadena(50)
tpFecha = Date
tpSaldo = 0..99999
tpPais = cadena(100)
tpNumAnuncios: 0..10
tpRenovacion = Boolean
tpNumHoras = Number
tpGeneracion = cadena(50)
tpLicencia = cadena(500)
tpPuntuacion = 0..10

**ESQUEMA DE RELACIÓN**
Compañia = (
		Nombre: tpNombre NO NULO;
		
)

Videojuego = (
		Nombre: tpNombre NO NULO;
		

);