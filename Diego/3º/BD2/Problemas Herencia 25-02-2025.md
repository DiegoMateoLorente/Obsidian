Espondor(esponsorId, presupuestoMa)

Persona(pId, nombre, tfno, dir, esponsorId,
foreign key (sponsorId) references Esponsor (esponsorId),
unique(esponsorId))

Empresa(empId, nombre, esponsorId,
foreign key (esponsorId) references Esponsor(esponsorId),
unique(esponsorId))

esponsorId is UNIQUE in Empresa+Persona