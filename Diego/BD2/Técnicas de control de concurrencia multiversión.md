NO ESTÁ EN LAS DIAPOS PERO ENTRA A EXAMEN
# Técnica de control de concurrencia multiversión basada en timestamps
Cuando llegan varias transacciones con varias operaciones, el SGBD asigna a cada operación un timestamp. 
- Se asigna a cada transacción un timestamp. Vamos a suponer que la transacción t tiene como timestamp t (t1 tendrá t1, t2 tendrá t2....
-  El objetivo es comprobar si un plan de ejecución es equivalente al plan serial t1,t2,t3... En caso de no ser equivalente se aborta una de las transacciones y se vuelve a lanzar (con otro timestamp distinto porque si no obviamente el resultado sería el mismo).
## Ejemplo
t2: leer(g). t3: esc(g), t2: leer(g)


A cada gránulo (g) se le va a dar una marca de tiempo de lectura (MT_lec) y otra de escritura (MT_esc).



|         | inicio | t2:leer(g) | t3:esc(g) |     | t2:leer(g) |     |
| ------- | ------ | ---------- | --------- | --- | ---------- | --- |
| MT_lec  | 0      | 2          | 2         | 0   | 2          | 0   |
| MT_esc  | 0      | 0          | 0         | 3   | 0          | 3   |
| version | v0     | v0         | v0        | v3  | v0         | v3  |

leer(t, g):
	k= versión de g con MT_esc más alta <= t
		leer(g, k)
		MT_lec(g, k) = max(MT_lec(g, k), t)


esc(t, g):
	k= versión de g con MT_esc más  alta <= t (version es columna entera)
	si MT_lec(g, k) <= t
		esc(g)
		insertar_nueva_version(MT_lec = 0, MT_esc = t, vt)
	sino abortar


leer(t2,g):
	k = 0
	leer(g,0)
	MT_lec(g, 0) = 2





# Planes serializables
t1: o11, o12,...
.
.
.
tn: on1,...

Plan ejecución: o12,o13....
¿es serializable? -> ¿es plan serial?




# Otro ejemplo
t4: esc(g) ; t2: est(g) ; t3: leer(g) ; t2: leer(g) ; t1:leer(g)
Orden: 't1<t2<t3' 

Para cada gránulo g, tenemos guardada una marca MT_lec, una marca MT_esc y la versión que tiene.

Se empieza con todo a 0.

leer(t, g):
	k= versión de g con MT_esc más alta <= t
		leer(g, k)
		MT_lec(g, k) = max(MT_lec(g, k), t)




esc(t, g):
	k= versión de g con MT_esc más  alta <= t (version es columna entera)
	si MT_lec(g, k) <= t
		esc(g)
		insertar_nueva_version(MT_lec = 0, MT_esc = t, vt)
	sino abortar

|         | inicio | t4:esc(g) |     | t2:esc(g) |     |     | t3:leer(g) |     |     | t2:leer(g) |     |     | t1:leer(g) |     |     |
| ------- | ------ | --------- | --- | --------- | --- | --- | ---------- | --- | --- | ---------- | --- | --- | ---------- | --- | --- |
| MT_lec  | 0      | 0         | 0   | 0         | 0   | 0   | 0          | 0   | 3   | 0          | 0   | 3   | 1          | 0   | 3   |
| MT_esc  | 0      | 0         | 4   | 0         | 4   | 2   | 0          | 4   | 2   | 0          | 4   | 2   | 0          | 4   | 2   |
| version | v0     | v0        | v4  | v0        | v4  | v2  | v0         | v4  | v2  | v0         | v4  | v2  | v0         | v4  | v2  |


Consideraciones:
- t3:leer(g): lees v2 por ser la MT_esc mas alta por debajo de 3. Cambiamos MT_lec por 3 al ser la maxima entre 0 y 3.
- t2:leer(g): lees v2 por ser la MT_esc mas alta por debajo de 2. Cambiamos MT_lec por 3 al ser la maxima entre 2 y 3 (la mantenemos como antes).
- t1:leer(g): lees v0 por ser la MT_esc mas alta por debajo de 1. Cambiamos MT_lec por 1 al ser la maxima entre 0 y 1



# Nueva técnica
Es pesimista y se basa en bloqueos (bloqueo quiere decir que entre las operaciones de transacciones se pueden meter bloqueos (es como un paso de testigo donde quien tegna el bloqueo de lectura quiere decir que los demas solo pueden leer, no escribir. Con bloqueo de escritura nadie puede hacer ni lectura ni escritura))




| T1          | T2                               |
| ----------- | -------------------------------- |
| bloq_lec(g) | 'T2 puede leer pero no escribir' |
| leer(g)     |                                  |
| desbloq(g)  | 'T2 ya puede escribir'           |

La técnica se llama TÉCNICA DE BLOQUEO EN DOS FASES. Si la transaccion empieza a pedir bloqueos y despues de generar el primer desbloqueo desbloquea los demás bloqueos, eso nos garantiza que va a haber un plan serial. La estructura es 'bloqueo bloqueo bloqueo' y luego 'desbloqueo desbloqueo desbloqueo', es decir, una vez desbloqueo por primera vez ya no puedo bloquear



CONSIDERACIONES EXAMEN
- De JPA lo mas jugoso para examen es JPQL.
- De la parte de Big Data y bases de datos NoSQL, hablamos de para qué sirven (para analizar datos y guardar cantidades grandes de datos). También hablamos que de las 3 propiedades que se pueden analizar de un sistema de informacion (teorema CAP), de esas 3 hay que priorizar 2. Los productos NoSQL priorizan la tolerancia a particiones. MongoDB, HBase, Cassandra son ejemplos de NoSQL. También vimos clasificacion de arquitecuras NoSQL (por columnas, documento y esas cosas). Aparte de la teoria estaba el MapReduce que permitia redistribuir informacion de varios ficheros y realizar un calculo en base a la información analizada. MapReduce es jugoso para examen
- De concurrencia las 4 técnicas de la teoría y la única tecnica practicada es la de control de multiversion (lo que no estaba en diapos pero explicó en la pizarra). Muy jugoso para examen
- Examen: 4 preguntas cortas (3 de Jordi, 1 de Ilarri) y 6 puntos de diseñar esuqema E/R (2 puntos), esquema relacional (2 puntos) y definir arquitectura o definir objetoRelacional(2 puntos). 