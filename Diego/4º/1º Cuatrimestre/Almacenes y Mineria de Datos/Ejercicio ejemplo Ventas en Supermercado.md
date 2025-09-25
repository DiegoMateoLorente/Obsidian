Hay que seguir el proceso de Kimball:
# 1. Seleccionar el proceso de negocio
El proceso de negocio propuesto por Ilarri y el correcto es el proceso de VENTAS del supermercado. Con ese proceso de negocio podemos hacer un análisis / explotación de los datos. Otro proceso de negocio es el de COMPRAS (compra del stock, proveedores). La ultima podría ser la GESTION DE INVENTARIO

Ahora hay que seleccionar el principal proceso de negocio en cuanto: impacto, explotación de datos y la dificultad de análisis. En cuanto a impacto, la opción ideal sería el proceso de ventas.
Nos centraremos en: Qué productos se venden, en qué tiendas, qué dias, bajo qué condiciones promocionales... 

Según la metodología de Kimball, un proceso de negocio es lo mismo que un data mart (repositorio analítico donde se puede tratar con los datos de un proceso de negocio).

# 2 Declarar el grano/gránulo
El gránulo más fino podría ser la cantidad de cada producto vendido en cada hora (incluyendo también la fecha) y en cada tienda. (es la opción propuesta por Jordan y yo y está bien).

Según Ilarri, una frase todavía mas simple sería (cada línea de cada ticket de compra).

# 3. Escoger las dimensiones

Estas serian las dimensiones primarias:
	SId.Producto -> Supermercado -> Fecha -> Hora -> Terminal(donde se ha realizado la venta)

Las secundarias podrían ser:
	Cliente -> Promoción


Propuesta de Ilarri:
Primarias:
	Producto y numero de ticket

Secundarias:
	Tiempo y promoción


Importante: Además de identificar las dimensiones, también hay que identificar los tipos (primario, secundario).

# 4. Identificar los hechos/métricas

Cantidad comprada, Precio total




# IMPORTANTE
En los esquemas de Kimball, no se usan booleanos ni tampoco enteros (para simular booleanos), si no que se  crean tipos numerados (festivo, no festivo por ejemplo).