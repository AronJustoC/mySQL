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

### 7.2.  Agregar condiciones a SELECT

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
