# 1. Crear base de datos

primero, creamos la base de datos llamada `jugos`:

```sql
CREATE DATABASE jugos;
```

> **nota:** asegúrate de tener permisos para crear bases de datos en tu servidor MySQL.

---

## 2. Crear las tablas necesarias

### 2.1. Tabla de clientes

para el registro de clientes, necesitamos los siguientes campos:

- `DNI`: documento nacional de identidad del cliente.
- `NOMBRE COMPLETO`: nombre completo del cliente.
- `DIRECCIÓN1`: primera línea de la dirección del cliente.
- `DIRECCIÓN2`: segunda línea de la dirección del cliente (opcional).
- `BARRIO`: barrio donde reside el cliente.
- `CIUDAD`: ciudad donde reside el cliente.
- `PROVINCIA`: provincia donde reside el cliente.
- `CP`: código postal del cliente.
- `EDAD`: edad del cliente.
- `SEXO`: sexo del cliente (`M` para masculino, `F` para femenino).
- `LIMITE_CREDITO`: límite de crédito del cliente.
- `VOLUMEN_COMPRA`: volumen de compra del cliente.
- `PRIMERA_COMPRA`: indica si es la primera compra del cliente (`1` para sí, `0` para no).

```sql
CREATE TABLE TBCLIENTES (
  DNI VARCHAR(20) NOT NULL,
  NOMBRE VARCHAR(150) NOT NULL,
  DIRECCION1 VARCHAR(150) NOT NULL,
  DIRECCION2 VARCHAR(150),
  BARRIO VARCHAR(50) NOT NULL,
  CIUDAD VARCHAR(50) NOT NULL,
  PROVINCIA VARCHAR(50) NOT NULL,
  CP VARCHAR(10) NOT NULL,
  EDAD SMALLINT NOT NULL,
  SEXO VARCHAR(1) NOT NULL,
  LIMITE_CREDITO FLOAT NOT NULL,
  VOLUMEN_COMPRA FLOAT NOT NULL,
  PRIMERA_COMPRA BIT(1) NOT NULL,
  PRIMARY KEY (DNI)
);
```

> **comentario:** la columna `DIRECCION2` es opcional, lo que permite mayor flexibilidad en los datos.

---

### 2.2. Tabla de productos

para el registro de productos, necesitamos los siguientes campos:

- `PRODUCTO`: código del producto.
- `NOMBRE`: nombre del producto.
- `ENVASE`: tipo de envase del producto.
- `VOLUMEN`: volumen del producto.
- `SABOR`: sabor del producto.
- `PRECIO`: precio del producto.

```sql
CREATE TABLE TBPRODUCTOS (
  PRODUCTO VARCHAR(20) NOT NULL,
  NOMBRE VARCHAR(150) NOT NULL,
  ENVASE VARCHAR(50) NOT NULL,
  VOLUMEN VARCHAR(20) NOT NULL,
  SABOR VARCHAR(50) NOT NULL,
  PRECIO FLOAT NOT NULL,
  PRIMARY KEY (PRODUCTO)
);
```

> **comentario:** asegúrate de que los códigos de producto sean únicos para evitar conflictos.

---

## 3. Insertar valores en las tablas

### 3.1. Insertar valores en la tabla de productos

insertamos algunos productos en la tabla `TBPRODUCTOS`:

```sql
INSERT INTO TBPRODUCTOS (
  PRODUCTO,
  NOMBRE,
  ENVASE,
  VOLUMEN,
  SABOR,
  PRECIO
) VALUES
  ('773912', 'clean', 'botella pet', '1 litro', 'naranja', 8.01),
  ('838819', 'clean', 'botella pet', '1.5 litro', 'naranja', 12.01),
  ('1037797', 'clean', 'botella pet', '2 litro', 'naranja', 16.01),
  ('812829', 'clean', 'lata', '2 litro', 'naranja', 2.81);
```

---

### 3.2. Insertar valores en la tabla de clientes

insertamos algunos clientes en la tabla `TBCLIENTES`:

```sql
INSERT INTO TBCLIENTES (
  DNI,
  NOMBRE,
  DIRECCION1,
  DIRECCION2,
  BARRIO,
  CIUDAD,
  PROVINCIA,
  CP,
  EDAD,
  SEXO,
  LIMITE_CREDITO,
  VOLUMEN_COMPRA,
  PRIMERA_COMPRA
) VALUES
  ('123456789', 'Juan Perez', 'Calle 123', 'Apartado 1', 'Barrio 1', 'Ciudad 1', 'Provincia 1', '12345', 25, 'M', 1000, 100, 1),
  ('987654321', 'Maria Lopez', 'Avenida 456', 'Depto 2', 'Barrio 2', 'Ciudad 2', 'Provincia 2', '54321', 30, 'F', 1500, 200, 1),
  ('456789123', 'Carlos Sanchez', 'Calle 789', 'Casa 3', 'Barrio 3', 'Ciudad 3', 'Provincia 3', '67890', 35, 'M', 2000, 300, 1);
```

> **comentario:** asegúrate de que los valores de `DNI` sean únicos, ya que es la clave primaria.

---

## 4. ACTUALIZAR REGISTROS EN LAS TABLAS

### 4.1. ACTUALIZAR UN SOLO REGISTRO EN LA TABLA DE PRODUCTOS

ACTUALIZAMOS EL VOLUMEN DE UN PRODUCTO ESPECÍFICO:

```SQL
UPDATE TBPRODUCTOS
SET VOLUMEN = '350 ML'
WHERE PRODUCTO = '812829';
```

---

### 4.2. ACTUALIZAR MÚLTIPLES REGISTROS EN LA TABLA DE PRODUCTOS

ACTUALIZAMOS EL VOLUMEN DE VARIOS PRODUCTOS UTILIZANDO UN `CASE` STATEMENT:

```SQL
UPDATE TBPRODUCTOS
SET VOLUMEN = CASE
    WHEN PRODUCTO = '812829' THEN '350 ML'
    WHEN PRODUCTO = '812830' THEN '500 ML'
    WHEN PRODUCTO = '812831' THEN '750 ML'
    ELSE VOLUMEN
END
WHERE PRODUCTO IN ('812829', '812830', '812831');
```

---

## 5. ELIMINAR REGISTROS EN LAS TABLAS

### 5.1. ELIMINAR REGISTROS EN LA TABLA DE PRODUCTOS

ELIMINAMOS ALGUNOS PRODUCTOS DE LA TABLA `TBPRODUCTOS`:

```SQL
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '838819';
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '1037797';
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '812829';
```

---

## 6. ESTABLECER CLAVES PRIMARIAS

### 6.1. ESTABLECER CLAVE PRIMARIA EN LA TABLA DE CLIENTES

```SQL
ALTER TABLE TBCLIENTES ADD PRIMARY KEY (DNI);
```

---

### 6.2. ESTABLECER CLAVE PRIMARIA EN LA TABLA DE PRODUCTOS

```SQL
ALTER TABLE TBPRODUCTOS ADD PRIMARY KEY (PRODUCTO);
```

---

### 6.3. VERIFICAR CLAVES PRIMARIAS

PARA VERIFICAR LA CLAVE PRIMARIA EN LA TABLA `TBPRODUCTOS`:

```SQL
SHOW KEYS FROM TBPRODUCTOS;
```

PARA VERIFICAR LA CLAVE PRIMARIA EN LA TABLA `TBCLIENTES`:

```SQL
SHOW KEYS FROM TBCLIENTES;
```

---

### 6.4. AGREGAR COLUMNA `FECHA_NACIMIENTO`

PARA AGREGAR UNA NUEVA COLUMNA LLAMADA `FECHA_NACIMIENTO` A LA TABLA `TBCLIENTES`:

```SQL
ALTER TABLE TBCLIENTES ADD FECHA_NACIMIENTO DATE;
```

---

## 7. Ejecutar consultas

### 7.1.Profundizando en SELECT

Para traer todas las filas de la tabla `TBCLIENTES`, utilizamos la sentencia `SELECT *`:

```SQL
SELECT * FROM TBCLIENTES;
```

> **nota:** si no se especifica el campo, se devuelve todos los campos.

Si queremos traer solo algunos campos:

```SQL
SELECT DNI, NOMBRE,EDAD, FECHA_NACIMIENTO FROM TBCLIENTES;
```

Si queremos traer solo un campo:

```SQL
SELECT FECHA_NACIMIENTO FROM TBCLIENTES;
```

Si queremos traer un campo y un valor:

```SQL
SELECT FECHA_NACIMIENTO, DNI FROM TBCLIENTES;
```

Podemos utilizar un alias para el campo:

```SQL
SELECT NOMBRE AS "Nombre del cliente", FECHA_NACIMIENTO as "Fecha de nacimiento" FROM TBCLIENTES;
```

Podemos limitar la cantidad de registros que devuelve la consulta:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD as "Años de edad" FROM TBCLIENTES LIMIT 2;
```

---

### 7.2. Agregar condiciones a SELECT

Podemos agregar condiciones a la consulta SELECT:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD as "Años de edad" FROM TBCLIENTES WHERE EDAD > 18;
```

Otros ejemplos en la tabla `TBPRODUCTOS` condiciones con operadores `=`, `>`, `<`, `>=`, `<=`, `!=`, `IN`:

Si usas > o < en la consulta, debes especificar el valor que quieres comparar:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD as "Años de edad" FROM TBCLIENTES WHERE NOMBRE > 'Juan Perez';
```

Otros ejemplos en la tabla `TBPRODUCTOS` condiciones con operadores `=`, `>`, `<`, `>=`, `<=`, `!=`:

```SQL\
SELECT NOMBRE AS "Nombre del producto", ENVASE AS "Envase del producto" FROM TBPRODUCTOS WHERE ENVASE = 'botella pet';
```

```SQL
SELECT NOMBRE AS "Nombre del producto", ENVASE AS "Envase del producto" FROM TBPRODUCTOS WHERE ENVASE != 'botella pet';
```

```SQL
SELECT NOMBRE AS "Nombre del producto", SABOR AS "Sabor del producto", PRECIO AS "Precio del producto" FROM TBPRODUCTOS WHERE PRECIO <=10.00;
```

Ejemplo en un rango de precios pricipalmente para tipo floating point:

```SQL
SELECT NOMBRE AS "Nombre del producto", SABOR AS "Sabor del producto", PRECIO AS "Precio del producto" FROM TBPRODUCTOS WHERE PRECIO BETWEEN 10.00 AND 15.00;
```

Para filtrar fechas, utilizamos el formato `YYYY-MM-DD`:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD AS "Edad del cliente", FECHA_NACIMIENTO AS "Precio del producto" FROM TBCLIENTES WHERE FECHA_NACIMIENTO BETWEEN '1999-01-01' AND '2010-12-31';
```

