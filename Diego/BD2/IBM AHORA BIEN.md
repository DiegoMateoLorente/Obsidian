CREATE TYPE ClienteUdt AS (
    DNI        VARCHAR(255),
    Edad       INT,
    Nombre     VARCHAR(255), 
    Apellidos  VARCHAR(255), 
    Email      VARCHAR(255), 
    Direccion  VARCHAR(255), 
    Telefono   VARCHAR(255)
) REF USING INT MODE DB2SQL;

-- Tipo padre
CREATE TYPE CuentaUdt AS (
    IBAN VARCHAR(255), 
    Numero_Cuenta INT, 
    Saldo FLOAT,
    Fecha_Creacion DATE
) REF USING INT MODE DB2SQL;

-- Hijos con herencia
CREATE TYPE CuentaAhorroUdt UNDER CuentaUdt AS (
    Interes FLOAT
) MODE DB2SQL;

CREATE TYPE CuentaCorrienteUdt UNDER CuentaUdt AS (
    limite_credito DECIMAL(15,2)
) MODE DB2SQL;



-- Tipo padre
CREATE TYPE OperacionBancariaUdt AS (
    Descripcion     VARCHAR(255),
    Codigo_Numerico INT,
    Hora            TIME,
    Fecha           DATE,
    Cantidad        FLOAT,
    Tipo            VARCHAR(255),
    Cuenta_IBAN     VARCHAR(255)
) REF USING INT MODE DB2SQL;

-- Subtipo Transferencia
CREATE TYPE TransferenciaUdt UNDER OperacionBancariaUdt AS (
    Cuenta_IBAN_Receptor VARCHAR(255)
) MODE DB2SQL;

-- Subtipo Retirada
CREATE TYPE RetiradaUdt UNDER OperacionBancariaUdt AS (
    Sucursal_Codigo INT
) MODE DB2SQL;

-- Subtipo Ingreso
CREATE TYPE IngresoUdt UNDER OperacionBancariaUdt AS (
    Sucursal_Codigo INT
) MODE DB2SQL;

CREATE TYPE SucursalUdt AS (
    Codigo                INT,
    Direccion             VARCHAR(255),
    Telefono              INT,
    Cuenta_corriente_IBAN VARCHAR(255)
) REF USING INT MODE DB2SQL;

CREATE TYPE TieneUdt AS (
    Cliente_DNI VARCHAR(255),
    Cuenta_IBAN VARCHAR(255)
) REF USING INT MODE DB2SQL;









-- Cliente (no hereda)
CREATE TABLE Cliente OF ClienteUdt
    (REF IS ref_cliente USER GENERATED,
     DNI WITH OPTIONS NOT NULL,
     PRIMARY KEY (DNI)
    );

-- Cuenta (padre)
CREATE TABLE Cuenta OF CuentaUdt
    (REF IS ref_cuenta USER GENERATED,
     IBAN WITH OPTIONS NOT NULL,
     PRIMARY KEY (IBAN)
    );

-- Cuenta Ahorro (hija)
CREATE TABLE CuentaAhorro OF CuentaAhorroUdt
    UNDER Cuenta
    INHERIT SELECT PRIVILEGES;

-- Cuenta Corriente (hija)
CREATE TABLE CuentaCorriente OF CuentaCorrienteUdt
    UNDER Cuenta
    INHERIT SELECT PRIVILEGES;

-- Sucursal
CREATE TABLE Sucursal OF SucursalUdt
    (REF IS ref_sucursal USER GENERATED,
     Codigo WITH OPTIONS NOT NULL,
     PRIMARY KEY (Codigo),
     FOREIGN KEY (Cuenta_corriente_IBAN) REFERENCES CuentaCorriente(IBAN)
    );

-- Operacion Bancaria (padre)
CREATE TABLE Operacion_Bancaria OF OperacionBancariaUdt
    (REF IS ref_operacion USER GENERATED,
     Codigo_Numerico WITH OPTIONS NOT NULL,
     Cuenta_IBAN WITH OPTIONS NOT NULL,
     PRIMARY KEY (Codigo_Numerico, Cuenta_IBAN),
     FOREIGN KEY (Cuenta_IBAN) REFERENCES Cuenta(IBAN)
    );

