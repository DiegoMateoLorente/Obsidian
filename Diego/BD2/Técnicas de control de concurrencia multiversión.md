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
| version | v0     | v0         | v0        | v3  | v0         | v3´ |

leer(t, g):
	k= versión de g con MT_esc más alta <= t
		leer(g, k)
		MT_lec(g, k) = max(MT_lec(g, k), t)


esc(t, g):
	k= versión de g con MT_esc más  alta <= t
	si MT_lec(g, k) <= t
		esc(g)
		insertar_nueva_version(MT_lec = 0, MT_esc = t, vt)
	sino abortar


leer(t2,g):
	k = 0
	leer(g,0)
	MT_lec(g, 0) = 2