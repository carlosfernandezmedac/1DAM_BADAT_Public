# Tema 8. El lenguaje SQL: crear bases de datos y tablas (DDL)

> Trabajamos con **MySQL 8** y **MySQL Workbench**. Todo el tema usa el mismo ejemplo: la **tienda** de clientes, productos y ventas del tema 7.

## Índice

1. [Qué es SQL](#1-qué-es-sql)
2. [Normas de escritura](#2-normas-de-escritura)
3. [Bases de datos](#3-bases-de-datos)
4. [Tipos de datos](#4-tipos-de-datos)
5. [Crear tablas](#5-crear-tablas)
6. [Restricciones](#6-restricciones)
7. [Claves foráneas en detalle](#7-claves-foráneas-en-detalle)
8. [Orden de creación y borrado](#8-orden-de-creación-y-borrado)
9. [Modificar y borrar tablas](#9-modificar-y-borrar-tablas)
10. [Insertar datos para probar](#10-insertar-datos-para-probar)
11. [Errores típicos de MySQL](#11-errores-típicos-de-mysql)
12. [Workbench: ingeniería directa e inversa](#12-workbench-ingeniería-directa-e-inversa)
13. [Ejemplo completo: la tienda](#13-ejemplo-completo-la-tienda)
14. [Resumen](#14-resumen)

---

## 1. Qué es SQL

**SQL** (*Structured Query Language*) es el lenguaje que usamos para hablar con una base de datos relacional. Se divide en sublenguajes según lo que hace cada sentencia:

| Sublenguaje | Nombre | Para qué sirve | Sentencias |
|---|---|---|---|
| **DDL** | Data Definition Language | **Crear y modificar la estructura** (bases de datos, tablas) | `CREATE`, `ALTER`, `DROP` |
| **DML** | Data Manipulation Language | Insertar, modificar y borrar **datos** | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Consultar datos | `SELECT` |
| **DCL** | Data Control Language | Permisos de usuarios | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Transacciones | `COMMIT`, `ROLLBACK` |

En este tema usamos **DDL** (creamos la estructura) y un poco de **DML** (`INSERT`) para comprobar que las restricciones funcionan.

**Idea clave:** el diagrama E/R y el modelo relacional del tema 7 son el *plano*. El DDL es la *obra*: cada tabla del modelo relacional se convierte en un `CREATE TABLE`.

---

## 2. Normas de escritura

- Cada sentencia termina en **punto y coma** `;`.
- Las palabras reservadas (`CREATE`, `TABLE`…) no distinguen mayúsculas de minúsculas, pero las escribimos **en mayúsculas** para leer mejor.
- Los espacios y saltos de línea no importan: escribimos una columna por línea.
- **Comentarios:**

```sql
-- Comentario de una línea (¡hay que dejar un espacio después de --!)
# También vale la almohadilla
/* Comentario
   de varias líneas */
```

- Los nombres de tablas y columnas: sin espacios ni tildes, sin empezar por número (`id_provincia`, no `id provincia`).


> ⚠️ **Error muy típico:** escribir `--comentario` sin espacio. MySQL da error de sintaxis. Tiene que ser `-- comentario`.

---

## 3. Bases de datos

```sql
CREATE DATABASE tienda;              -- crea la base de datos
CREATE DATABASE IF NOT EXISTS tienda; -- la crea solo si no existe (evita error)

SHOW DATABASES;                      -- lista las bases de datos
USE tienda;                          -- selecciona la base de datos con la que trabajamos

DROP DATABASE tienda;                -- ¡borra la base de datos con todo lo que contiene!
DROP DATABASE IF EXISTS tienda;      -- la borra solo si existe
```

**Importante:** después de `CREATE DATABASE` hay que hacer `USE`. Si no, las tablas no saben en qué base de datos crearse y MySQL da el error *No database selected*.

> 💡 En clase, para repetir un ejercicio desde cero, empieza el script con `DROP DATABASE IF EXISTS tienda;`.

---

## 4. Tipos de datos

Cada columna tiene un **tipo**, que limita qué valores admite. Los más usados en MySQL:

| Tipo | Qué guarda | Ejemplo |
|---|---|---|
| `INT` | Número entero | `25`, `-3` |
| `DECIMAL(p,s)` | Número con decimales **exactos**. `p` = cifras totales, `s` = decimales | `DECIMAL(8,2)` → `123456.78` |
| `FLOAT`, `DOUBLE` | Decimales aproximados (científicos) | `3.14159` |
| `VARCHAR(n)` | Texto de longitud **variable**, hasta `n` caracteres | `'Ana'` |
| `CHAR(n)` | Texto de longitud **fija** `n` | `'ES'` |
| `TEXT` | Texto largo (descripciones, comentarios) | |
| `DATE` | Fecha (`'AAAA-MM-DD'`) | `'2024-01-10'` |
| `TIME` | Hora (`'HH:MM:SS'`) | `'17:45:05'` |
| `DATETIME` | Fecha y hora | `'2024-01-10 17:45:05'` |
| `BOOLEAN` | Verdadero/falso (en realidad `TINYINT(1)`: 0 o 1) | `TRUE`, `FALSE` |

**Reglas prácticas:**

- **Dinero → `DECIMAL`**, nunca `FLOAT` (el `FLOAT` redondea y pierde céntimos).
- **Identificadores que no se operan** (DNI, teléfono, código postal) → **texto**, no `INT`. No se suman ni se restan, y un teléfono puede empezar por 0.
- **`VARCHAR` o `CHAR`:** `CHAR(9)` reserva siempre 9 caracteres; `VARCHAR(9)` ocupa solo lo que se escribe. En la asignatura usamos **`VARCHAR`** por defecto.
- Las **fechas** se escriben entre comillas y con formato **año-mes-día**: `'2024-01-10'`.
- Las comillas son **simples** `' '` para texto y fechas.

---

## 5. Crear tablas

```sql
CREATE TABLE nombre_tabla (
    columna1  TIPO  [restricciones],
    columna2  TIPO  [restricciones],
    ...
    [restricciones de tabla]
);
```

Ejemplo mínimo:

```sql
CREATE TABLE PRODUCTOS (
    IdP      VARCHAR(5),
    nombreP  VARCHAR(50),
    stock    INT
);
```

Comandos para comprobar lo que hemos creado:

```sql
SHOW TABLES;                 -- lista las tablas de la base de datos
DESCRIBE PRODUCTOS;          -- muestra las columnas, tipos y claves
SHOW CREATE TABLE PRODUCTOS; -- muestra el CREATE TABLE completo (con restricciones)
```

Cada columna se escribe como **nombre + tipo + restricciones**, separadas por coma. **La última línea no lleva coma.**

---

## 6. Restricciones

Una **restricción** es una regla que la base de datos obliga a cumplir. Si intentamos guardar un dato que la incumple, MySQL **rechaza** la operación con un error.

![tablas de la tienda](img/01_tienda_tablas.png)

| Restricción | Qué obliga | Lo deducimos de… |
|---|---|---|
| `PRIMARY KEY` | Identifica cada fila: único y no nulo | La clave del modelo relacional |
| `FOREIGN KEY` | El valor debe existir en la tabla referenciada | Las FK del modelo relacional |
| `NOT NULL` | La columna no puede quedar vacía | Cardinalidad mínima 1 / enunciado |
| `UNIQUE` | No se pueden repetir valores | Relación 1:1 / enunciado |
| `DEFAULT` | Valor por defecto si no se indica | Enunciado |
| `CHECK` | El valor cumple una condición | Enunciado |
| `AUTO_INCREMENT` | El número se genera solo (1, 2, 3…) | Ids numéricos |

### 6.1 NOT NULL

```sql
nombreP VARCHAR(50) NOT NULL
```

Sin `NOT NULL`, la columna **admite NULL** (valor vacío). `NULL` no es lo mismo que `0` ni que `''`: significa *"no se sabe / no hay dato"*.

### 6.2 UNIQUE

```sql
provincia VARCHAR(30) NOT NULL UNIQUE
```

No permite dos filas con el mismo valor. Una columna `UNIQUE` sin `NOT NULL` sí admite varios `NULL`.

### 6.3 PRIMARY KEY

**Clave simple** (una columna), en la propia columna:

```sql
IdP VARCHAR(5) PRIMARY KEY
```

**Clave compuesta** (varias columnas), al final de la tabla:

```sql
PRIMARY KEY (DNI, IdP)
```

La PK es siempre `NOT NULL` y `UNIQUE` sin tener que escribirlo. Solo puede haber **una PK por tabla**.

### 6.4 AUTO_INCREMENT

```sql
id_provincia INT AUTO_INCREMENT PRIMARY KEY
```

- MySQL pone el número **solo**, empezando en 1.
- **No se indica el id** al insertar: se omite la columna.
- La columna tiene que ser clave (normalmente la PK) y solo puede haber una por tabla.
- Si se borra una fila, **ese número no se reutiliza**.

### 6.5 DEFAULT

```sql
stock INT NOT NULL DEFAULT 0
fechaVenta DATE DEFAULT (CURRENT_DATE)   -- fecha de hoy
```

- Si al insertar **no escribimos** la columna, toma el valor por defecto.
- Si escribimos `NULL` explícitamente en una columna `NOT NULL`, **da error** aunque tenga `DEFAULT`.
- Para fechas, la función va **entre paréntesis**: `DEFAULT (CURRENT_DATE)`.

### 6.6 CHECK

```sql
habitantes INT CHECK (habitantes > 0)
CHECK (CHAR_LENGTH(DNI) = 9)
```

Obliga a que se cumpla una condición. La condición va **entre paréntesis**.

### 6.7 FOREIGN KEY

```sql
FOREIGN KEY (id_provincia) REFERENCES PROVINCIAS(id_provincia)
```

Se lee: *"la columna `id_provincia` de esta tabla tiene que existir en `PROVINCIAS(id_provincia)`"*.

**Requisitos:**

1. La **tabla referenciada tiene que existir ya**.
2. La columna referenciada debe ser **PK** (o `UNIQUE`).
3. Las dos columnas tienen que tener **el mismo tipo** (`INT` con `INT`, `VARCHAR(9)` con `VARCHAR(9)`).

**¿FK nula o `NOT NULL`?** Una FK **puede ser nula** salvo que se indique lo contrario:

| Situación en el diagrama | FK |
|---|---|
| Cardinalidad mínima **1** (obligatoria) | `NOT NULL` |
| Cardinalidad mínima **0** (opcional) | puede ser nula |
| La FK forma parte de la **PK** | `NOT NULL` automáticamente |

### Nombrar las restricciones (opcional)

Se puede poner nombre con `CONSTRAINT`. Es útil para borrarlas luego con `ALTER TABLE`:

```sql
CONSTRAINT fk_cliente_provincia
    FOREIGN KEY (id_provincia) REFERENCES PROVINCIAS(id_provincia)
```

Si no se pone nombre, MySQL inventa uno (`clientes_ibfk_1`, `clientes_chk_1`…).

### Restricciones de columna y de tabla

- **De columna:** se escriben junto a la columna (`NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK`, PK simple).
- **De tabla:** se escriben al final, después de todas las columnas (PK compuesta, `FOREIGN KEY`, `CHECK` con varias columnas).

---

## 7. Claves foráneas en detalle

¿Qué pasa si borramos una fila que otras tablas referencian? Por ejemplo, si borramos el cliente Ana, ¿qué pasa con sus ventas? Lo decide la acción `ON DELETE` (y `ON UPDATE` si se cambia la clave).

```sql
FOREIGN KEY (DNI) REFERENCES CLIENTES(DNI)
    ON DELETE CASCADE
    ON UPDATE CASCADE
```

| Acción | Qué hace al borrar/modificar la fila padre |
|---|---|
| `RESTRICT` / `NO ACTION` | **Lo impide** con un error. Es lo que pasa **si no se indica nada** |
| `CASCADE` | **Borra (o actualiza) también** las filas hijas |
| `SET NULL` | Pone **NULL** en la FK de las filas hijas |
| `SET DEFAULT` | MySQL (InnoDB) **no lo admite** |

**Ejemplo con la tienda.** Ana (`44444444X`) tiene dos ventas. Ejecutamos:

```sql
DELETE FROM CLIENTES WHERE DNI = '44444444X';
```

| Definición de la FK de `VENTAS.DNI` | Resultado |
|---|---|
| (sin nada, o `RESTRICT`) | ❌ Error 1451: no se puede borrar, hay ventas que la usan |
| `ON DELETE CASCADE` | ✅ Se borra Ana **y sus dos ventas** |
| `ON DELETE SET NULL` | ❌ No se puede ni crear la tabla: `DNI` es parte de la PK y no puede ser nulo |

**¿Cuál elegir?**

- Por defecto, usamos **`RESTRICT`** (no escribimos nada): protege los datos.
- `CASCADE` es cómodo pero **peligroso**: un solo `DELETE` puede borrar muchísimas filas en cadena (borrar una provincia podría borrar sus clientes y todas sus ventas).
- `SET NULL` solo sirve si la FK **admite nulos** (cardinalidad mínima 0).

---

## 8. Orden de creación y borrado

Como una FK solo puede apuntar a una tabla que **ya existe**, hay un orden:

**Crear:** primero las tablas **padre** (las que no tienen FK o las referenciadas), después las **hijas**.

```
PROVINCIAS, PRODUCTOS   →   CLIENTES   →   VENTAS
```

**Borrar:** al revés. Primero las **hijas**, después las padre.

```sql
DROP TABLE VENTAS;
DROP TABLE CLIENTES;
DROP TABLE PRODUCTOS;
DROP TABLE PROVINCIAS;
```

Si intentamos borrar una tabla padre mientras otra la referencia, MySQL da error. Con `DROP TABLE IF EXISTS nombre;` no falla si la tabla no existe.

---

## 9. Modificar y borrar tablas

`ALTER TABLE` cambia la estructura de una tabla que ya existe.

```sql
-- Añadir una columna
ALTER TABLE CLIENTES ADD COLUMN email VARCHAR(80);
ALTER TABLE CLIENTES ADD COLUMN telefono VARCHAR(9) AFTER nombre;   -- colocarla tras otra

-- Cambiar el tipo o las restricciones de una columna (hay que repetir TODO)
ALTER TABLE CLIENTES MODIFY COLUMN nombre VARCHAR(80) NOT NULL;

-- Cambiar el nombre de una columna
ALTER TABLE CLIENTES RENAME COLUMN nombre TO nombre_completo;

-- Borrar una columna
ALTER TABLE CLIENTES DROP COLUMN email;

-- Añadir restricciones
ALTER TABLE CLIENTES ADD CONSTRAINT uq_email UNIQUE (email);
ALTER TABLE PRODUCTOS ADD CONSTRAINT ck_stock CHECK (stock >= 0);
ALTER TABLE VENTAS ADD CONSTRAINT fk_ventas_idp
    FOREIGN KEY (IdP) REFERENCES PRODUCTOS(IdP);

-- Quitar restricciones
ALTER TABLE CLIENTES DROP INDEX uq_email;                 -- UNIQUE
ALTER TABLE PRODUCTOS DROP CHECK ck_stock;                -- CHECK
ALTER TABLE VENTAS DROP FOREIGN KEY fk_ventas_idp;        -- FOREIGN KEY

-- Cambiar el nombre de la tabla
RENAME TABLE CLIENTES TO CLIENTES_TIENDA;
```

> ⚠️ Con `MODIFY` hay que **escribir de nuevo todas** las propiedades de la columna (`NOT NULL`, `DEFAULT`…). Lo que no escribas se pierde.

> ⚠️ Para borrar una restricción necesitas su **nombre**. Si no se lo pusimos, lo vemos con `SHOW CREATE TABLE`.

**Borrar una tabla** (se pierden la estructura y todos sus datos, no se puede deshacer):

```sql
DROP TABLE VENTAS;
DROP TABLE IF EXISTS VENTAS;
```

---

## 10. Insertar datos para probar

Para comprobar que nuestras restricciones funcionan, necesitamos insertar datos. Es DML, pero lo necesitamos ya.

```sql
-- Indicando las columnas (el orden lo decides tú; las omitidas toman DEFAULT / AUTO_INCREMENT)
INSERT INTO PROVINCIAS (provincia, habitantes) VALUES ('Jaén', 100000);

-- Varias filas a la vez
INSERT INTO PROVINCIAS (provincia, habitantes) VALUES
    ('Granada', 200000),
    ('Málaga', 500000);

-- Sin indicar columnas: hay que dar TODOS los valores y en el orden de la tabla
INSERT INTO PRODUCTOS VALUES ('P01', 'Ratón', 50);
```

Para ver el contenido y para borrar:

```sql
SELECT * FROM PROVINCIAS;
DELETE FROM CLIENTES WHERE DNI = '44444444X';
```

**Orden de inserción:** el mismo que el de creación. Primero `PROVINCIAS` y `PRODUCTOS`, luego `CLIENTES`, y por último `VENTAS`. No se puede insertar un cliente en una provincia que no existe.

---

## 11. Errores típicos de MySQL

| Error | Mensaje | Causa más habitual |
|---|---|---|
| **1064** | You have an error in your SQL syntax | Falta una coma o un `;`, `CHECK` sin paréntesis, `--` sin espacio, coma sobrante en la última columna |
| **1046** | No database selected | Falta `USE nombre_bd;` |
| **1050** | Table already exists | La tabla ya existe (usa `DROP TABLE IF EXISTS`) |
| **1215** | Cannot add foreign key constraint | La tabla padre **no existe aún**, o los **tipos** de las dos columnas no coinciden, o la columna referenciada no es PK/UNIQUE |
| **1062** | Duplicate entry '…' for key | Valor repetido en una PK o `UNIQUE` |
| **1048** | Column '…' cannot be null | Se escribió `NULL` en una columna `NOT NULL` |
| **1364** | Field '…' doesn't have a default value | Falta una columna `NOT NULL` sin `DEFAULT` en el `INSERT` |
| **1452** | Cannot add or update a child row | La FK apunta a una fila que **no existe** en la tabla padre |
| **1451** | Cannot delete or update a parent row | Se intenta borrar una fila que **otras tablas referencian** |
| **3819** | Check constraint '…' is violated | El valor incumple un `CHECK` |
| **1830** | Column cannot be NOT NULL: needed in a foreign key constraint SET NULL | `ON DELETE SET NULL` sobre una columna `NOT NULL` o que es PK |

**Para no confundir 1452 y 1451:** los dos hablan de FK.

- **1452** (*child row*): estás **insertando o modificando en la hija** y el padre no existe.
- **1451** (*parent row*): estás **borrando o modificando en el padre** y hay hijas que lo usan.

---

## 12. Workbench: ingeniería directa e inversa

**Ejecutar un script.** Abre una pestaña SQL (`File → New Query Tab`), pega el script y pulsa el rayo ⚡ para ejecutar todo. Si pulsas el rayo con el cursor y una sentencia seleccionada, solo ejecuta esa.

**Ver el resultado.** En el panel izquierdo (*Schemas*), pulsa el botón de refrescar. Aparecerá la base de datos, y dentro **Tables**.

**Ingeniería inversa** (de la base de datos al diagrama): sirve para comprobar que nuestro SQL crea lo que dibujamos.

1. `Database → Reverse Engineer…`
2. Selecciona la conexión y la base de datos (`tienda`).
3. Siguiente hasta terminar, marcando todas las tablas.
4. Se abre el diagrama con las tablas y las flechas de las FK.
5. Para guardarlo: `File → Export → Export as PDF`.

**Ingeniería directa** (del diagrama al SQL): en un modelo (`File → New Model`) dibujas las tablas y luego `Database → Forward Engineer…` genera el script `CREATE TABLE`.

---