-- Transferencia (hija de Operacion_Bancaria)
CREATE TABLE Transferencia OF TransferenciaUdt
    UNDER Operacion_Bancaria
    INHERIT SELECT PRIVILEGES
    (FOREIGN KEY (Cuenta_IBAN_Receptor) REFERENCES Cuenta(IBAN));
    
-- Retirada (hija de Operacion_Bancaria)
CREATE TABLE Retirada OF RetiradaUdt
    UNDER Operacion_Bancaria
    INHERIT SELECT PRIVILEGES
    (FOREIGN KEY (Sucursal_Codigo) REFERENCES Sucursal(Codigo));


-- Ingreso (hija de Operacion_Bancaria)
CREATE TABLE Ingreso OF IngresoUdt
    UNDER Operacion_Bancaria
    INHERIT SELECT PRIVILEGES
    (FOREIGN KEY (Sucursal_Codigo) REFERENCES Sucursal(Codigo));

-- Tiene
CREATE TABLE Tiene OF TieneUdt
    (REF IS ref_tiene USER GENERATED,
     Cliente_DNI WITH OPTIONS NOT NULL,
     Cuenta_IBAN WITH OPTIONS NOT NULL,
     PRIMARY KEY (Cliente_DNI, Cuenta_IBAN),
     FOREIGN KEY (Cliente_DNI) REFERENCES Cliente(DNI),
     FOREIGN KEY (Cuenta_IBAN) REFERENCES Cuenta(IBAN)
    );

















--#SET TERMINATOR @

CREATE TRIGGER check_balance_before_transfer
NO CASCADE BEFORE INSERT ON Transferencia
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   DECLARE v_saldo DECIMAL(15,2);
   SET v_saldo = (SELECT Saldo FROM Cuenta WHERE IBAN = N.Cuenta_IBAN);
   IF v_saldo < N.Cantidad THEN
      SIGNAL SQLSTATE '75001' SET MESSAGE_TEXT = 'Saldo insuficiente para realizar la transferencia.';
   END IF;
END@

CREATE TRIGGER update_balance_after_ingreso
AFTER INSERT ON Ingreso
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   UPDATE Cuenta SET Saldo = Saldo + N.Cantidad WHERE IBAN = N.Cuenta_IBAN;
END@

CREATE TRIGGER update_balance_after_retiro
AFTER INSERT ON Retirada
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   UPDATE Cuenta SET Saldo = Saldo - N.Cantidad WHERE IBAN = N.Cuenta_IBAN;
END@

CREATE TRIGGER delete_cliente_tiene
AFTER DELETE ON Cliente
REFERENCING OLD AS O
FOR EACH ROW
BEGIN ATOMIC
   DELETE FROM Tiene WHERE Cliente_DNI = O.DNI;
END@

CREATE TRIGGER check_transferencia_same_account
NO CASCADE BEFORE INSERT ON Transferencia
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   IF N.Cuenta_IBAN = N.Cuenta_IBAN_Receptor THEN
      SIGNAL SQLSTATE '75002' SET MESSAGE_TEXT = 'No se puede transferir a la misma cuenta.';
   END IF;
END@

CREATE TRIGGER check_tipo_operacion_ingreso
NO CASCADE BEFORE INSERT ON Ingreso
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   DECLARE v_tipo VARCHAR(255);
   SET v_tipo = (SELECT Tipo FROM Operacion_Bancaria WHERE Codigo_Numerico = N.Codigo_Numerico AND Cuenta_IBAN = N.Cuenta_IBAN);
   IF v_tipo != 'INGRESO' THEN
      SIGNAL SQLSTATE '75003' SET MESSAGE_TEXT = 'La operación no es un INGRESO.';
   END IF;
END@

CREATE TRIGGER check_tipo_operacion_transferencia
NO CASCADE BEFORE INSERT ON Transferencia
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   DECLARE v_tipo VARCHAR(255);
   SET v_tipo = (SELECT Tipo FROM Operacion_Bancaria WHERE Codigo_Numerico = N.Codigo_Numerico AND Cuenta_IBAN = N.Cuenta_IBAN);
   IF v_tipo != 'TRANSFERENCIA' THEN
      SIGNAL SQLSTATE '75004' SET MESSAGE_TEXT = 'La operación no es una TRANSFERENCIA.';
   END IF;
