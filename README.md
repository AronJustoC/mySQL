# Empresa de jugos en MySQL

<!--toc:start-->
- [Empresa de jugos en MySQL](#empresa-de-jugos-en-mysql)
  - [1. Crear base de datos](#1-crear-base-de-datos)
  - [2. Crear las tablas necesarias](#2-crear-las-tablas-necesarias)
    - [2.1. Tabla de clientes](#21-tabla-de-clientes)
    - [2.2. Tabla de productos](#22-tabla-de-productos)
  - [3. Insertar valores en las tablas](#3-insertar-valores-en-las-tablas)
    - [3.1. Insertar valores en la tabla de productos](#31-insertar-valores-en-la-tabla-de-productos)
    - [3.2. Insertar valores en la tabla de clientes](#32-insertar-valores-en-la-tabla-de-clientes)
  - [4. Actualizar registros en las tablas](#4-actualizar-registros-en-las-tablas)
    - [4.1. Actualizar un solo registro en la tabla de productos](#41-actualizar-un-solo-registro-en-la-tabla-de-productos)
    - [4.2. Actualizar múltiples registros en la tabla de productos](#42-actualizar-múltiples-registros-en-la-tabla-de-productos)
  - [5. Eliminar registros en las tablas](#5-eliminar-registros-en-las-tablas)
    - [5.1. Eliminar registros en la tabla de productos](#51-eliminar-registros-en-la-tabla-de-productos)
<!--toc:end-->

Este proyecto tiene como objetivo gestionar una base de datos para una empresa de jugos utilizando MySQL. A continuación, se detallan los pasos para crear la base de datos, las tablas necesarias, insertar datos, actualizar registros y eliminar registros.

## 1. Crear base de datos

Primero, creamos la base de datos llamada `jugos`:

```sql
CREATE DATABASE jugos;
```

## 2. Crear las tablas necesarias

### 2.1. Tabla de clientes

Para el registro de clientes, necesitamos los siguientes campos:

- DNI: Documento Nacional de Identidad del cliente.
- NOMBRE COMPLETO: Nombre completo del cliente.
- DIRECCIÓN1: Primera línea de la dirección del cliente.
- DIRECCIÓN2: Segunda línea de la dirección del cliente (opcional).
- BARRIO: Barrio donde reside el cliente.
- CIUDAD: Ciudad donde reside el cliente.
- PROVINCIA: Provincia donde reside el cliente.
- CP: Código postal del cliente.
- EDAD: Edad del cliente.
- SEXO: Sexo del cliente (M para masculino, F para femenino).
- LIMITE_CREDITO: Límite de crédito del cliente.
- VOLUMEN_COMPRA: Volumen de compra del cliente.
- PRIMERA_COMPRA: Indica si es la primera compra del cliente (1 para sí, 0 para no).

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

### 2.2. Tabla de productos

Para el registro de productos, necesitamos los siguientes campos:

- PRODUCTO: Código del producto.
- NOMBRE: Nombre del producto.
- ENVASE: Tipo de envase del producto.
- VOLUMEN: Volumen del producto.
- SABOR: Sabor del producto.
- PRECIO: Precio del producto.

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

## 3. Insertar valores en las tablas

### 3.1. Insertar valores en la tabla de productos

Insertamos algunos productos en la tabla `TBPRODUCTOS`:

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

### 3.2. Insertar valores en la tabla de clientes

Insertamos algunos clientes en la tabla `TBCLIENTES`:

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

## 4. Actualizar registros en las tablas

### 4.1. Actualizar un solo registro en la tabla de productos

Actualizamos el volumen de un producto específico:

```sql
UPDATE TBPRODUCTOS 
SET VOLUMEN = '350 ml' 
WHERE PRODUCTO = '812829';
```

### 4.2. Actualizar múltiples registros en la tabla de productos

Actualizamos el volumen de varios productos utilizando un `CASE` statement:

```sql
-- Actualizar múltiples productos en la tabla 'tbproductos'
UPDATE tbproductos
SET volumen = CASE 
    WHEN producto = '812829' THEN '350 ml'  -- CUANDO el producto es '812829', ENTONCES establece el volumen a '350 ml'
    WHEN producto = '812830' THEN '500 ml'  -- CUANDO el producto es '812830', ENTONCES establece el volumen a '500 ml'
    WHEN producto = '812831' THEN '750 ml'  -- CUANDO el producto es '812831', ENTONCES establece el volumen a '750 ml'
    ELSE volumen -- mantener el valor original si no hay coincidencia
END
WHERE producto IN ('812829', '812830', '812831');
```

## 5. Eliminar registros en las tablas

### 5.1. Eliminar registros en la tabla de productos

Eliminamis la base de datos `jugos`:

```sql
DROP DATABASE jugos;
```

Eliminamos la tabla `TBPRODUCTOS`:

```sql
DROP TABLE TBPRODUCTOS;
```

Eliminamos algunos registros(productos) de la tabla `TBPRODUCTOS`:

```sql
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '838819';
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '1037797';
DELETE FROM TBPRODUCTOS WHERE PRODUCTO = '812829';
```
