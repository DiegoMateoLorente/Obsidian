-- TIPOS ROW PARA PROCEDIMIENTOS (NO PARA TABLAS)
CREATE TYPE ClienteUdt AS ROW (
    DNI        VARCHAR(255),
    Edad       INT,
    Nombre     VARCHAR(255), 
    Apellidos  VARCHAR(255), 
    Email      VARCHAR(255), 
    Direccion  VARCHAR(255), 
    Telefono   VARCHAR(255)
);

CREATE TYPE CuentaUdt AS ROW (
    IBAN            VARCHAR(255), 
    Numero_Cuenta   INT, 
    Saldo           FLOAT,
    Fecha_Creacion  DATE
);

CREATE TYPE OperacionBancariaUdt AS ROW (
    Descripcion     VARCHAR(255),
    Codigo_Numerico INT,
    Hora           TIME,
    Fecha          DATE,
    Cantidad       FLOAT,
    Tipo           VARCHAR(255),
    Cuenta_IBAN    VARCHAR(255)
);

CREATE TYPE TieneUdt AS ROW (
    Cliente_DNI VARCHAR(255),
    Cuenta_IBAN VARCHAR(255)
);

-- TABLAS TRADICIONALES (SIN ROW TYPE)
CREATE TABLE Cliente (
    DNI        VARCHAR(255) PRIMARY KEY,
    Edad       INT NOT NULL,
    Nombre     VARCHAR(255) NOT NULL,
    Apellidos  VARCHAR(255) NOT NULL,
    Email      VARCHAR(255),
    Direccion  VARCHAR(255) NOT NULL,
    Telefono   VARCHAR(255) NOT NULL
);

CREATE TABLE Cuenta (
    IBAN            VARCHAR(255) PRIMARY KEY,
    Numero_Cuenta   INT NOT NULL,
    Saldo           FLOAT NOT NULL,
    Fecha_Creacion  DATE NOT NULL
);

CREATE TABLE CuentaAhorro (
    IBAN      VARCHAR(255) PRIMARY KEY,
    Interes   FLOAT NOT NULL,
    FOREIGN KEY (IBAN) REFERENCES Cuenta(IBAN)
);

CREATE TABLE CuentaCorriente (
    IBAN      VARCHAR(255) PRIMARY KEY,
    FOREIGN KEY (IBAN) REFERENCES Cuenta(IBAN)
);

CREATE TABLE Operacion_Bancaria (
    Codigo_Numerico INT,
    Cuenta_IBAN     VARCHAR(255),
    Descripcion     VARCHAR(255),
    Hora           TIME NOT NULL,
    Fecha          DATE NOT NULL,
    Cantidad       FLOAT NOT NULL,
    Tipo           VARCHAR(255) NOT NULL,
    PRIMARY KEY (Codigo_Numerico, Cuenta_IBAN),
    FOREIGN KEY (Cuenta_IBAN) REFERENCES Cuenta(IBAN)
);

CREATE TABLE Transferencia (
    Codigo_Numerico       INT,
    Cuenta_IBAN_Emisor    VARCHAR(255),
    Cuenta_IBAN_Receptor  VARCHAR(255),
    PRIMARY KEY (Codigo_Numerico, Cuenta_IBAN_Emisor, Cuenta_IBAN_Receptor),
    FOREIGN KEY (Codigo_Numerico, Cuenta_IBAN_Emisor) REFERENCES Operacion_Bancaria(Codigo_Numerico, Cuenta_IBAN),
    FOREIGN KEY (Cuenta_IBAN_Receptor) REFERENCES Cuenta(IBAN)
);

CREATE TABLE Sucursal (
    Codigo                INT PRIMARY KEY,
    Direccion             VARCHAR(255) NOT NULL,
    Telefono              INT NOT NULL,
    Cuenta_corriente_IBAN VARCHAR(255) NOT NULL,
    FOREIGN KEY (Cuenta_corriente_IBAN) REFERENCES CuentaCorriente(IBAN)
);

CREATE TABLE Retirada (
    Codigo_Numerico INT,
    Cuenta_IBAN     VARCHAR(255),
    Sucursal_Codigo INT,
    PRIMARY KEY (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo),
    FOREIGN KEY (Codigo_Numerico, Cuenta_IBAN) REFERENCES Operacion_Bancaria(Codigo_Numerico, Cuenta_IBAN),
    FOREIGN KEY (Sucursal_Codigo) REFERENCES Sucursal(Codigo)
);

CREATE TABLE Ingreso (
    Codigo_Numerico INT,
    Cuenta_IBAN     VARCHAR(255),
    Sucursal_Codigo INT,
    PRIMARY KEY (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo),
    FOREIGN KEY (Codigo_Numerico, Cuenta_IBAN) REFERENCES Operacion_Bancaria(Codigo_Numerico, Cuenta_IBAN),
    FOREIGN KEY (Sucursal_Codigo) REFERENCES Sucursal(Codigo)
);

CREATE TABLE Tiene (
    Cliente_DNI VARCHAR(255),
    Cuenta_IBAN VARCHAR(255),
    PRIMARY KEY (Cliente_DNI, Cuenta_IBAN),
    FOREIGN KEY (Cliente_DNI) REFERENCES Cliente(DNI),
    FOREIGN KEY (Cuenta_IBAN) REFERENCES Cuenta(IBAN)
);