END@

CREATE TRIGGER check_tipo_operacion_retiro
NO CASCADE BEFORE INSERT ON Retirada
REFERENCING NEW AS N
FOR EACH ROW
BEGIN ATOMIC
   DECLARE v_tipo VARCHAR(255);
   SET v_tipo = (SELECT Tipo FROM Operacion_Bancaria WHERE Codigo_Numerico = N.Codigo_Numerico AND Cuenta_IBAN = N.Cuenta_IBAN);
   IF v_tipo != 'RETIRO' THEN
      SIGNAL SQLSTATE '75005' SET MESSAGE_TEXT = 'La operación no es un RETIRO.';
   END IF;
END@










-- 1️⃣ Insertar datos en la tabla Cliente
INSERT INTO Cliente (ref_cliente, Nombre, Apellidos, Edad, Telefono, Email, Direccion, DNI)
VALUES (MAKE_REF(Cliente, '123456789A'), 'Juan', 'Pérez', 30, '555-1234', 'juan.perez@email.com', 'Calle Falsa 123, Ciudad X', '123456789A');

INSERT INTO Cliente (ref_cliente, Nombre, Apellidos, Edad, Telefono, Email, Direccion, DNI)
VALUES (MAKE_REF(Cliente, '987654321B'), 'Ana', 'Gómez', 28, '555-5678', 'ana.gomez@email.com', 'Avenida Siempre Viva 456, Ciudad Y', '987654321B');

INSERT INTO Cliente (ref_cliente, Nombre, Apellidos, Edad, Telefono, Email, Direccion, DNI)
VALUES (MAKE_REF(Cliente, '112233445C'), 'Carlos', 'Martínez', 35, '555-9876', 'carlos.martinez@email.com', 'Calle Principal 789, Ciudad Z', '112233445C');

INSERT INTO Cliente (ref_cliente, Nombre, Apellidos, Edad, Telefono, Email, Direccion, DNI)
VALUES (MAKE_REF(Cliente, '223344556D'), 'Laura', 'Hernández', 40, '555-1111', 'laura.hernandez@email.com', 'Plaza Mayor 12, Ciudad W', '223344556D');

INSERT INTO Cliente (ref_cliente, Nombre, Apellidos, Edad, Telefono, Email, Direccion, DNI)
VALUES (MAKE_REF(Cliente, '334455667E'), 'Pedro', 'López', 25, '555-2222', 'pedro.lopez@email.com', 'Calle del Sol 45, Ciudad V', '334455667E');

-- 2️⃣ Insertar datos en la tabla Sucursal
INSERT INTO Sucursal (ref_sucursal, Codigo, Direccion, Telefono)
VALUES (MAKE_REF(Sucursal, 101), 101, 'Sucursal Central, Ciudad X', 5551111);

INSERT INTO Sucursal (ref_sucursal, Codigo, Direccion, Telefono)
VALUES (MAKE_REF(Sucursal, 102), 102, 'Sucursal Norte, Ciudad Y', 5552222);

INSERT INTO Sucursal (ref_sucursal, Codigo, Direccion, Telefono)
VALUES (MAKE_REF(Sucursal, 103), 103, 'Sucursal Este, Ciudad Z', 5553333);

-- 3️⃣ Insertar datos en la tabla Cuenta
INSERT INTO Cuenta (ref_cuenta, Numero_Cuenta, IBAN, Saldo, Fecha_Creacion)
VALUES (MAKE_REF(Cuenta, 'ES1234567890123456789012'), 1234567890, 'ES1234567890123456789012', 1000.00, '2022-01-15');

INSERT INTO Cuenta (ref_cuenta, Numero_Cuenta, IBAN, Saldo, Fecha_Creacion)
VALUES (MAKE_REF(Cuenta, 'ES9876543210987654321098'), 987654321, 'ES9876543210987654321098', 2500.00, '2022-02-20');