Filtrar solo con el año:

- Para dia y mes sera solamente reemplazar YEAR por DAY y MONTH:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD AS "Edad del cliente", FECHA_NACIMIENTO AS "Precio del producto" FROM TBCLIENTES WHERE YEAR(FECHA_NACIMIENTO) = 1995;
```

Update solo con el año:

```SQL
UPDATE TBCLIENTES SET FECHA_NACIMIENTO = '1995-01-01' WHERE YEAR(FECHA_NACIMIENTO) = 1995;
```

Filtros compuestos:

```SQL
SELECT NOMBRE AS "Nombre del cliente", EDAD AS "Edad del cliente", FECHA_NACIMIENTO AS "Precio del producto" FROM TBCLIENTES WHERE YEAR(FECHA_NACIMIENTO) = 1995 AND MONTH(FECHA_NACIMIENTO) = 1;
```

---

## 8. Consultas avanzadas

### 8.1. Para caso importaresmos archivos SQL

Para importar archivos SQL, utilizamos el comando `mysql`:

```bash
mysql -u [usuario] -p [nombre_base_datos] < archivo.sql
```

En nuestro caso las tablas se importaron en el siguiente orden:

```bash
mysql -u root -p jugos < jugos_tabla_de_clientes.sql
mysql -u root -p jugos < jugos_tabla_de_productos.sql
mysql -u root -p jugos < jugos_tabla_de_vendedores.sql
mysql -u root -p jugos < jugos_facturas.sql
mysql -u root -p jugos < jugos_items_facturas.sql
```

---

### 8.2. Diagrama de entidad-relación

Tenemos el diagrama de entidad-relación de las tablas importadas en la base de datos `jugos`:

- DiagramaER.png
  Si quieres ver el diagrama de entidad-relación de la base de datos `jugos`, puedes utilizar el siguiente comando:

```bash
feh DiagramaER.png
```

De acuerdo a la imagen, la base de datos `jugos` tiene las siguientes tablas:

- TBCLIENTES
- TBPRODUCTOS
- TBVENDEDORES
- FACTURAS
- ITEMS_FACTURAS

Y sus respectivas relaciones:

- TBCLIENTES -> TBPRODUCTOS
- TBCLIENTES -> TBVENDEDORES
- TBPRODUCTOS -> TBVENDEDORES
- TBVENDEDORES -> FACTURAS
- TBVENDEDORES -> ITEMS_FACTURAS

---

Utilizando el diagrama se pueden realizar consultas avanzadas, como por ejemplo:

- Consultas de agregación de datos
- Consultas de eliminación de datos
- Consultas de actualización de datos

---

### 8.3 Consultas en base al diagrama de entidad-relación

#### 8.3.1. Consultas realizadas

Usando SELECT:

```SQL
SELECT * FROM tabla_de_productos WHERE SABOR = 'mango' AND TAMANO = '470 ml';
```

```SQL
SELECT * FROM tabla_de_productos WHERE SABOR = 'mango' OR TAMANO = '470 ml';
```

```SQL
SELECT * FROM tabla_de_productos WHERE NOT (SABOR = 'mango') OR TAMANO = '470 ml';
```

```SQL
SELECT * FROM tabla_de_productos WHERE NOT (SABOR = 'mango' OR TAMANO = '470 ml');
```

```SQL
SELECT * FROM tabla_de_productos WHERE SABOR IN ('mango', 'papaya', 'kiwi');
```

Que es lo mismo que:

```SQL
SELECT * FROM tabla_de_productos WHERE SABOR = 'mango' OR SABOR = 'papaya' OR SABOR = 'kiwi';
```

```SQL
SELECT * FROM tabla_de_clientes WHERE CIUDAD IN ( 'Guadalajara', 'CIUDAD DE MEXICO') AND (EDAD BETWEEN 20 AND 25);
```

Usando LIKE:
Para buscar un nombre de cliente que contenga la palabra 'perez' en el campo NOMBRE.

```SQL
SELECT * FROM tabla_de_clientes WHERE NOMBRE LIKE '%perez%';
```

Para buscar un nombre de cliente que contenga la palabra 'perez' en el final del campo NOMBRE.

```SQL
SELECT * FROM tabla_de_clientes WHERE NOMBRE LIKE '%perez';
```

Busca un nombre de cliente que contenga la palabra 'perez' en el inicio del campo NOMBRE.

```SQL
SELECT * FROM tabla_de_clientes WHERE NOMBRE LIKE 'perez%';
```

#### 8.3 Usando DISTINCT, LIMIT, ORDER BY

DISTINCT:

```SQL
SELECT DISTINCT NOMBRE FROM tabla_de_clientes;
```

```SQL
SELECT DISTINCT NOMBRE FROM tabla_de_productos;
```

```SQL
SELECT DISTINCT ENVASE, TAMANO, SABOR FROM tabla_de_productos WHERE SABOR = 'mango' OR TAMANO = '470 ml';
```

LIMIT:

```SQL
SELECT DISTINCT NOMBRE FROM tabla_de_clientes LIMIT 3;
```

La tabla se limita a mostrar 5 registros a partir del 3er registro.

```SQL
SELECT DISTINCT NOMBRE FROM tabla_de_productos LIMIT 3, 5;
```

ORDER BY:

```SQL
SELECT DISTINCT NOMBRE FROM tabla_de_clientes ORDER BY NOMBRE;
```

```SQL
SELECT DISTINCT * FROM tabla_de_productos ORDER BY PRECIO_DE_LISTA DESC;
```

```SQL
SELECT DISTINCT * FROM tabla_de_productos ORDER BY PRECIO_DE_LISTA DESC, NOMBRE ASC;
```

GROUP BY:

```SQL
SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes GROUP BY ESTADO;
```

```SQL
SELECT ENVASE, AVG(PRECIO_DE_LISTA) AS PRECIO_PROMEDIO
FROM tabla_de_productos GROUP BY ENVASE;
```

```SQL
SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO
FROM tabla_de_productos GROUP BY ENVASE;
```

```SQL
SELECT ENVASE, COUNT(*)
FROM tabla_de_productos GROUP BY ENVASE;
```

```SQL
SELECT BARRIO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes GROUP BY BARRIO;
```

```SQL
SELECT BARRIO, AVG(LIMITE_DE_CREDITO) AS LIMITE_MEDIO
FROM tabla_de_clientes GROUP BY BARRIO;
```

```SQL
SELECT NOMBRE, COUNT(*)
FROM tabla_de_clientes GROUP BY NOMBRE;
```

```SQL
SELECT CIUDAD, COUNT(*)
FROM tabla_de_clientes GROUP BY CIUDAD;
```

```SQL
SELECT CIUDAD, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes GROUP BY CIUDAD;
```

```SQL
SELECT ESTADO, COUNT(*)
FROM tabla_de_clientes GROUP BY ESTADO;
```

```SQL
SELECT ESTADO, BARRIO, MAX(LIMITE_DE_CREDITO) AS LIMITE, EDAD
FROM tabla_de_clientes
WHERE EDAD >= 20
GROUP BY ESTADO, BARRIO, EDAD
ORDER BY EDAD DESC;
```

#### 8.4 Usando COUNT

```SQL
SELECT COUNT(*) FROM TBCLIENTES;
```

```SQL
SELECT COUNT(DNI) FROM TBCLIENTES;
```

#### 8.5 Usando SUM

```SQL
SELECT SUM(VOLUMEN_COMPRA) FROM TBCLIENTES;
```

#### 8.6 Usando AVG

```SQL
SELECT AVG(VOLUMEN_COMPRA) FROM TBCLIENTES;
```

#### 8.7 Usando MAX

```SQL
SELECT MAX(VOLUMEN_COMPRA) FROM TBCLIENTES;
```

#### 8.8 Usando MIN

```SQL
SELECT MIN(VOLUMEN_COMPRA) FROM TBCLIENTES;
```

#### 8.9 Usando HAVING

Por lo general va después de GROUP BY:

```SQL
SELECT SUM(EDAD) FROM tabla_de_clientes HAVING SUM(EDAD) > 1000;
```

```SQL
SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes
GROUP BY ESTADO
HAVING SUM(LIMITE_DE_CREDITO) > 1000000;
```

```SQL
SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO,
MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO
FROM tabla_de_productos GROUP BY ENVASE
HAVING SUM(PRECIO_DE_LISTA) > 80;
```

```txt
+-------------------+---------------+---------------+
| ENVASE            | PRECIO_MAXIMO | PRECIO_MINIMO |
+-------------------+---------------+---------------+
| Botella de Vidrio |         13.31 |           3.3 |
| Botella PET       |         38.01 |             7 |
+-------------------+---------------+---------------+
```

```SQL
SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO,
MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO
FROM tabla_de_productos GROUP BY ENVASE
HAVING SUM(PRECIO_DE_LISTA) > 80
AND MAX(PRECIO_DE_LISTA) >= 20;
```

```txt
+-------------------+---------------+---------------+
| ENVASE            | PRECIO_MAXIMO | PRECIO_MINIMO |
+-------------------+---------------+---------------+
| Botella de Vidrio |         13.31 |           3.3 |
+-------------------+---------------+---------------+
```

#### 8.10. Usando CASE para filtrar datos

```SQL
SELECT NOMBRE_DEL_PRODUCTO, PRECIO_DE_LISTA,
CASE
  WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
  WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
  ELSE 'Barato'
