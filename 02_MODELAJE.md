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