INSERT INTO Cuenta (ref_cuenta, Numero_Cuenta, IBAN, Saldo, Fecha_Creacion)
VALUES (MAKE_REF(Cuenta, 'ES5432167890123456789012'), 543216789, 'ES5432167890123456789012', 1500.00, '2022-03-01');

-- 4️⃣ Insertar datos en la tabla Operacion_Bancaria
INSERT INTO Operacion_Bancaria (ref_operacion, Codigo_Numerico, Fecha, Cantidad, Tipo, Descripcion, Cuenta_IBAN)
VALUES (MAKE_REF(Operacion_Bancaria, (1, 'ES1234567890123456789012')), 1, '2023-03-01', 200.00, 'TRANSFERENCIA', 'Transferencia de dinero', 'ES1234567890123456789012');

INSERT INTO Operacion_Bancaria (ref_operacion, Codigo_Numerico, Fecha, Cantidad, Tipo, Descripcion, Cuenta_IBAN)
VALUES (MAKE_REF(Operacion_Bancaria, (2, 'ES9876543210987654321098')), 2, '2023-03-02', 500.00, 'RETIRO', 'Retiro en cajero automático', 'ES9876543210987654321098');

INSERT INTO Operacion_Bancaria (ref_operacion, Codigo_Numerico, Fecha, Cantidad, Tipo, Descripcion, Cuenta_IBAN)
VALUES (MAKE_REF(Operacion_Bancaria, (3, 'ES5432167890123456789012')), 3, '2023-03-03', 300.00, 'INGRESO', 'Ingreso en cuenta', 'ES5432167890123456789012');

-- 5️⃣ Insertar datos en la tabla Transferencia
INSERT INTO Transferencia (Codigo_Numerico, Cuenta_IBAN, Cuenta_IBAN_Receptor)
VALUES (1, 'ES1234567890123456789012', 'ES9876543210987654321098');

INSERT INTO Transferencia (Codigo_Numerico, Cuenta_IBAN, Cuenta_IBAN_Receptor)
VALUES (2, 'ES9876543210987654321098', 'ES1234567890123456789012');

-- 6️⃣ Insertar datos en la tabla Retirada
INSERT INTO Retirada (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo)
VALUES (1, 'ES1234567890123456789012', 101);

INSERT INTO Retirada (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo)
VALUES (2, 'ES9876543210987654321098', 102);

-- 7️⃣ Insertar datos en la tabla Ingreso
INSERT INTO Ingreso (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo)
VALUES (1, 'ES1234567890123456789012', 101);

INSERT INTO Ingreso (Codigo_Numerico, Cuenta_IBAN, Sucursal_Codigo)
VALUES (2, 'ES9876543210987654321098', 102);

-- 8️⃣ Insertar datos en la tabla CuentaAhorro
INSERT INTO CuentaAhorro (IBAN, Interes)
VALUES ('ES1234567890123456789012', 1.5);

INSERT INTO CuentaAhorro (IBAN, Interes)
VALUES ('ES5432167890123456789012', 2.0);

-- 9️⃣ Insertar datos en la tabla CuentaCorriente
INSERT INTO CuentaCorriente (IBAN, limite_credito)
VALUES ('ES9876543210987654321098', 3000.00);

-- 🔟 Insertar datos en la tabla Tiene (relación Cliente-Cuenta)
INSERT INTO Tiene (ref_tiene, Cliente_DNI, Cuenta_IBAN)
VALUES (MAKE_REF(Tiene, ('123456789A', 'ES1234567890123456789012')), '123456789A', 'ES1234567890123456789012');

INSERT INTO Tiene (ref_tiene, Cliente_DNI, Cuenta_IBAN)
VALUES (MAKE_REF(Tiene, ('987654321B', 'ES9876543210987654321098')), '987654321B', 'ES9876543210987654321098');

INSERT INTO Tiene (ref_tiene, Cliente_DNI, Cuenta_IBAN)
VALUES (MAKE_REF(Tiene, ('112233445C', 'ES5432167890123456789012')), '112233445C', 'ES5432167890123456789012');
