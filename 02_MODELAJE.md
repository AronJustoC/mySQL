# Proyectar con MySQL

## Proyectando una base de datos

1. Analizar los requisitos

   - Entender las reglas de negocio(realizar entrevistas)
   - Diseñar un modelo correspondiente con la realidad

2. Crear un modelo conceptual
   - Construir el diagrama de entidad-relación (ERD)
   - Establecer cardinalidades de las entidades(1-1, 1-n, n-m)

```txt
        +-----------+        +-------------+        +-----------+
        | Vendedor  |1------< Realiza     >------N  |  Ventas   |
        +-----------+        +-------------+        +-----------+
                                                  /       ^
                                                 /        |
                                                v         |
        +-----------+        +-------------+   1|      1 |
        |  Cliente  |1------< Involucra    >-----        |
        +-----------+        +-------------+             |
                                                        |
                                                        v
                                                +---------------+
                                                | Posee         |
                                                +---------------+
                                                        |
                                                        N
                                                        |
                                                        v
                                              +-------------------+
                                              |  Items Vendidos   |
                                              +-------------------+
                                                        ^
                                                        |
                                                        1
                                              +---------------+
                                              | Contiene      |
                                              +---------------+
                                                        ^
                                                        |
                                                        1
                                              +----------------+
                                              |   Productos    |
                                              +----------------+
```

3. Se estableceran las características de las tablas - Diagrama de entidad-relación

```txt
+----------------+      1     +----------+     N      +-------------+      1     +-----------+
|   Vendedor     |------------| Realiza  |------------|   Venta     |------------|  Posee    |
|* Matrícula     |            +----------+            |* Número     |            +-----------+
|  Nombre        |                                   >|  Fecha      |                   |
|  Barrio        |      1     +----------+     N     >|* DNI        |                   | N
|  Comisión      |    +-------| Involucra|------------|* Matrícula  |                   |
|  Fecha Adm.    |    |       +----------+            |  Impuesto   |                   v
|  Vacaciones    |    |                               +-------------+        +--------------------+
+----------------+    |                                                      |  Items Vendidos    |
                     |                                                      |* Número            |
+----------------+    |                                                      |* Código            |
|   Cliente      |----+                                                      |  Cantidad          |
|* DNI           |                                                           |  Precio            |
|  Nombre        |                                                           +--------------------+
|  Dirección     |                                                         1 |
|  Barrio        |           +-----------------------------------------------+
|  Ciudad        |           |
|  Estado        |           |
|  CP            |           |      +-----------+     1       +----------------+
|  Fecha Nac.    |           +------| Contiene  |-------------|   Productos    |
|  Edad          |                  +-----------+             |* Código        |
|  Sexo          |                                           |  Descripción   |
|  Límite Créd.  |                                           |  Sabor         |
|  Volumen Comp. |                                           |  Tamaño        |
|  Primera Comp. |                                           |  Envase        |
+----------------+                                           |  Precio Lista  |
                                                            +----------------+
```

4. Transformar el diagrama de entidades en esquema de tablas
   - Transformar cada entidad en una o mas tablas fisicas de la base de datos
   - Cada conexión entre entidades se transforma en una relación de la base de datos