END AS PRECIO
FROM tabla_de_productos;
```

```txt
+---------------------+-----------------+-----------+
| NOMBRE_DEL_PRODUCTO | PRECIO_DE_LISTA | PRECIO    |
+---------------------+-----------------+-----------+
| Sabor da Montaña    |            6.31 | Asequible |
| Línea Citrus        |               7 | Asequible |
| Vida del Campo      |            8.41 | Asequible |
| Vida del Campo      |           19.51 | Costoso   |
| Vida del Campo      |           24.01 | Costoso   |
| Festival de Sabores |           38.01 | Costoso   |
| Clean               |           16.01 | Costoso   |
| Light               |            4.56 | Barato    |
| Línea Citrus        |             4.9 | Barato    |
| Línea Citrus        |             4.9 | Barato    |
| Verano              |             3.3 | Barato    |
| Verano              |            5.18 | Asequible |
| Refrescante         |           11.01 | Asequible |
----------------------+-----------------+-----------+
35 rows in set (0.01 sec)
```

```SQL
SELECT ENVASE, SABOR,
CASE
  WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
  WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
  ELSE 'Barato'
END AS PRECIO, MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO
FROM tabla_de_productos
WHERE TAMANO = '700 ml'
GROUP BY ENVASE,
CASE
  WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
  WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
  ELSE 'Barato'
