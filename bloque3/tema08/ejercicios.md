# Tema 8. Ejercicios de SQL (DDL)

> Trabajamos con **MySQL 8**. Cada ejercicio tiene la solución desplegable: intenta hacerlo antes de abrirla.

---

## Ejercicio 1: La tienda: del modelo relacional a SQL

Pasamos a SQL el modelo relacional de la tienda (tema 7):

```
PROVINCIAS (id_provincia, provincia, habitantes)
    PK: id_provincia

CLIENTES (DNI, nombre, id_provincia)
    PK: DNI
    FK: id_provincia → PROVINCIAS(id_provincia)  [NOT NULL]

PRODUCTOS (IdP, nombreP, stock)
    PK: IdP

VENTAS (DNI, IdP, fechaVenta, unds)
    PK: (DNI, IdP)
    FK: DNI → CLIENTES(DNI)
    FK: IdP → PRODUCTOS(IdP)
```


**Condiciones del enunciado:**

- El id_provincia lo genera la base de datos automáticamente.
- No puede haber dos provincias con el mismo nombre.
- El número de habitantes de una provincia es siempre mayor que 0.
- Todos los clientes tienen nombre y provincia. El DNI tiene como máximo 9 caracteres.
- El stock de un producto es 0 si no se indica y nunca es nulo.
- Todos los productos tienen nombre.
- En una venta siempre se indican la fecha y las unidades, y las unidades son mayores que 0.

Se pide: escribe el script SQL que crea la base de datos tienda y sus cuatro tablas, con todas las restricciones. Piensa en el orden en que hay que crear las tablas.


---


### Parte A. Modificar la tienda (ALTER TABLE)

La tienda crece y hay que cambiar la estructura **sin borrar las tablas**. Escribe las sentencias para:

1. Añadir a `CLIENTES` una columna `email` de hasta 80 caracteres.
2. Hacer que el `email` no se pueda repetir entre clientes.
3. Añadir a `PRODUCTOS` una columna `precio` con 2 decimales (hasta 6 cifras en total), que no sea nula y por defecto 0.
4. Añadir un `CHECK` a `PRODUCTOS` para que `precio` no sea negativo, llamándolo `ck_precio`.
5. Ampliar `nombreP` de `PRODUCTOS` a 100 caracteres, manteniendo que no sea nulo.
6. Cambiar el nombre de la columna `nombre` de `CLIENTES` por `nombre_completo`.
7. Borrar la columna `email` de `CLIENTES`.
8. Borrar el `CHECK` `ck_precio`.


---

### Parte B. Borrar con FK: ON DELETE

Partimos de la tienda del ejercicio 1, con estos datos:

```sql
INSERT INTO PROVINCIAS (provincia, habitantes) VALUES ('Jaén', 100000), ('Granada', 200000);
INSERT INTO PRODUCTOS (IdP, nombreP, stock) VALUES ('P01', 'Ratón', 50), ('P02', 'Teclado', 20);
INSERT INTO CLIENTES VALUES ('44444444X', 'Ana', 1), ('55555555H', 'Luis', 2);
INSERT INTO VENTAS VALUES
    ('44444444X', 'P01', '2024-01-10', 1),
    ('44444444X', 'P02', '2024-01-10', 2),
    ('55555555H', 'P01', '2024-01-12', 1);
```

Se ejecuta:

```sql
DELETE FROM CLIENTES WHERE DNI = '44444444X';
```

**Se pide:**

a) ¿Qué ocurre si `VENTAS` está creada **como en el ejercicio 1** (sin `ON DELETE`)?

b) Escribe cómo habría que definir la FK `DNI` de `VENTAS` para que, al borrar un cliente, se borren también sus ventas. ¿Qué filas quedarían en `VENTAS` tras el `DELETE`?

---

## Ejercicio 2: Proyectos

Pasamos a SQL el modelo relacional de una empresa (tema 7):

```
DEPARTAMENTOS (cod_dept, departamento)
    PK: cod_dept

EMPLEADOS (dni, nombre, salario, cod_dept)
    PK: dni
    FK: cod_dept → DEPARTAMENTOS(cod_dept)       [puede ser NULL]

PROYECTOS (cod_proyecto, nombre, cod_dept)
    PK: cod_proyecto
    FK: cod_dept → DEPARTAMENTOS(cod_dept)       [NOT NULL]
```

![alt text](img/ejercicio2.png)

**Condiciones del enunciado:**

- El cod_dept lo genera la base de datos automáticamente.
- Todo departamento tiene nombre y no puede haber dos con el mismo nombre.
- El DNI tiene como máximo 9 caracteres. Todo empleado tiene nombre.
- El salario de un empleado es 1200 si no se indica, nunca es nulo y siempre es mayor que 0.
- Un empleado puede no tener departamento.
- El código de proyecto tiene como máximo 5 caracteres. Todo proyecto tiene nombre y pertenece a un departamento.

**Se pide:** escribe el script SQL que crea la base de datos empresa y sus tres tablas, con todas las restricciones. Piensa en el orden en que hay que crear las tablas.

---

### Parte A. Modificar la empresa (ALTER TABLE)

La empresa crece y hay que cambiar la estructura **sin borrar las tablas**. Escribe las sentencias para:

1. Añadir a `EMPLEADOS` una columna `email` de hasta 80 caracteres.
2. Hacer que el `email` no se pueda repetir entre empleados.
3. Añadir a `DEPARTAMENTOS` una columna `presupuesto` con 2 decimales (hasta 8 cifras en total), que no sea nula y por defecto 0.
4. Añadir un `CHECK` a `DEPARTAMENTOS` para que `presupuesto` no sea negativo, llamándolo `ck_presupuesto`.
5. Ampliar `nombre` de `EMPLEADOS` a 80 caracteres, manteniendo que no sea nulo.
6. Cambiar el nombre de la columna `departamento` de `DEPARTAMENTOS` por `nombre_dept`.
7. Borrar la columna `email` de `EMPLEADOS`.
8. Borrar el `CHECK` `ck_presupuesto`.