```txt
+------------------+      1     +----------+     N      +-----------------+      1     +-----------+
|   TB_VENDEDOR    |------------| Realiza  |----------->|     VENTA       |------------|  Posee    |
|* MATRICULA(str)  |            +----------+            |* NUMERO(str)    |            +-----------+
|  NOMBRE(str)     |                                    |  FECHA(date)    |                   |
|  BARRIO(str)     |      1     +----------+     N      |* DNI(str)       |                   | N
|  COMISION(float) |    +-------| Involucra|----------->|* MATRICULA(str) |                   |
|  FECHA_ADM(date) |    |       +----------+            |  IMPUESTO(float)|                   v
|  VACACIONES(bool)|    |                               +-----------------+       +----------------------+
+------------------+    |                                                         |    TB_ITEMS_VENDIDOS |
                        |                                                         |*   NUMERO(str)       |
+------------------+    |                                                         |*   CODIGO(str)       |
|   TB_CLIENTE     |----+                                                         |    CANTIDAD(int)     |
|* DNI(str)        |                                                              |    PRECIO(float)     |
|  NOMBRE(str)     |                                                              +----------------------+
|  DIRECCION(str)  |                                                           1 |
|  BARRIO(str)     |            +-----------------------------------------------+
|  CIUDAD(str)     |            |
|  ESTADO(str)     |            |
|  CP(str)         |            |      +-----------+     1       +------------------+
|  FECHA_NAC(date) |            +------| Contiene  |-------------|   TB_PRODUCTOS   |
|  EDAD(int)       |                   +-----------+             |*  CODIGO(str)    |
|  SEXO(str)       |                                             |   DESCRIPCION(str)|
|  LIMITE_CRED(int)|                                             |   SABOR(str)     |
|  VOLUMEN_COMP(int)|                                            |   TAMANO(str)    |
|  PRIMERA_COMP(bool)|                                           |   ENVASE(str)    |
+-------------------+                                            |   PRECIO_LISTA(float)|
                                                                 +------------------+
```

5. Construir la base de datos

```sql
CREATE DATABASE jugos__ventas;
CREATE SCHEMA IF NOT EXISTS jugos__ventas2;
DROP TABLE IF EXISTS jugos__ventas2.tb_vendedor;
DROP TABLE IF EXISTS jugos__ventas2.tb_vendedor DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE jugos__ventas;
```

- Crear una tabla con llaves primarias (PK)

```sql
USE jugos__ventas;

CREATE TABLE IF NOT EXISTS tb_vendedor (
    MATRICULA VARCHAR(5) NOT NULL,
    NOMBRE VARCHAR(100) NULL,
    BARRIO VARCHAR(100) NULL,
    COMISION FLOAT NULL,
    FECHA_ADMISION DATE NULL,
    DE_VACACIONES BOOLEAN NULL,
    PRIMARY KEY (MATRICULA)
);

CREATE TABLE IF NOT EXISTS tb_productos (
    CODIGO VARCHAR(10) NOT NULL,
    DESCRIPCION VARCHAR(100) NULL,
    SABOR VARCHAR(50) NULL,
    TAMANO VARCHAR(50) NULL,
    ENVASE VARCHAR(50) NULL,
    PRECIO_LISTA FLOAT NULL,
    PRIMARY KEY (CODIGO)
);

CREATE TABLE IF NOT EXISTS tb_cliente (
    DNI VARCHAR(10) NOT NULL,
    NOMBRE VARCHAR(100) NULL,
    DIRECCION VARCHAR(150) NULL,
    BARRIO VARCHAR(100) NULL,
    CIUDAD VARCHAR(100) NULL,
    ESTADO VARCHAR(100) NULL,
    CP VARCHAR(10) NULL,
    FECHA_NAC DATE NULL,
    EDAD SMALLINT NULL,
    SEXO VARCHAR(1) NULL,
    LIMITE_CREDITO FLOAT NULL,
    VOLUMEN_COMPRA FLOAT NULL,
    PRIMERA_COMPRA BIT(1) NULL,
    PRIMARY KEY (DNI)
);
```

- Crear tablas con llaves foráneas (FK)

```sql
USE jugos__ventas;

CREATE TABLE IF NOT EXISTS tb_venta (
    NUMERO VARCHAR(10) NOT NULL,
    FECHA DATE NULL,
    DNI VARCHAR(10) NULL,
    MATRICULA VARCHAR(5) NULL,
    IMPUESTO FLOAT NULL,
    PRIMARY KEY (NUMERO)
);

ALTER TABLE tb_venta ADD CONSTRAINT FK_CLIENTE FOREIGN KEY (DNI) REFERENCES tb_cliente (DNI);
ALTER TABLE tb_venta ADD CONSTRAINT FK_VENDEDOR FOREIGN KEY (MATRICULA) REFERENCES tb_vendedor (MATRICULA);

ALTER TABLE tb_venta RENAME tb_factura;

==================================

USE jugos__ventas;

CREATE TABLE tb_items_facturas (
    NUMERO VARCHAR(10) NOT NULL,
    CODIGO VARCHAR(10) NOT NULL,
    CANTIDAD INT,
    PRECIO FLOAT,
    PRIMARY KEY (NUMERO, CODIGO)
);

ALTER TABLE tb_items_facturas ADD CONSTRAINT FK_FACTURA FOREIGN KEY (NUMERO) REFERENCES tb_factura (NUMERO);
ALTER TABLE tb_items_facturas ADD CONSTRAINT FK_PRODUCTO FOREIGN KEY (CODIGO) REFERENCES tb_productos (CODIGO);


```