END
ORDER BY ENVASE;
```

```txt
+-------------------+-----------------+-----------+
| ENVASE            | SABOR           | PRECIO    |
+-------------------+-----------------+-----------+
| Botella de Vidrio | Uva             | Asequible |
| Botella PET       | Lima/Limón      | Asequible |
| Botella de Vidrio | Cereza/Manzana  | Asequible |
| Botella PET       | Sandía          | Costoso   |
| Botella PET       | Cereza/Manzana  | Costoso   |
| Botella PET       | Asái            | Costoso   |
| Botella PET       | Naranja         | Costoso   |
| Lata              | Sandía          | Barato    |
| Botella de Vidrio | Lima/Limón      | Barato    |
| Botella de Vidrio | Limón           | Barato    |
+-------------------+-----------------+-----------+
```

#### 8.11. Uso de JOINS

JOINS son una forma de unir tablas relacionadas en una consulta SQL.

- INNER JOINS: se utilizan para unir tablas relacionadas en una consulta SQL.

```sql
SELECT A.NOMBRE, B.MATRICULA, COUNT(*)
FROM tabla_de_vendedores A
INNER JOIN
facturas B
ON A.MATRICULA = B.MATRICULA
GROUP BY A.NOMBRE, B.MATRICULA;
```

```txt
+----------------------+-----------+----------+
| NOMBRE               | MATRICULA | COUNT(*) |
+----------------------+-----------+----------+
| Miguel Pavón Silva   | 00235     |    29389 |
| Claudia Morales      | 00236     |    29375 |
| Concepción Martinez  | 00237     |    29113 |
+----------------------+-----------+----------+
```

- LEFT y RIGHT JOINS: se utilizan para unir tablas relacionadas en una consulta SQL.

```sql
SELECT DISTINCT A.DNI, A.NOMBRE, B.DNI FROM tabla_de_clientes A
INNER JOIN
facturas B
ON A.DNI = B.DNI;
```

```txt
+-------------+--------------------+-------------+
| DNI         | NOMBRE             | DNI         |
+-------------+--------------------+-------------+
| 1471156710  | Erica Carvajo      | 1471156710  |
| 3623344710  | Marcos Rosas       | 3623344710  |
| 492472718   | Jorge Castro       | 492472718   |
| 50534475787 | Abel Pintos        | 50534475787 |
| 5576228758  | Joana Olivera      | 5576228758  |
| 5648641702  | Paolo Mendez       | 5648641702  |
| 5840119709  | Gabriel Roca       | 5840119709  |
| 7771579779  | Marcelo Perez      | 7771579779  |
| 8502682733  | Luis Silva         | 8502682733  |
| 8719655770  | Carlos Santivañez  | 8719655770  |
| 9283760794  | Edson Calisaya     | 9283760794  |
| 94387575700 | María Jimenez      | 94387575700 |
+-------------+--------------------+-------------+
```

```sql
SELECT DISTINCT A.DNI, A.NOMBRE, B.DNI FROM tabla_de_clientes A
LEFT JOIN
facturas B
ON A.DNI = B.DNI
WHERE B.DNI IS NULL;
```

```sql
SELECT DISTINCT B.DNI, B.NOMBRE, A.DNI FROM facturas A
RIGHT JOIN
tabla_de_clientes B
ON B.DNI = A.DNI
WHERE A.DNI IS NULL;
```

- Los dos tienen el mismo resultado:

```txt
+-------------+-------------------+------+
| DNI         | NOMBRE            | DNI  |
+-------------+-------------------+------+
| 9275760794  | Alberto Rodriguez | NULL |
| 94387591700 | Walter Soruco     | NULL |
| 95939180787 | Ximena Gómez      | NULL |
+-------------+-------------------+------+
```

- FULL y CROSS JOINS: se utilizan para unir tablas relacionadas en una consulta SQL.

```sql
SELECT tabla_de_clientes.NOMBRE, tabla_de_vendedores.BARRIO,
tabla_de_vendedores.NOMBRE
FROM tabla_de_clientes
INNER JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO;
```

```sql
SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_vendedores.BARRIO,
tabla_de_vendedores.NOMBRE
FROM tabla_de_clientes
```

```sql
SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_clientes.BARRIO,
tabla_de_vendedores.NOMBRE, tabla_de_vendedores.VACACIONES
FROM tabla_de_clientes
LEFT JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO;
```

```txt
+--------------------+-------------------+-------------------------+----------------------+------------------------+
| NOMBRE             | CIUDAD            | BARRIO                  | NOMBRE               | VACACIONES             |
+--------------------+-------------------+-------------------------+----------------------+------------------------+
| Erica Carvajo      | Ciudad de México  | Del Valle               | Claudia Morales      | 0x01                   |
| Marcos Rosas       | Ciudad de México  | Del Valle               | Claudia Morales      | 0x01                   |
| Jorge Castro       | Ciudad de México  | Locaxco                 | NULL                 | NULL                   |
| Abel Pintos        | Ciudad de México  | Cuajimalpa              | NULL                 | NULL                   |
| Joana Olivera      | Ciudad de México  | Condesa                 | Miguel Pavón Silva   | 0x00                   |
+--------------------+-------------------+-------------------------+----------------------+------------------------+
```

- FULL JOINS(Pendiente)
  Es la union de los dos tipos de JOINs(LEFT y RIGHT) anteriores,
  permitiendo que se incluya más datos en los resultados.

```sql
SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_clientes.BARRIO,
tabla_de_vendedores.NOMBRE, tabla_de_vendedores.VACACIONES
FROM tabla_de_clientes
RIGHT JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO
UNION
SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_clientes.BARRIO,
tabla_de_vendedores.NOMBRE, tabla_de_vendedores.VACACIONES
FROM tabla_de_clientes
LEFT JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO;
```

```txt
+--------------------+-------------------+-------------------------+----------------------+------------------------+
| NOMBRE             | CIUDAD            | BARRIO                  | NOMBRE               | VACACIONES             |
+--------------------+-------------------+-------------------------+----------------------+------------------------+
| Joana Olivera      | Ciudad de México  | Condesa                 | Miguel Pavón Silva   | 0x00                   |
| Gabriel Roca       | Ciudad de México  | Del Valle               | Claudia Morales      | 0x01                   |
| Marcos Rosas       | Ciudad de México  | Del Valle               | Claudia Morales      | 0x01                   |
| Erica Carvajo      | Ciudad de México  | Del Valle               | Claudia Morales      | 0x01                   |
| Luis Silva         | Ciudad de México  | Contadero               | Concepción Martinez  | 0x01                   |
| Alberto Rodriguez  | Guadalajara       | Oblatos                 | Patricia Sánchez     | 0x00                   |
| Jorge Castro       | Ciudad de México  | Locaxco                 | NULL                 | NULL                   |
| Abel Pintos        | Ciudad de México  | Cuajimalpa              | NULL                 | NULL                   |
| Paolo Mendez       | Ciudad de México  | Héroes de Padierna      | NULL                 | NULL                   |
| Marcelo Perez      | Ciudad de México  | Carola                  | NULL                 | NULL                   |
| Carlos Santivañez  | Ciudad de México  | Floresta Coyoacán       | NULL                 | NULL                   |
| Edson Calisaya     | Ciudad de México  | Barrio del Niño Jesús   | NULL                 | NULL                   |
| María Jimenez      | Guadalajara       | Barragán Hernández      | NULL                 | NULL                   |
| Walter Soruco      | Ciudad de México  | Ex Hacienda Coapa       | NULL                 | NULL                   |
| Ximena Gómez       | Guadalajara       | Alcalde Barranquitas    | NULL                 | NULL                   |
+--------------------+-------------------+-------------------------+----------------------+------------------------+
```

#### 8.12. Usando UNION

UNION se usa para unir tablas relacionadas en una consulta SQL.

CURIOSIDAD: Se ejecuta DISTINCT por defecto, pero UNION ALL
hace que se muestren los valores repetidos.

```sql
SELECT DISTINCT tabla_de_clientes;
SELECT DISTINCT tabla_de_vendedores;

