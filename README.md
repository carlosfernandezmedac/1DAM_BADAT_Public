# Bases de Datos — 1DAM

Apuntes, ejemplos y ejercicios de la asignatura **Bases de Datos** de 1º DAM.

---

## Contenidos

### 1er Trimestre

| Bloque | Temas | Contenido |
|--------|-------|-----------|
| [Bloque 1 — Conceptos Iniciales](bloque1/README.md) | 1, 2, 3 | Ficheros, fundamentos de BBDD, SGBD |
| [Bloque 2 — Diseño E/R](bloque2/README.md) | 4, 5, 7 | Modelo E/R básico, E/R extendido, modelo relacional y normalización |
| [Bloque 3 — DDL](bloque3/README.md) | 6, 8 | Tipos de datos, restricciones, creación de tablas |

### 2º Trimestre

| Bloque | Temas | Contenido |
|--------|-------|-----------|
| [Bloque 4 — DML: Consultas SQL](bloque4/README.md) | 9, 10, 11 | SELECT, funciones, JOINs |
| [Bloque 5 — DML, DCL y TCL](bloque5/README.md) | 12, 13, 14 | INSERT, UPDATE, DELETE, permisos, transacciones |

### 3er Trimestre

| Bloque | Temas | Contenido |
|--------|-------|-----------|
| [Bloque 6 — PL/SQL](bloque6/README.md) | 15, 16, 17 | Procedimientos, funciones, triggers |
| [Bloque 7 — BBDD Objeto-Relacionales](bloque7/README.md) | 18, 19, 20 | Oracle objeto-relacional |

---

## Los lenguajes de SQL

A lo largo del curso trabajarás con cuatro tipos de sentencias SQL. Es importante que sepas distinguirlas:

| Tipo | Nombre completo | Para qué sirve | Ejemplos |
|------|----------------|----------------|---------|
| **DDL** | Data Definition Language | Crear y modificar la estructura de la base de datos | `CREATE`, `ALTER`, `DROP` |
| **DML** | Data Manipulation Language | Insertar, consultar, modificar y eliminar datos | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** | Data Control Language | Controlar los permisos de acceso | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Gestionar transacciones | `COMMIT`, `ROLLBACK` |

```
Bloque 3  ──►  DDL   → primero creamos la estructura
Bloque 4  ──►  DML   → luego consultamos los datos
Bloque 5  ──►  DML + DCL + TCL  → modificamos, controlamos y protegemos
```

---

## Estructura de cada tema

- **Apuntes.md**: Resumen teórico de cada tema. 
- **casospracticos.md** — Casos prácticos con su resolución
- **ejercicios.md** — Ejercicios para practicar y desarrollados en clase


---


## Herramientas

- **MySQL Workbench** — para las prácticas de SQL (bloques 3,4,5)
- **Oracle Database** — para los bloques 6 y 7