## Incluyendo datos en las tablas

```sql
USE jugos__ventas;

INSERT INTO tb_productos (CODIGO, DESCRIPCION, SABOR, TAMANO, ENVASE, PRECIO_LISTA) VALUES ('1050206', 'Ligth','Sandia', '350 ml', 'lata', 4.56);

SELECT * FROM tb_productos;
```

- Insertar varios registros

```sql
INSERT INTO tb_productos VALUES
('1050207', 'Ligth','Sandia', '350 ml', 'lata', 4.56),
('1050208', 'Ligth','Sandia', '350 ml', 'lata', 4.56),
('1050209', 'Ligth','Sandia', '350 ml', 'lata', 4.56);
```

- Traer datos de otra base de datos

```sql
USE jugos__ventas;

SELECT * FROM jugos_ventas.tabla_de_productos;

INSERT INTO jugos__ventas.tb_productos
SELECT CODIGO_DEL_PRODUCTO AS CODIGO, NOMBRE_DEL_PRODUCTO AS DESCRIPCION,SABOR, TAMANO, ENVASE, PRECIO_DE_LISTA AS PRECIO_LISTA
FROM jugos_ventas.tabla_de_productos
WHERE CODIGO_DEL_PRODUCTO NOT IN (SELECT CODIGO FROM tb_productos);
```

- Traer datos de otra base de datos (clientes por dni)

```sql
USE jugos__ventas;

SELECT * FROM jugos_ventas.tabla_de_clientes;

INSERT INTO jugos__ventas.tb_cliente
SELECT
    DNI,
    NOMBRE,
    DIRECCION_1 AS DIRECCION,
    BARRIO,
    CIUDAD,
    ESTADO,
    CP,
    FECHA_DE_NACIMIENTO AS FECHA_NAC,
    EDAD,
    SEXO,
    LIMITE_DE_CREDITO AS LIMITE_CREDITO,
    VOLUMEN_DE_COMPRA AS VOLUMEN_COMPRA,
    PRIMERA_COMPRA
FROM jugos_ventas.tabla_de_clientes
WHERE DNI NOT IN (SELECT DNI FROM tb_cliente);
```

- Importar de archivos CSV (por terminal es dificil pero podemos hacerlo con un script IA)

```sql
DELETE FROM tb_vendedor;

INSERT INTO tb_vendedor (MATRICULA, NOMBRE, BARRIO, COMISION, FECHA_ADMISION, DE_VACACIONES) VALUES
('235', 'Miguel Pavón Silva', 'Condesa', 0.08, '2014-08-15', 0),
('236', 'Claudia Morales', 'Del Valle', 0.08, '2013-09-17', 1),
('237', 'Concepción Martinez', 'Contadero', 0.11, '2017-03-18', 1),
('238', 'Patricia Sánchez', 'Oblatos', 0, '2016-08-21', 0);
```

## Alterando y excluyendo datos existentes

```sql
SELECT * FROM tb_productos;

UPDATE tb_productos SET PRECIO_LISTA = 5 WHERE CODIGO =  '290478';

UPDATE tb_productos SET DESCRIPCION = 'Vida en el campo',
TAMANO = '1 litro',
ENVASE = 'Botella PET' WHERE CODIGO =  '290478';

SELECT * FROM tb_cliente;

UPDATE tb_cliente SET VOLUMEN_COMPRA = VOLUMEN_COMPRA/10;
```

- Usando UPDATE con FROM