SELECT DISTINCT BARRIO FROM tabla_de_clientes
UNION
SELECT DISTINCT BARRIO FROM tabla_de_vendedores;
```

```txt
+-------------------------+
| BARRIO                  |
+-------------------------+
| Del Valle               |
| Locaxco                 |
| Cuajimalpa              |
| Condesa                 |
| Héroes de Padierna      |
| Carola                  |
| Contadero               |
| Floresta Coyoacán       |
| Oblatos                 |
| Barrio del Niño Jesús   |
| Barragán Hernández      |
| Ex Hacienda Coapa       |
| Alcalde Barranquitas    |
+-------------------------+
```

```sql
SELECT DISTINCT BARRIO FROM tabla_de_clientes
UNION ALL
SELECT DISTINCT BARRIO FROM tabla_de_vendedores;
```

```txt
+-------------------------+
| BARRIO                  |
+-------------------------+
| Del Valle               |
| Locaxco                 |
| Cuajimalpa              |
| Condesa                 |
| Héroes de Padierna      |
| Carola                  |
| Contadero               |
| Floresta Coyoacán       |
| Oblatos                 |
| Barrio del Niño Jesús   |
| Barragán Hernández      |
| Ex Hacienda Coapa       |
| Alcalde Barranquitas    |
| Condesa                 |
| Del Valle               |
| Contadero               |
| Oblatos                 |
+-------------------------+
```

```sql
SELECT NOMBRE, BARRIO, "Cliente" AS TIPO FROM tabla_de_clientes
UNION
SELECT NOMBRE, BARRIO, "Vendedor" AS TIPO FROM tabla_de_vendedores;
```

```txt
+----------------------+-------------------------+----------+
| NOMBRE               | BARRIO                  | TIPO     |
+----------------------+-------------------------+----------+
| Erica Carvajo        | Del Valle               | Cliente  |
| Marcos Rosas         | Del Valle               | Cliente  |
| Jorge Castro         | Locaxco                 | Cliente  |
| Abel Pintos          | Cuajimalpa              | Cliente  |
| Joana Olivera        | Condesa                 | Cliente  |
| Paolo Mendez         | Héroes de Padierna      | Cliente  |
| Gabriel Roca         | Del Valle               | Cliente  |
| Marcelo Perez        | Carola                  | Cliente  |
| Luis Silva           | Contadero               | Cliente  |
| Carlos Santivañez    | Floresta Coyoacán       | Cliente  |
| Alberto Rodriguez    | Oblatos                 | Cliente  |
| Edson Calisaya       | Barrio del Niño Jesús   | Cliente  |
| María Jimenez        | Barragán Hernández      | Cliente  |
| Walter Soruco        | Ex Hacienda Coapa       | Cliente  |
| Ximena Gómez         | Alcalde Barranquitas    | Cliente  |
| Miguel Pavón Silva   | Condesa                 | Vendedor |
| Claudia Morales      | Del Valle               | Vendedor |
| Concepción Martinez  | Contadero               | Vendedor |
| Patricia Sánchez     | Oblatos                 | Vendedor |
+----------------------+-------------------------+----------+
```

#### 8.13. SUBCONSULTAS

```sql
SELECT * FROM tabla_de_clientes
WHERE BARRIO IN ('Condesa', 'Del Valle', 'Contadero', 'Oblatos');
```

Que sera lo mismo que:

```sql
SELECT * FROM tabla_de_clientes
WHERE BARRIO IN (SELECT DISTINCT BARRIO FROM tabla_de_vendedores);
```

```sql
SELECT X.ENVASE, X.PRECIO_MAXIMO FROM (SELECT ENVASE, MAX(PRECIO_DE_LISTA)
AS PRECIO_MAXIMO
FROM tabla_de_productos
GROUP BY ENVASE) X
WHERE X.PRECIO_MAXIMO >= 10;
```

#### 8.14. VIEWS

- Se para agrupar consultas en una sola vista.
- Se utilizan para simplificar la consulta.

Creamos la vista:

```sql
CREATE VIEW vw_envases_grandes AS
SELECT ENVASE, MAX(PRECIO_DE_LISTA)
AS PRECIO_MAXIMO
FROM tabla_de_productos
GROUP BY ENVASE;
```

La empleamos en la consulta:

```sql
SELECT
A.NOMBRE_DEL_PRODUCTO,
A.ENVASE,
A.PRECIO_DE_LISTA,
B.PRECIO_MAXIMO,((PRECIO_DE_LISTA/PRECIO_MAXIMO)-1)*100 AS PORCENTAJE_DE_VARIACION
FROM
tabla_de_productos A
INNER JOIN vw_envases_grandes B
ON A.ENVASE = B.ENVASE;
```

#### 8.15. FUNCIONES

- relacionadas a STRINGS

```sql
SELECT LTRIM("    MySQL is great ");  --Corta espacios a la izquierda
SELECT RTRIM("MySQL is great     ");  --Corta espacios a la derecha
SELECT TRIM("   My is great   ");  --Corta espacios a la izquierda y a la derecha
SELECT CONCAT("MySQL ", "is ", "great");  --Concatena cadenas
SELECT UPPER("MySQL is great");  --Convierte a mayúsculas
SELECT LOWER("MySQL is great");  --Convierte a mayúsculas
SELECT SUBSTRING("MySQL is great", 10, 5);  --Extrae una parte de la cadena
```

```sql
SELECT CONCAT(NOMBRE, " ", DNI) FROM tabla_de_clientes;
```

```txt
+-------------------------------+
| CONCAT(NOMBRE, " ", DNI)      |
+-------------------------------+
| Erica Carvajo 1471156710      |
| Marcos Rosas 3623344710       |
| Jorge Castro 492472718        |
| Abel Pintos 50534475787       |
| Joana Olivera 5576228758      |
| Paolo Mendez 5648641702       |
| Gabriel Roca 5840119709       |
| Marcelo Perez 7771579779      |
| Luis Silva 8502682733         |
| Carlos Santivañez 8719655770  |
| Alberto Rodriguez 9275760794  |
| Edson Calisaya 9283760794     |
| María Jimenez 94387575700     |
| Walter Soruco 94387591700     |
| Ximena Gómez 95939180787      |
+-------------------------------+
15 rows in set (0.00 sec)
```

- ADD_DATE

```sql
SELECT ADDDATE('2025-03-22', INTERVAL 1 DAY);  --Agrega 1 día
```

- CURRENT_DATE

```sql
SELECT CURRENT_DATE();  --Devuelve la fecha actual
SELECT CURRENT_TIME();  --Devuelve la hora actual
SELECT CURRENT_TIMESTAMP();  --Devuelve la fecha y hora actual
SELECT YEAR(CURRENT_TIMESTAMP());
SELECT DATEDIFF('2025-03-22', '2025-03-25');
SELECT LOCALTIMESTAMP();

