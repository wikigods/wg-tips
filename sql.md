La base de datos está conformada por cuatro tablas principales: Clientes, Productos, Ventas y DetalleVenta. La tabla Clientes almacena la información de los compradores; Productos registra el catálogo disponible; Ventas almacena la información general de cada compra realizada; y DetalleVenta relaciona cada venta con los productos adquiridos. Se implementaron llaves primarias para identificar de forma única cada registro y llaves foráneas para mantener la integridad referencial. Además, se creó una vista para facilitar las consultas, un procedimiento almacenado para consultar ventas por cliente, un trigger para validar el stock disponible antes de registrar una venta y un usuario con permisos de solo lectura para garantizar la seguridad de la información.

Clientes
---------
IdCliente PK

Productos
---------
IdProducto PK

Ventas
-------
IdVenta PK
IdCliente FK

DetalleVenta
------------
IdDetalle PK
IdVenta FK
IdProducto FK

```sql
/*==============================================================
    UNIVERSIDAD CÉSAR VALLEJO
    CURSO        : GESTIÓN DE DATOS E INFORMACIÓN
    ACTIVIDAD    : IMPLEMENTACIÓN DE BASE DE DATOS EN SQL SERVER
    EMPRESA      : TechUCVStore S.A.C.
    BASE DE DATOS: TechUCVStore
==============================================================*/
USE master;
GO

IF DB_ID('TechUCVStore') IS NOT NULL
BEGIN
    ALTER DATABASE TechUCVStore SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
    DROP DATABASE TechUCVStore;
END
GO

CREATE DATABASE TechUCVStore;
GO

USE TechUCVStore;
GO

/*==============================================================
                    TABLA: CLIENTES
==============================================================*/
PRINT 'CREANDO TABLA CLIENTES';
GO

CREATE TABLE Clientes(
    IdCliente INT IDENTITY(1,1) PRIMARY KEY,
    DNI CHAR(8) UNIQUE NOT NULL,
    Nombres VARCHAR(100) NOT NULL,
    Apellidos VARCHAR(100) NOT NULL,
    Telefono VARCHAR(15),
    Correo VARCHAR(100),
    Direccion VARCHAR(150)
);
GO

/*==============================================================
                    TABLA: PRODUCTOS
==============================================================*/
PRINT 'CREANDO TABLA PRODUCTOS';
GO

CREATE TABLE Productos(
    IdProducto INT IDENTITY(1,1) PRIMARY KEY,
    Nombre VARCHAR(100) NOT NULL,
    Categoria VARCHAR(50),
    Precio DECIMAL(10,2) NOT NULL,
    Stock INT NOT NULL
);
GO

/*==============================================================
                    TABLA: VENTAS
==============================================================*/
PRINT 'CREANDO TABLA VENTAS';
GO

CREATE TABLE Ventas(
    IdVenta INT IDENTITY(1,1) PRIMARY KEY,
    IdCliente INT NOT NULL,
    FechaVenta DATE NOT NULL,
    Total DECIMAL(10,2) NOT NULL,

    CONSTRAINT FK_Ventas_Clientes
    FOREIGN KEY(IdCliente)
    REFERENCES Clientes(IdCliente)
);
GO

/*==============================================================
                TABLA: DETALLE VENTA
==============================================================*/
PRINT 'CREANDO TABLA DETALLEVENTA';
GO

CREATE TABLE DetalleVenta(
    IdDetalle INT IDENTITY(1,1) PRIMARY KEY,
    IdVenta INT NOT NULL,
    IdProducto INT NOT NULL,
    Cantidad INT NOT NULL,
    PrecioUnitario DECIMAL(10,2) NOT NULL,
    Subtotal DECIMAL(10,2) NOT NULL,

    CONSTRAINT FK_DetalleVenta_Ventas
        FOREIGN KEY(IdVenta)
        REFERENCES Ventas(IdVenta),

    CONSTRAINT FK_DetalleVenta_Productos
        FOREIGN KEY(IdProducto)
        REFERENCES Productos(IdProducto)
);
GO

/*==============================================================
                INSERCIÓN DE CLIENTES
==============================================================*/
PRINT 'INSERTANDO CLIENTES';
GO

INSERT INTO Clientes
(DNI,Nombres,Apellidos,Telefono,Correo,Direccion)
VALUES
('71234561','Juan','Perez','999111111','juan@gmail.com','Lima'),
('71234562','Luis','Torres','999111112','luis@gmail.com','Piura'),
('71234563','Carlos','Rojas','999111113','carlos@gmail.com','Chiclayo'),
('71234564','Ana','Garcia','999111114','ana@gmail.com','Lima'),
('71234565','Maria','Flores','999111115','maria@gmail.com','Trujillo'),
('71234566','Pedro','Lopez','999111116','pedro@gmail.com','Cusco'),
('71234567','Lucia','Sanchez','999111117','lucia@gmail.com','Arequipa'),
('71234568','Miguel','Ramirez','999111118','miguel@gmail.com','Tacna'),
('71234569','Rosa','Castillo','999111119','rosa@gmail.com','Ica'),
('71234570','Jose','Vargas','999111120','jose@gmail.com','Puno'),
('71234571','Carmen','Diaz','999111121','carmen@gmail.com','Huancayo'),
('71234572','Fernando','Morales','999111122','fernando@gmail.com','Ayacucho'),
('71234573','Patricia','Ortega','999111123','patricia@gmail.com','Cajamarca'),
('71234574','Ricardo','Gutierrez','999111124','ricardo@gmail.com','Huaraz'),
('71234575','Andrea','Mendoza','999111125','andrea@gmail.com','Tumbes'),
('71234576','Jorge','Silva','999111126','jorge@gmail.com','Moquegua'),
('71234577','Diana','Reyes','999111127','diana@gmail.com','Pucallpa'),
('71234578','Kevin','Navarro','999111128','kevin@gmail.com','Tarapoto'),
('71234579','Valeria','Herrera','999111129','valeria@gmail.com','Chimbote'),
('71234580','Diego','Salazar','999111130','diego@gmail.com','Lambayeque');
GO

/*==============================================================
                INSERCIÓN DE PRODUCTOS
==============================================================*/
PRINT 'INSERTANDO PRODUCTOS';
GO

INSERT INTO Productos
(Nombre,Categoria,Precio,Stock)
VALUES
('Laptop Lenovo','Computo',2500.00,20),
('Mouse Logitech','Accesorios',80.00,100),
('Teclado Redragon','Accesorios',150.00,60),
('Monitor Samsung','Monitores',850.00,25),
('Disco SSD 1TB','Almacenamiento',420.00,40),
('Laptop HP 15','Computo',2800.00,15),
('Laptop Dell Inspiron','Computo',3200.00,12),
('Memoria RAM 16GB','Componentes',320.00,50),
('Disco Duro 2TB','Almacenamiento',380.00,35),
('Impresora Epson L3250','Impresoras',950.00,18),
('Audifonos Sony','Audio',250.00,45),
('WebCam Logitech C920','Accesorios',350.00,30),
('Router TP-Link AX1500','Redes',280.00,22),
('Memoria USB 64GB','Almacenamiento',45.00,120),
('Tablet Samsung Galaxy Tab A9','Tablets',980.00,16);
GO

/*==============================================================
                INSERCIÓN DE VENTAS
==============================================================*/
PRINT 'INSERTANDO VENTAS';
GO

INSERT INTO Ventas
(IdCliente,FechaVenta,Total)
VALUES
(1,'2026-01-10',2580.00),
(2,'2026-01-11',930.00),
(3,'2026-02-05',420.00),
(4,'2026-02-20',150.00),
(5,'2026-03-12',850.00),
(6,'2026-03-15',3200.00),
(7,'2026-03-18',280.00),
(8,'2026-03-20',380.00),
(9,'2026-03-25',250.00),
(10,'2026-04-01',950.00),
(11,'2026-04-03',350.00),
(12,'2026-04-05',980.00),
(13,'2026-04-07',2500.00),
(14,'2026-04-10',320.00),
(15,'2026-04-12',850.00),
(16,'2026-04-15',420.00),
(17,'2026-04-18',2800.00),
(18,'2026-04-20',150.00),
(19,'2026-04-22',80.00),
(20,'2026-04-25',45.00),
(1,'2026-05-01',280.00),
(2,'2026-05-03',2500.00),
(3,'2026-05-05',950.00),
(4,'2026-05-08',350.00),
(5,'2026-05-10',420.00),
(6,'2026-05-12',3200.00),
(7,'2026-05-15',380.00),
(8,'2026-05-18',980.00),
(9,'2026-05-20',2800.00),
(10,'2026-05-22',320.00),
(11,'2026-05-24',250.00),
(12,'2026-05-26',150.00),
(13,'2026-05-28',850.00),
(14,'2026-06-01',420.00),
(15,'2026-06-03',2500.00),
(16,'2026-06-05',950.00),
(17,'2026-06-07',350.00),
(18,'2026-06-09',280.00),
(19,'2026-06-11',80.00),
(20,'2026-06-13',45.00),
(1,'2026-06-15',3200.00),
(2,'2026-06-17',2800.00),
(3,'2026-06-19',380.00),
(4,'2026-06-21',250.00),
(5,'2026-06-23',980.00),
(6,'2026-06-25',320.00),
(7,'2026-06-27',850.00),
(8,'2026-06-28',420.00),
(9,'2026-06-29',150.00),
(10,'2026-06-30',2500.00);
GO

/*==============================================================
            INSERCIÓN DE DETALLE DE VENTA
==============================================================*/
PRINT 'INSERTANDO DETALLE DE VENTAS';
GO

INSERT INTO DetalleVenta
(IdVenta,IdProducto,Cantidad,PrecioUnitario,Subtotal)
VALUES
-- Venta 1 = 2580
(1,1,1,2500.00,2500.00),
(1,2,1,80.00,80.00),

-- Venta 2 = 930
(2,4,1,850.00,850.00),
(2,2,1,80.00,80.00),

-- Venta 3 = 420
(3,5,1,420.00,420.00),

-- Venta 4 = 150
(4,3,1,150.00,150.00),

-- Venta 5 = 850
(5,4,1,850.00,850.00),

-- Venta 6 = 3200
(6,7,1,3200.00,3200.00),

-- Venta 7 = 280
(7,13,1,280.00,280.00),

-- Venta 8 = 380
(8,9,1,380.00,380.00),

-- Venta 9 = 250
(9,11,1,250.00,250.00),

-- Venta 10 = 950
(10,10,1,950.00,950.00),

-- Venta 11 = 350
(11,12,1,350.00,350.00),

-- Venta 12 = 980
(12,15,1,980.00,980.00),

-- Venta 13 = 2500
(13,1,1,2500.00,2500.00),

-- Venta 14 = 320
(14,8,1,320.00,320.00),

-- Venta 15 = 850
(15,4,1,850.00,850.00),

-- Venta 16 = 420
(16,5,1,420.00,420.00),

-- Venta 17 = 2800
(17,6,1,2800.00,2800.00),

-- Venta 18 = 150
(18,3,1,150.00,150.00),

-- Venta 19 = 80
(19,2,1,80.00,80.00),

-- Venta 20 = 45
(20,14,1,45.00,45.00),

-- Venta 21 = 280
(21,13,1,280.00,280.00),

-- Venta 22 = 2500
(22,1,1,2500.00,2500.00),

-- Venta 23 = 950
(23,10,1,950.00,950.00),

-- Venta 24 = 350
(24,12,1,350.00,350.00),

-- Venta 25 = 420
(25,5,1,420.00,420.00),

-- Venta 26 = 3200
(26,7,1,3200.00,3200.00),

-- Venta 27 = 380
(27,9,1,380.00,380.00),

-- Venta 28 = 980
(28,15,1,980.00,980.00),

-- Venta 29 = 2800
(29,6,1,2800.00,2800.00),

-- Venta 30 = 320
(30,8,1,320.00,320.00),

-- Venta 31 = 250
(31,11,1,250.00,250.00),

-- Venta 32 = 150
(32,3,1,150.00,150.00),

-- Venta 33 = 850
(33,4,1,850.00,850.00),

-- Venta 34 = 420
(34,5,1,420.00,420.00),

-- Venta 35 = 2500
(35,1,1,2500.00,2500.00),

-- Venta 36 = 950
(36,10,1,950.00,950.00),

-- Venta 37 = 350
(37,12,1,350.00,350.00),

-- Venta 38 = 280
(38,13,1,280.00,280.00),

-- Venta 39 = 80
(39,2,1,80.00,80.00),

-- Venta 40 = 45
(40,14,1,45.00,45.00),

-- Venta 41 = 3200
(41,7,1,3200.00,3200.00),

-- Venta 42 = 2800
(42,6,1,2800.00,2800.00),

-- Venta 43 = 380
(43,9,1,380.00,380.00),

-- Venta 44 = 250
(44,11,1,250.00,250.00),

-- Venta 45 = 980
(45,15,1,980.00,980.00),

-- Venta 46 = 320
(46,8,1,320.00,320.00),

-- Venta 47 = 850
(47,4,1,850.00,850.00),

-- Venta 48 = 420
(48,5,1,420.00,420.00),

-- Venta 49 = 150
(49,3,1,150.00,150.00),

-- Venta 50 = 2500
(50,1,1,2500.00,2500.00);

GO

/*==============================================================
                CONSULTA 1
        TOTAL DE VENTAS POR CLIENTE
==============================================================*/

SELECT 'CONSULTA 1 - TOTAL DE VENTAS POR CLIENTE' AS Resultado;

SELECT
C.Nombres,
C.Apellidos,
SUM(V.Total) AS TotalComprado
FROM Clientes C
INNER JOIN Ventas V
ON C.IdCliente=V.IdCliente
GROUP BY
C.Nombres,
C.Apellidos;
GO

/*==============================================================
                CONSULTA 2
            PRODUCTO MÁS VENDIDO
==============================================================*/

SELECT 'CONSULTA 2 - PRODUCTO MAS VENDIDO' AS Resultado;

SELECT TOP 1
P.Nombre,
SUM(D.Cantidad) AS CantidadVendida
FROM Productos P
INNER JOIN DetalleVenta D
ON P.IdProducto=D.IdProducto
GROUP BY P.Nombre
ORDER BY CantidadVendida DESC;
GO

/*==============================================================
                CONSULTA 3
            VENTAS POR MES
==============================================================*/

SELECT 'CONSULTA 3 - VENTAS POR MES' AS Resultado;

SELECT
MONTH(FechaVenta) AS Mes,
SUM(Total) AS TotalVentas
FROM Ventas
GROUP BY MONTH(FechaVenta)
ORDER BY Mes;
GO

/*==============================================================
                CONSULTA 4
        TOP 5 CLIENTES CON MAYOR COMPRA
==============================================================*/

SELECT 'CONSULTA 4 - TOP 5 CLIENTES CON MAYOR COMPRA' AS Resultado;

SELECT TOP 5
C.Nombres,
C.Apellidos,
SUM(V.Total) AS TotalComprado
FROM Clientes C
INNER JOIN Ventas V
ON C.IdCliente=V.IdCliente
GROUP BY
C.Nombres,
C.Apellidos
ORDER BY TotalComprado DESC;
GO

/*==============================================================
                CREACIÓN DE VISTA
==============================================================*/

PRINT 'CREANDO VISTA vwVentas';
GO

CREATE VIEW vwVentas
AS
SELECT
V.IdVenta,
C.Nombres,
P.Nombre AS Producto,
D.Cantidad,
D.Subtotal,
V.FechaVenta
FROM Ventas V
INNER JOIN Clientes C
ON V.IdCliente=C.IdCliente
INNER JOIN DetalleVenta D
ON V.IdVenta=D.IdVenta
INNER JOIN Productos P
ON D.IdProducto=P.IdProducto;
GO

SELECT 'VISTA - LISTADO GENERAL DE VENTAS' AS Resultado;

SELECT *
FROM vwVentas;
GO

/*==============================================================
        PROCEDIMIENTO ALMACENADO
        VENTAS POR CLIENTE
==============================================================*/

PRINT 'CREANDO PROCEDIMIENTO ALMACENADO';
GO

CREATE PROCEDURE spVentasCliente
@IdCliente INT
AS
BEGIN

SELECT
*
FROM Ventas
WHERE IdCliente=@IdCliente;

END;
GO

SELECT 'PROCEDIMIENTO ALMACENADO - VENTAS POR CLIENTE' AS Resultado;

EXEC spVentasCliente 1;
GO

/*==============================================================
                TRIGGER
    VALIDAR STOCK ANTES DE INSERTAR
==============================================================*/

PRINT 'CREANDO TRIGGER';
GO

CREATE TRIGGER trgValidarStock
ON DetalleVenta
INSTEAD OF INSERT
AS
BEGIN

IF EXISTS
(
SELECT *
FROM inserted i
INNER JOIN Productos p
ON i.IdProducto=p.IdProducto
WHERE i.Cantidad>p.Stock
)

BEGIN
RAISERROR('Stock insuficiente.',16,1);
ROLLBACK TRANSACTION;
END

ELSE

BEGIN

INSERT INTO DetalleVenta
(IdVenta,IdProducto,Cantidad,PrecioUnitario,Subtotal)

SELECT
IdVenta,
IdProducto,
Cantidad,
PrecioUnitario,
Subtotal

FROM inserted;

END

END;
GO

/*==============================================================
            VERIFICACIÓN DE LOS REGISTROS
==============================================================*/

SELECT 'TOTAL DE CLIENTES' AS Resultado;
SELECT COUNT(*) AS TotalClientes
FROM Clientes;
GO

SELECT 'TOTAL DE PRODUCTOS' AS Resultado;
SELECT COUNT(*) AS TotalProductos
FROM Productos;
GO

SELECT 'TOTAL DE VENTAS' AS Resultado;
SELECT COUNT(*) AS TotalVentas
FROM Ventas;
GO

SELECT 'TOTAL DE DETALLE DE VENTAS' AS Resultado;
SELECT COUNT(*) AS TotalDetalleVentas
FROM DetalleVenta;
GO

/*==============================================================
                SEGURIDAD
            USUARIO SOLO LECTURA
==============================================================*/

PRINT 'CREANDO USUARIO SOLO LECTURA';
GO

USE TechUCVStore;
GO

IF EXISTS (SELECT * FROM sys.database_principals WHERE name='UsuarioLectura')
DROP USER UsuarioLectura;
GO

IF EXISTS (SELECT * FROM sys.server_principals WHERE name='UsuarioLectura')
DROP LOGIN UsuarioLectura;
GO

CREATE LOGIN UsuarioLectura
WITH PASSWORD='Password123!';
GO

CREATE USER UsuarioLectura
FOR LOGIN UsuarioLectura;
GO

ALTER ROLE db_datareader
ADD MEMBER UsuarioLectura;
GO

SELECT 'USUARIO DE SOLO LECTURA CREADO CORRECTAMENTE' AS Resultado;
GO
```

La base de datos TechUCVStore fue diseñada para administrar la información de clientes, productos y ventas de la empresa. Está compuesta por cuatro tablas relacionadas mediante llaves primarias y foráneas que garantizan la integridad referencial. Asimismo, se implementó una vista para facilitar consultas, un procedimiento almacenado para recuperar información específica, un trigger para validar reglas de negocio y un usuario con permisos de solo lectura para reforzar la seguridad de la base de datos.