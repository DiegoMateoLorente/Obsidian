mPara el dataset Advertising.csv:
![[Pasted image 20251106131534.png]]

Tenemos que TV, radio y newspaper es la inversion (en miles de dolares) realizada en esos servicios, y sales es el beneficio en miles de dólares. 
TV, radio y newspaper son variables predictoras(independientes) y sales es una variable respuesta (dependiente)
Tenemos la funcion:
Y = f(X) + epsilon
sales = F(TV,radio,newspaper) + epsilon (error impresecible)
Por lo tanto: Y = Respuesta, X = Variables predictoras

En la regresión lineal supondremos que en nuestro modelo f(X) es lineal en los predictores X = (X1, . . . , Xp):
Y = β0 + β1X1 + · · · + βpXp + epsilon
Los coeficientes β0, β1, . . . , βp: coeficientes de regresi´on
Usando los datos de entrenamiento, hallaremos una estimaci´on de los coeficientes de regresi´on: βˆ 0, βˆ 1, . . . , βˆ p

Por tanto tendremos una estimaci´on de la funci´on de regresi´on: f(X) ≈ ˆf(X) = βˆ 0 + βˆ 1X1 + · · · + βˆ pXp

Podremos realizar predicciones: para el dato x, la predicci´on es yˆ yˆ = ˆf(x)

model = lm(datos\$sales~datos\$TV)
esto es semejante a sales = β0 + β1TV 
![[Pasted image 20251106133734.png]]
Ahora con estos datos nos toca evaluar si es un buen modelo.

![[Pasted image 20251106133947.png]]![[Pasted image 20251106133959.png]]
abline nos dibuja la mejor recta para este modelo
Ahora nos toca validar el modelo, es decir, cómo ver si es bueno o malo.

Nos vamos a hacer las siguientes preguntas:
- ¿Los coeficientes estimados βˆ i son “buenos”?
- ¿El modelo lineal explica “bien” los datos de entrenamiento?
![[Pasted image 20251106134145.png]]
Aunque no se cumplan estas condiciones, seguimos para delante (estamos suponiendo que se cumplen)

![[Pasted image 20251107131023.png]]
Dato, el β0 puede llegar a manipularse para que sea 0, si a todos los datos (puntos) les resto β0.



Cuando hagamos summary, mirar p valor por si es demasiado alto y se tengan qiue descartar los datos (no sirven). Luego mirar el t valor de las entradas y ver si tambien son altos para descartarlos. Por ultimo, mirar el R cuadrado. Va de 0 a 1 Y quiere decir el porcentaje de la variabilidad en que influye la entrasda. Si vale 0.9, quiere decir que el 90% de la variabilidad de ventas viene explicado por la entrada, por lo uqe la entrada influye en la salida).
El mejor modelo es el que tiene mejores resultados con menos predictores.

# Nueva sesión 13-11-2025
Cuando un beta1 = 0.04, quiere decir que afecta un 4% en la salida. Pero tengo que definir cuánto de seguro estoy de eso. De ahi sacamos el intervalo de confianza. Un intervalo de confianza del 95% significa que si tuviesemos 100 datasets y sacasemos 100 intervalos de confianza como este, en 95 sacaremos el verdadero valor.

Hasta ahora, sabemos que podemos aplicar el modelo de regresión lineal cuando la salida es cuantitativa. Pero puede ocurriri que alguno de los predictores o la salida sean cualitativas.
Vamos a utilizar ahora otro dataset:
![[Pasted image 20251113123628.png]]
Como podemos ver, ahora uno de los predictores (smoke) es cualitativo.
Lo mas dificil en este tipo de estudios es tener una buena muestra.
Hay 2 formas de codificar variables cualitativas: Variables dummy (generar variables ficticias para cada uno de los niveles que tiene la variable cualitativa. Aqui tenemos 2 niveles (fuma/noFuma). Por lo tanto creamos una variable con 2 valores (para cada valor posible)) u otro modelovllamado one hot encoding (si yo tengo 3 niveles (bien/mal/regular), esto se convierte en 3 columnas (una para cada valor) y pone un 1 en donde corresponda) (de momento vamos con las dummy).
Ejemplo one-hot-encoding:
Si tenemos AP (apariencia) = B(bien) / M(mal) / R(regular), si en una fila vale M y otra B, se haría lo siguiente:

| AP = B | AP = M | AP = R |
| ------ | ------ | ------ |
| 0      | 1      | 0      |
| 1      | 0      | 0      |

datos$smoke = as.factor(datos$smoke)
Esto lo que hace es cambiar el valor de smoke por un factor de 2 niveles donde no vale 1 y yes vale 2.


RESUMEN
Cuantitativo - Cuantitativo : Regresion lineal
Cuantitativo - Cualitativo : Analisis de la varianza
Cualitativo - Cualitativo : Chi cuadrado