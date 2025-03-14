create type CursoUdt as (numCurso integer,
	descriptión varchar(300),
	refAsignaturas ref(asignaturaUdt) array[10]) scope Asignatura references are checked on delete set null)
instantiable not final ref is system generated;


create type AsignaturaUdt as (nombreAsign varchar(30),
	numCurso integer,
	description varchar(300),
	refCurso ref(cursoUdt) scope Curso references are checked on delete cascade)
instantiable not final ref is system generated;

create table Curso of CursoUdt(
	primary key(numCurso),
	description with options not null,
	ref is CursoID system generated
)

create table Asignatura of AsignaturaUdt(
	primary key (nombreAsign, numCurso),
	foreign key (numCurso) references Curso(numCurso) on delete cascade,
	description with options not null,
	refCurso with options not null,
	ref is AsignaturaID system generated
)