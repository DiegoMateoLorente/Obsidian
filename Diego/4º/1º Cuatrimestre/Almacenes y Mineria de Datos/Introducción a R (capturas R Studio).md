# Vectores
![[Pasted image 20250912131718.png]]
# Matrices
![[Pasted image 20250912132310.png]]
**IMPORTANTE**: runif(numNumeros,min,max) devuelve numNumeros aleatorios en el rango (min,max). floor lo que hace es truncar el numero (si es 5.6 es 5 y así).
![[Pasted image 20250912132826.png]]

# Tipos de datos básicos en R
- integer (int): Se tiene que poner L al lado del numero.
- numeric (num): No se pone L al lado del numero.
- characters (chr): Los elementos entre doble comilla "".
- logical (logi): es booleano (T,F), aunque tambien sirve TRUE,  FALSE.
- factor: grupos categóricos cualitativos de datos
![[Pasted image 20250912133841.png]]

# Operaciones entre conjuntos
![[Pasted image 20250912134140.png]]
![[Pasted image 20250912134720.png]]
**Detalle**: A la derecha de la terminal aparece una pestaña de data para ver los valores de los conjuntos creados en memoria.
![[Pasted image 20250912134628.png]]
![[Pasted image 20250912135014.png]]
**DETALLE**: He puesto un ejemplo del view con v para ver lo que se muestra con vectores. Con matrices se muestra como si fuera una matriz (como una tabla)

# Segunda sesión de explicación R
getwd(): Get Working Directory
![[Pasted image 20251017130941.png]]
![[Pasted image 20251017131158.png]]
Aqui guardamos el fichero desde Moodle y lo cargamos en datos. Son 200 filas y 5 columnas. El fichero es de tipo frame. Las columnas son X, TV, radio, newspaper y sales. Las filas son los valores de cada columna.
![[Pasted image 20251017131441.png]]
![[Pasted image 20251017131739.png]]
![[Pasted image 20251017131812.png]]
Son 100 filas pero no cabe
![[Pasted image 20251017131933.png]]
R permite eliminar  columnas de esta forma
![[Pasted image 20251017132056.png]]
![[Pasted image 20251017132300.png]]
![[Pasted image 20251017132341.png]]
![[Pasted image 20251017132843.png]]
File > New File > R Script crea un script de R. Se ejecuta en el boton Source
![[Pasted image 20251017133337.png]]
![[Pasted image 20251017133537.png]]
![[Pasted image 20251017133647.png]]
Ha multiplicado por 2 todos los numeros porque ninguno es 2 exactamente
![[Pasted image 20251017133957.png]]
CTRL+ Shift + C comenta lo que seleccionas