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
INSERT INTO PRODUCTOS VALUES ('P01', 'Ratón', 50), ('P02', 'Teclado', 20);
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