```sql
SELECT * FROM tb_cliente;

SELECT * FROM jugos_ventas.tabla_de_clientes;

SELECT * FROM tb_vendedor A
INNER JOIN
jugos_ventas.tabla_de_vendedores B
ON A.MATRICULA = SUBSTRING(B.MATRICULA, 3, 3);

UPDATE tb_vendedor A
INNER JOIN
jugos_ventas.tabla_de_vendedores B
ON A.MATRICULA = SUBSTRING(B.MATRICULA, 3, 3)
SET A.DE_VACACIONES = B.VACACIONES;
```

- Excluyendo datos de nuestras tablas

```sql
DELETE FROM tb_productos WHERE CODIGO = '1001000';

DELETE FROM tb_productos WHERE TAMANO = '1 Litro';

SELECT CODIGO_DEL_PRODUCTO FROM jugos_ventas.tabla_de_productos;

SELECT CODIGO FROM tb_productos
WHERE CODIGO NOT IN (SELECT CODIGO_DEL_PRODUCTO FROM jugos_ventas.tabla_de_productos);

DELETE FROM tb_productos
WHERE CODIGO NOT IN (SELECT CODIGO_DEL_PRODUCTO FROM jugos_ventas.tabla_de_productos);

CREATE TABLE `tb_productos2` (
  `CODIGO` varchar(10) NOT NULL,
  `DESCRIPCION` varchar(100) DEFAULT NULL,
  `SABOR` varchar(50) DEFAULT NULL,
  `TAMANO` varchar(50) DEFAULT NULL,
  `ENVASE` varchar(50) DEFAULT NULL,
  `PRECIO_LISTA` float DEFAULT NULL,
  PRIMARY KEY (`CODIGO`)
);

INSERT INTO tb_productos2
SELECT * FROM tb_productos;

DELETE FROM tb_productos2;
```

- COMMIT y ROLLBACK

```sql
USE jugos__ventas;

INSERT INTO tb_vendedor (
MATRICULA,
NOMBRE,
BARRIO,
COMISION,
FECHA_ADMISION,
DE_VACACIONES)
VALUES
('260', 'Miguel Pavón Silva', 'Condesa', 0.08, '2014-08-15', 0);

SELECT * FROM tb_vendedor;

START TRANSACTION;

INSERT INTO tb_vendedor (MATRICULA,
NOMBRE,
BARRIO,
COMISION,
FECHA_ADMISION,
DE_VACACIONES)
VALUES
('261', 'Matias Pavón Silva', 'Condesa', 0.08, '2014-08-15', 0),
('262', 'Mogly Pollom Canta', 'Catania', 0.08, '2014-08-15', 0);

UPDATE tb_vendedor SET COMISION = 0.05*1.05;

ROLLBACK;

COMMIT;
```

- Auto incremento patrones y triggers

```sql
CREATE TABLE tb_identificacion(
    ID INT AUTO_INCREMENT NOT NULL,
    DESCRIPCION VARCHAR(100) NULL,
    PRIMARY KEY(ID)
);

SELECT * FROM tb_identificacion;

INSERT INTO tb_identificacion(DESCRIPCION) VALUES('Cliente A');
INSERT INTO tb_identificacion(DESCRIPCION) VALUES('Cliente B');
INSERT INTO tb_identificacion(DESCRIPCION) VALUES('Cliente C');

DELETE FROM tb_identificacion WHERE ID=2;

---Definiendo patrones para campos

CREATE TABLE tb_default(
    ID INT AUTO_INCREMENT NOT NULL,
    DESCRIPCION VARCHAR(100) NOT NULL,
    DIRECCION VARCHAR(100) NULL,
    CIUDAD VARCHAR(100) NULL DEFAULT 'Santiago',
    FECHA_DE_CREACION TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY(ID)
);

SELECT * FROM tb_default;

INSERT INTO tb_default(DESCRIPCION, DIRECCION, CIUDAD, FECHA_DE_CREACION)
VALUES('Cliente A', 'Calle 1', 'Santiago', '2022-01-01');

INSERT INTO tb_default(DESCRIPCION)
VALUES('Cliente Y');
```

- Triggers

