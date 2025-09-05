Asignaturas donde haya 1 alumno con el mismo nombre que el profesor:

String q5: "select distinct a" + "from ESTUDIANTE e, in (e.asignaturas) ea, " 
			+ "PROFESOR p, in (p.asignaturas) pa"
			+ "where ea.id =pa.id AND e.nombre = p.nombre"