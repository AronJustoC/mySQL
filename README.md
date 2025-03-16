# Empresa de jugos en MySQL

## 1. Crear base de datos

```sql
CREATE DATABASE jugos;
```

## 2. Crear las tablas necesarias

Para el registro de clientes:

- DNI
- NOMBRE COMPLETO
- DIRECCIÓN1
- DIRECCIÓN2
- BARRIO
- CIUDAD
- PROVINCIA
- CP
- EDAD
- SEXO
- LIMITE_CREDITO
- VOLUMEN_COMPRA
- PRIMERA_COMPRA

Para el registro de productos:

- PRODUCTO
- NOMBRE
- ENVASE
- VOLUMEN
- SABOR
- PRECIO

## 3. Crearmos las queries para los registros de clientes

### 3.1 Crear TABLES

-Primero creamos la tabla clientes con los campos necesarios:

```sql
CREATE TABLE  TBCLIENTES (
  DNI VARCHAR(20) NOT NULL,
  NOMBRE VARCHAR(150) NOT NULL,
  DIRECCION1 VARCHAR(150) NOT NULL,
  DIRECCION2 VARCHAR(150) NOT NULL,
  BARRIO VARCHAR(50) NOT NULL,
  CIUDAD VARCHAR(50) NOT NULL,
  PROVINCIA VARCHAR(50) NOT NULL,
  CP VARCHAR(10) NOT NULL,
  EDAD SMALLINT NOT NULL,
  SEXO VARCHAR(1) NOT NULL,
  LIMITE_CREDITO FLOAT NOT NULL,
  VOLUMEN_COMPRA FLOAT NOT NULL,
  PRIMERA_COMPRA BIT(1) NOT NULL
)
```

- Creamos la tabla productos con los campos necesarios:

```sql
CREATE TABLE TBPRODUCTOS (
  PRODUCTO VARCHAR(20) NOT NULL,
  NOMBRE VARCHAR(150) NOT NULL,
  ENVASE VARCHAR(50) NOT NULL,
  VOLUMEN VARCHAR(20) NOT NULL,
  SABOR VARCHAR(50) NOT NULL,
  PRECIO FLOAT NOT NULL
)
```

### 3.2 Insertar valores en tablas

-Ahora vamos a insertar un registro de clientes en la tabla clientes:

```sql
INSERT INTO clientes (
  dni, 
  nombre, 
  direccion1, 
  direccion2, 
  barrio, 
  ciudad, 
  provincia, 
  cp, 
  edad, 
  sexo, 
  limite_credito, 
  volumen_compra, 
  primera_compra)
VALUES (
  123456789, 
  'Juan Perez', 
  'Calle 123', 
  'Apartado 1', 
  'Barrio 1', 
  'Ciudad 1', 
  'Provincia 1', 
  '12345', 
  25, 
  'M', 
  1000, 
  100, 
  2020-01-01);
```

### 3.2 Consultar registro de clientes
