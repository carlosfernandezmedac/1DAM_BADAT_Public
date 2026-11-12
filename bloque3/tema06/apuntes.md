# Tema 6. El modelo relacional: repaso de conceptos

---

## Índice

1. [Introducción](#1-introducción)
2. [Dominios y tipos de datos (MySQL)](#2-dominios-y-tipos-de-datos-mysql)
3. [Claves](#3-claves)
4. [Restricciones de integridad](#4-restricciones-de-integridad)
5. [Del modelo relacional a SQL: mapa rápido](#5-del-modelo-relacional-a-sql-mapa-rápido)
6. [Resumen](#6-resumen)

---

## 1. Introducción

Este tema es un **repaso**. Los pasos para pasar del E/R al modelo relacional ya los has visto (Tema 7) y el SQL para crearlo lo verás en el Tema 8.

Aquí fijamos el **vocabulario que vamos a usar en el Tema 8**: los tipos de datos con los que se define cada columna, los distintos tipos de clave (primaria, alternativa, foránea) y las reglas de integridad, incluido qué ocurre al borrar un registro referenciado.

---

## 2. Dominios y tipos de datos (MySQL)

El **dominio** es el conjunto de valores que puede tomar un atributo. En MySQL se define con el **tipo de datos** y, si hace falta, restricciones (`CHECK`, `NOT NULL`...).

| Qué guardo | Tipo | Ejemplo | Notas |
|---|---|---|---|
| Entero | `INT` | stock, unidades |  |
| Decimal exacto | `DECIMAL(p,s)` | precio `DECIMAL(8,2)` | `p` = cifras totales, `s` = decimales. |
| Decimal aproximado | `FLOAT`, `DOUBLE` | medidas | Pueden redondear|
| Texto variable | `VARCHAR(n)` | nombre | Ocupa lo que se escribe, hasta `n` |
| Texto fijo | `CHAR(n)` | DNI, código postal | Siempre `n` caracteres |
| Texto largo | `TEXT` | descripción | Sin límite práctico |
| Fecha | `DATE` | 2024-01-10 | Formato `AAAA-MM-DD` |
| Fecha y hora | `DATETIME` | 2024-01-10 18:30:00 | |
| Sí/No | `BOOLEAN` | activo | En MySQL es un `TINYINT(1)` (0/1) |

**Cómo elegir:**
- ¿Voy a hacer cuentas con él? → número. Un teléfono o un código postal **no** se suma: va como texto (`VARCHAR`/`CHAR`), así no se pierde el 0 inicial.
- ¿Tiene siempre la misma longitud? → `CHAR`. ¿Varía? → `VARCHAR`.
- ¿Es dinero? → `DECIMAL`.

**Valor nulo (`NULL`):** significa "no se sabe / no aplica". **No es 0 ni cadena vacía.** Con NULL no se puede comparar con `=` (se usa `IS NULL`).

## 3. Claves

Una **clave** es un atributo (o conjunto) que identifica de forma única cada fila.

| Tipo | Definición | Ejemplo en CLIENTES(DNI, email, nombre) |
|---|---|---|
| **Superclave** | Cualquier conjunto que identifica filas únicas | (DNI), (DNI, nombre), (email)... |
| **Clave candidata** | Superclave **mínima** (no se le puede quitar nada) | DNI, email |
| **Clave primaria (PK)** | La candidata que elegimos como identificador | DNI |
| **Clave alternativa** | Las candidatas que no se eligieron | email (`UNIQUE NOT NULL`) |
| **Clave foránea (FK)** | Atributo que referencia la PK de otra tabla | id_provincia → PROVINCIAS |

**Reglas de la PK:**
- Única y **nunca NULL**.
- **Estable:** su valor no debería cambiar nunca, porque otras tablas lo copian como FK. Por eso se prefiere un DNI o un id a un email o un nombre.
- **Corta:** un INT o un código pequeño ocupa menos y se compara más rápido que un texto largo.
- Si ninguna columna sirve, se crea una **clave artificial** (`id` con `AUTO_INCREMENT`).
- Puede ser **compuesta** (varias columnas). Ej.: en VENTAS la PK es (DNI, IdP), porque un mismo cliente puede comprar varios productos y un mismo producto lo pueden comprar varios clientes. Es lo que sale de una relación N:M.

**Reglas de la FK:**
- Su valor debe existir en la PK a la que apunta, **o ser NULL**.
- Puede repetirse (muchos clientes de la misma provincia).
- Es NULL-able salvo que esté en la PK o se declare `NOT NULL` (relación con participación mínima 1).
- Debe tener **el mismo tipo** que la PK a la que referencia.

## 4. Restricciones de integridad

Son las reglas que impiden que los datos queden incoherentes. El SGBD las comprueba solo, en cada `INSERT`, `UPDATE` y `DELETE`.

| Regla | Qué exige | Si se viola, MySQL... |
|---|---|---|
| **Integridad de dominio** | El valor es del tipo y cumple el `CHECK` | rechaza (1366 / 3819) |
| **Integridad de entidad** | La PK es única y no nula | rechaza (1062 / 1048) |
| **Integridad referencial** | Toda FK apunta a una PK que existe (o es NULL) | rechaza (1452 / 1451) |
| **Unicidad** (`UNIQUE`) | No se repite el valor | rechaza (1062) |
| **Obligatoriedad** (`NOT NULL`) | Debe tener valor | rechaza (1048 / 1364) |

### 4.1 Integridad referencial: qué pasa al borrar o modificar

Si una fila "padre" (PROVINCIAS) tiene filas "hijas" (CLIENTES), ¿qué hacemos al borrarla? Es la cláusula `ON DELETE` (y `ON UPDATE` si cambia la PK):

| Opción | Efecto sobre las filas hijas | Cuándo usarla |
|---|---|---|
| `RESTRICT` / `NO ACTION` (por defecto) | **Prohíbe** borrar al padre mientras tenga hijas | Lo más seguro: datos que no deben perderse |
| `CASCADE` | **Borra** también las hijas | Datos que no tienen sentido sin el padre (los préstamos de un socio) |
| `SET NULL` | Pone la FK a **NULL** en las hijas | Cuando la relación es opcional. La columna debe admitir NULL |
| `SET DEFAULT` | Valor por defecto | **No lo soporta InnoDB** (MySQL) |

#### Ejemplo: `ON DELETE CASCADE`

Se indica al crear la clave foránea:

```sql
CREATE TABLE CLIENTES (
    DNI           VARCHAR(9) PRIMARY KEY,
    nombre        VARCHAR(50) NOT NULL,
    id_provincia  INT NOT NULL,
    FOREIGN KEY (id_provincia) REFERENCES PROVINCIAS(id_provincia)
        ON DELETE CASCADE
);
```

Con estos datos:

| PROVINCIAS | | CLIENTES | | |
|---|---|---|---|---|
| **id_provincia** | provincia | **DNI** | nombre | id_provincia |
| 1 | Jaén | 44444444X | Ana | 1 |
| 2 | Granada | 55555555H | Luis | 2 |
| | | 66666666N | Marta | 1 |

Ejecutamos:

```sql
DELETE FROM PROVINCIAS WHERE id_provincia = 1;
```

Al borrar Jaén, MySQL borra **automáticamente** a todos los clientes que apuntaban a ella (Ana y Marta)

Ejemplos: al borrar la provincia Jaén,
- con `RESTRICT` → error 1451 si hay clientes de Jaén;
- con `CASCADE` → se borran también sus clientes
- con `SET NULL` → se borraría Jaén y Ana y Marta quedarían sin provincia.


## 5. Del modelo relacional a SQL: mapa rápido

| En el modelo relacional | En SQL (Tema 8) |
|---|---|
| Dominio | Tipo de datos, `CHECK` |
| Atributo obligatorio | `NOT NULL` |
| Clave primaria (simple/compuesta) | `PRIMARY KEY (...)` |
| Clave alternativa | `UNIQUE` (+ `NOT NULL`) |
| Clave foránea | `FOREIGN KEY ... REFERENCES ...` |
| Qué hacer al borrar | `ON DELETE ...` |
| Clave artificial | `INT AUTO_INCREMENT PRIMARY KEY` |