---

### Parte B. Borrar con FK: ON DELETE SET NULL

Partimos de la empresa del ejercicio, con estos datos:

```sql
INSERT INTO DEPARTAMENTOS (departamento) VALUES ('Ventas'), ('Compras');
INSERT INTO EMPLEADOS (dni, nombre, salario, cod_dept) VALUES
    ('11111111A', 'Ana', 1500, 1),
    ('22222222B', 'Luis', 1300, 1),
    ('33333333C', 'Eva', 1400, 2),
    ('44444444D', 'Pau', 1200, NULL);
INSERT INTO PROYECTOS (cod_proyecto, nombre, cod_dept) VALUES ('PR1', 'Web', 2);
```

Se ejecuta:

```sql
DELETE FROM DEPARTAMENTOS WHERE cod_dept = 1;
```

**Se pide:**

a) ¿Qué ocurre si `EMPLEADOS` está creada **como en el ejercicio** (sin `ON DELETE`)?

b) Escribe cómo habría que definir la FK `cod_dept` de `EMPLEADOS` para que, al borrar un departamento, sus empleados **se queden sin departamento** (y no se borren). ¿Cómo quedaría la tabla `EMPLEADOS` tras el `DELETE`?

---

## Ejercicio 3: Proveedores y piezas: del modelo relacional a SQL

Pasamos a SQL el modelo relacional de un almacén de piezas (tema 7):

```
CATEGORIAS (cod_categoria, nombre)
    PK: cod_categoria

PROVEEDORES (cod_proveedor, direccion, ciudad, provincia)
    PK: cod_proveedor

PIEZAS (cod_pieza, nombre, color, precio, cod_categoria)
    PK: cod_pieza
    FK: cod_categoria → CATEGORIAS(cod_categoria)     [NOT NULL]

SUMINISTRA (cod_proveedor, cod_pieza, cantidad, fecha)
    PK: (cod_proveedor, cod_pieza)
    FK: cod_proveedor → PROVEEDORES(cod_proveedor)
    FK: cod_pieza → PIEZAS(cod_pieza)
```

![alt text](img/ejercicio3.png)

**Condiciones del enunciado:**

- Los códigos son de texto de hasta 5 caracteres (`C1`, `P01`, `A101`…).
- Toda categoría tiene nombre y no puede haber dos con el mismo nombre.
- De un proveedor, la dirección es obligatoria. La ciudad y la provincia pueden quedar vacías.
- Toda pieza tiene nombre y precio (con 2 decimales, hasta 6 cifras en total, nunca negativo). El color puede quedar vacío.
- Toda pieza pertenece a una categoría.
- En un suministro, la cantidad es obligatoria y mayor que 0. Si no se indica la fecha, se usa la fecha de hoy.

**Se pide:** escribe el script SQL que crea la base de datos `almacen` y sus cuatro tablas, con todas las restricciones. Piensa en el orden en que hay que crear las tablas.

---

### Parte A. Modificar el almacén (ALTER TABLE)

El almacén crece y hay que cambiar la estructura **sin borrar las tablas**. Escribe las sentencias para:

1. Añadir a `PROVEEDORES` una columna `telefono` de hasta 9 caracteres.
2. Hacer que el `telefono` no se pueda repetir entre proveedores.
3. Añadir a `PIEZAS` una columna `stock` de tipo entero, que no sea nula y por defecto 0.
4. Añadir un `CHECK` a `PIEZAS` para que `stock` no sea negativo, llamándolo `ck_stock`.
5. Ampliar `nombre` de `PIEZAS` a 100 caracteres, manteniendo que no sea nulo.
6. Cambiar el nombre de la columna `direccion` de `PROVEEDORES` por `domicilio`.
7. Borrar la columna `telefono` de `PROVEEDORES`.
8. Borrar el `CHECK` `ck_stock`.

---

### Parte B. Borrar con FK: ON DELETE

Partimos del almacén del ejercicio, con estos datos:

```sql
INSERT INTO CATEGORIAS VALUES ('C1', 'Periféricos'), ('C2', 'Pantallas');
INSERT INTO PROVEEDORES (cod_proveedor, direccion) VALUES ('P01', 'C/ Mayor 1'), ('P02', 'C/ Sol 5');
INSERT INTO PIEZAS (cod_pieza, nombre, precio, cod_categoria) VALUES
    ('A101', 'Ratón', 9.99, 'C1'),
    ('A102', 'Monitor', 129.90, 'C2');
INSERT INTO SUMINISTRA (cod_proveedor, cod_pieza, cantidad, fecha) VALUES
    ('P01', 'A101', 20, '2025-03-01'),
    ('P01', 'A102', 5, '2025-03-01'),
    ('P02', 'A101', 40, '2025-03-05');
```

Se ejecuta:

```sql
DELETE FROM PROVEEDORES WHERE cod_proveedor = 'P01';
```

**Se pide:**

a) ¿Qué ocurre si `SUMINISTRA` está creada **como en el ejercicio** (sin `ON DELETE`)?

b) Escribe cómo habría que definir la FK `cod_proveedor` de `SUMINISTRA` para que, al borrar un proveedor, se borren también sus suministros. ¿Qué filas quedarían en `SUMINISTRA` tras el `DELETE`?