SELECT current_timestamp() AS DIA_HOY,
DATE_SUB(current_timestamp(),
INTERVAL 1 MONTH) AS RESULTADO;
```

```sql
SELECT DISTINCT
FECHA_VENTA,
DAYNAME(FECHA_VENTA) AS DIA_VENTA,
MONTHNAME(FECHA_VENTA) AS MES_VENTA,
YEAR(FECHA_VENTA) AS ANO_VENTA
FROM facturas;
```

- FUNCIONES NUMERICAS

```sql
SELECT CEILING(1.5);  --Arredonda hacia arriba
SELECT FLOOR(1.5);  --Arredonda hacia abajo
SELECT ROUND(1.54545, 3) AS RESULTADO;  --Arredonda a 3 decimales
SELECT RAND() AS RESULTADO;  --Devuelve un número aleatorio entre 0 y 1
```

```sql
SELECT NUMERO, CANTIDAD, PRECIO, ROUND(CANTIDAD*PRECIO,2) AS FACTURACION FROM items_facturas;
```

#### 8.16. CONVIRTIENDO DATOS

```sql
SELECT CURRENT_DATE() AS RESULTADO;
SELECT
CONCAT("La fecha y la hora actual son: ",
CURRENT_DATE(),
" y ",
CURRENT_TIME()) AS RESULTADO;
```

```sql
SELECT
CONCAT("La fecha y la hora actual son: ",
DATE_FORMAT(CURRENT_TIMESTAMP(), "%W, %d/%m/%Y a las %T")) AS RESULTADO;
```

```sql
SELECT CONVERT(23.55, CHAR) AS RESULTADO;
SELECT SUBSTRING(CONVERT(23.55, CHAR),3,1) AS RESULTADO;
```

#### 8.17. PROYECTO CON LO APRENDIDO

```sql
SELECT * FROM facturas;
SELECT * FROM items_facturas;
SELECT F.DNI, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")AS MES_ANIO,
IFa.CANTIDAD FROM facturas F
INNER JOIN
items_facturas IFa
ON F.NUMERO = IFa.NUMERO;
```

- Cantidad de ventas por mes para cada cliente

```sql
SELECT F.DNI, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")AS MES_ANIO,
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA FROM facturas F
INNER JOIN
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
GROUP BY F.DNI, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y");
```

- Limite de ventas por cliente (Volumen en decilitros)

```sql
SELECT * FROM tabla_de_clientes TC;

SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")AS MES_ANIO,
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA,
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA
FROM facturas F
INNER JOIN
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN
tabla_de_clientes TC
ON F.DNI = TC.DNI
GROUP BY F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y");
```

```sql
SELECT A.DNI, A.NOMBRE, A.MES_ANIO,
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA
FROM (SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")AS MES_ANIO,
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA,
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA
FROM facturas F
INNER JOIN
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN
tabla_de_clientes TC
ON F.DNI = TC.DNI
GROUP BY F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")
) A;
```

```sql
SELECT A.DNI, A.NOMBRE, A.MES_ANIO,
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
    WHEN (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta valida'
    ELSE 'Venta invalida'
END AS STATUS_VENTA
FROM (SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")AS MES_ANIO,
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA,
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA
FROM facturas F
INNER JOIN
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN
tabla_de_clientes TC
ON F.DNI = TC.DNI
GROUP BY F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y")
) A;
```

- Se requiere un informe con estas características:
  INF. VENTAS 2016

```txt
┌──────────┬──────┬───────────┬───────────┐
│ SABOR    │ AÑO  │ CANT. LT  │ PORCENT.  │
├──────────┼──────┼───────────┼───────────┤
│ FRESA    │ 2016 │ XXXXX     │ XXXX      │
│ NARANJA  │ 2016 │ XXXXX     │ XXXX      │
│  ...     │  ... │  ...      │  ...      │
│ SANDÍA   │ 2016 │ XXXXX     │ XXXX      │
└──────────┴──────┴───────────┴───────────┘
```

```sql
SELECT SABOR, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, YEAR(F.FECHA_VENTA) AS AÑO
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY SABOR, YEAR(F.FECHA_VENTA)
ORDER BY SUM(IFa.CANTIDAD) DESC;
```

```txt
+-----------------+-------------------+------+
| SABOR           | CANTIDAD_TOTAL    | AÑO  |
+-----------------+-------------------+------+
| Mango           |            613309 | 2016 |
| Sandía          |            487625 | 2016 |
| Naranja         |            483663 | 2016 |
| Manzana         |            363166 | 2016 |
| Asái            |            357275 | 2016 |
| Maracuyá        |            245456 | 2016 |
| Lima/Limón      |            239634 | 2016 |
| Frutilla/Limón  |            238118 | 2016 |
| Cereza/Manzana  |            236535 | 2016 |
| Uva             |            120597 | 2016 |
| Cereza          |            120478 | 2016 |
| Frutilla        |            120384 | 2016 |
+-----------------+-------------------+------+
```

- Cantidad total por year:

```sql
SELECT YEAR(F.FECHA_VENTA) AS AÑO, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY YEAR(F.FECHA_VENTA);
```

```txt
+------+----------------+
| AÑO  | CANTIDAD_TOTAL |
+------+----------------+
| 2016 |        3626240 |
+------+----------------+
```

- Completamos el informe solicitado:

```sql
SELECT * FROM (SELECT SABOR,
SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL,
YEAR(F.FECHA_VENTA) AS AÑO
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY SABOR, YEAR(F.FECHA_VENTA)
ORDER BY SUM(IFa.CANTIDAD) DESC) AS VENTAS_SABOR
INNER JOIN
(SELECT YEAR(F.FECHA_VENTA) AS AÑO, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY YEAR(F.FECHA_VENTA)) AS VENTA_TOTAL_YEAR
ON VENTAS_SABOR.AÑO = VENTA_TOTAL_YEAR.AÑO
```

```sql
SELECT
VENTAS_SABOR.SABOR,
VENTAS_SABOR.AÑO,
VENTAS_SABOR.CANTIDAD_TOTAL AS CANTIDAD_LITROS,
ROUND((VENTAS_SABOR.CANTIDAD_TOTAL/VENTA_TOTAL.CANTIDAD_TOTAL)*100,2) AS PORCENTAJE
FROM (SELECT SABOR,
SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL,
YEAR(F.FECHA_VENTA) AS AÑO
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY SABOR, YEAR(F.FECHA_VENTA)
ORDER BY SUM(IFa.CANTIDAD) DESC) AS VENTAS_SABOR
INNER JOIN
(SELECT YEAR(F.FECHA_VENTA) AS AÑO, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL
FROM tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON IFa.NUMERO = F.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY YEAR(F.FECHA_VENTA)) AS VENTA_TOTAL
ON VENTAS_SABOR.AÑO = VENTA_TOTAL.AÑO
ORDER BY VENTAS_SABOR.CANTIDAD_TOTAL DESC;
```