```sql
CREATE TABLE tb_facturacion(
    FECHA DATE NULL,
    VENTA_TOTAL FLOAT NULL
);

CREATE TABLE tb_factura1 (
    NUMERO VARCHAR(10) NOT NULL,
    FECHA DATE NULL,
    DNI VARCHAR(10) NULL,
    MATRICULA VARCHAR(5) NULL,
    IMPUESTO FLOAT NULL,
    PRIMARY KEY (NUMERO),
    KEY FK_CLIENTE1 (DNI),
    KEY FK_VENDEDOR1 (MATRICULA),
    CONSTRAINT FK_CLIENTE1 FOREIGN KEY (DNI) REFERENCES tb_cliente (DNI),
    CONSTRAINT FK_VENDEDOR1 FOREIGN KEY (MATRICULA) REFERENCES tb_vendedor (MATRICULA)
);

CREATE TABLE tb_items_facturas1 (
    NUMERO VARCHAR(10) NOT NULL,
    CODIGO VARCHAR(10) NOT NULL,
    CANTIDAD INT DEFAULT NULL,
    PRECIO FLOAT DEFAULT NULL,
    PRIMARY KEY (NUMERO, CODIGO),
    KEY FK_PRODUCTO1 (CODIGO),
    CONSTRAINT FK_FACTURA1 FOREIGN KEY (NUMERO) REFERENCES tb_factura1 (NUMERO),
    CONSTRAINT FK_PRODUCTO1 FOREIGN KEY (CODIGO) REFERENCES tb_productos (CODIGO)
);

SELECT * FROM tb_items_facturas1;
SELECT * FROM tb_factura1;
SELECT * FROM tb_facturacion;


SHOW COLUMNS FROM tb_items_facturas1;
SHOW COLUMNS FROM tb_facturacion;
SHOW COLUMNS FROM tb_factura1;

INSERT INTO tb_factura1 VALUES
('0100', '2022-01-01',  '1471156710', '235', 0.10);

INSERT INTO tb_items_facturas1 VALUES
('0100', '1000889', 10, 20),
('0100', '1002767', 21, 65),
('0100', '1004327', 41, 123);


DELIMITER //

CREATE TRIGGER tb_facturacion
AFTER INSERT ON tb_items_facturas1
FOR EACH ROW
BEGIN

  DELETE FROM tb_facturacion;
  INSERT INTO tb_facturacion
  SELECT FECHA, SUM(CANTIDAD*PRECIO) AS VENTA_TOTAL
  FROM tb_factura1 A
  INNER JOIN
  tb_items_facturas1 B
  ON A.NUMERO = B.NUMERO
  GROUP BY A.FECHA;

END //

INSERT INTO tb_factura1 VALUES
('0103', '2022-01-01',  '1471156710', '235', 0.18);

INSERT INTO tb_items_facturas1 VALUES
('0103', '1000889', 100, 20),
('0103', '1002767', 210, 65),
('0103', '1004327', 41, 123);

================================================

SELECT * FROM tb_factura1;
SELECT * FROM tb_items_facturas1;

UPDATE tb_items_facturas1 SET CANTIDAD = 600
WHERE NUMERO = '0103' AND CODIGO = '1000889';

DELETE FROM tb_items_facturas1
WHERE NUMERO = '0103' AND CODIGO = '1000889';

DELIMITER //

CREATE TRIGGER TG_FACTURACION_DELETE
AFTER DELETE ON tb_items_facturas1
FOR EACH ROW
BEGIN

  DELETE FROM tb_facturacion;
  INSERT INTO tb_facturacion
  SELECT FECHA, SUM(CANTIDAD*PRECIO) AS VENTA_TOTAL
  FROM tb_factura1 A
  INNER JOIN
  tb_items_facturas1 B
  ON A.NUMERO = B.NUMERO
  GROUP BY A.FECHA;

END //

---

DELIMITER //

CREATE TRIGGER TG_FACTURACION_UPDATE
AFTER UPDATE ON tb_items_facturas1
FOR EACH ROW
BEGIN

  DELETE FROM tb_facturacion;
  INSERT INTO tb_facturacion
  SELECT FECHA, SUM(CANTIDAD*PRECIO) AS VENTA_TOTAL
  FROM tb_factura1 A
  INNER JOIN
  tb_items_facturas1 B
  ON A.NUMERO = B.NUMERO
  GROUP BY A.FECHA;

END //
```
