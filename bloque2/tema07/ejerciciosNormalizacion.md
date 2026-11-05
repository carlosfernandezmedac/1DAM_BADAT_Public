# Ejercicios — Normalización
# Tema 7: de la tabla plana a 3FN

---

## Ejercicio 1 — Alumnos, asignaturas y profesores

Un centro guarda las notas de sus alumnos en una única tabla, `ALUMNOS_NOTAS`:

| DNI_alumno | Nombre | Apellido | Asignaturas | Códigos_asig | Nota | Profesor | email_profesor |
|---|---|---|---|---|---|---|---|
| 11111111X | Iván | Castillo | Lenguajes de marcas, Bases de datos | LMSGI, BBDD | 7, 1 | Carlos Fernández | carlos@medac.es |
| 22222222H | Nuria | Delgado | Programación | PROG | 3 | Daniel Montañes | daniel@medac.es |
| 33333333Z | Marta | Macías | Entornos de Desarrollo | END | 8 | Celia Macías | celia@medac.es |

*Cada asignatura la imparte un único profesor, pero un profesor puede impartir varias asignaturas.*

Normaliza la tabla hasta cumplir la Tercera Forma Normal (3FN). Muestra todas las tablas resultantes y sus claves primarias.

---

## Ejercicio 2 — Socios y actividades de un gimnasio

Un gimnasio guarda las inscripciones de sus socios en una única tabla, `SOCIOS_ACTIVIDADES`:

| id_socio | nombre | cod_actividad | actividad | cod_ciudad | ciudad | fecha_alta |
|---|---|---|---|---|---|---|
| S001 | Lucía | A01 | Yoga | 23 | Jaén | 10/01/2026 |
| S001 | Lucía | A02 | Pilates | 23 | Jaén | 15/02/2026 |
| S002 | Pablo | A02 | Pilates | 41 | Sevilla | 03/03/2026 |
| S002 | Pablo | A03 | Spinning | 41 | Sevilla | 03/03/2026 |
| S003 | Lucía | A01 | Yoga | 41 | Sevilla | 21/04/2026 |

*Cada socio vive en una única ciudad (la que indica su código) y puede estar inscrito en varias actividades. `fecha_alta` es la fecha en que el socio se apuntó a esa actividad. La clave primaria de la tabla es `(id_socio, cod_actividad)`.*

**Se pide:** normaliza la tabla hasta cumplir la **Tercera Forma Normal (3FN)**. Muestra todas las tablas resultantes y sus claves primarias, indicando el paso en que se obtiene cada una (1FN, 2FN y 3FN).

---

## Ejercicio 3 — Ventas de una tienda de informática

Una tienda de informática guarda sus ventas en una única tabla, `VENTAS_TIENDA`:

| DNI | IdP | NombreP | Stock | Nombre | Provincia | FechaVenta | Unds | Habitantes |
|---|---|---|---|---|---|---|---|---|
| 44444444X | P01, P02 | Ratón, Auricular | 50, 100 | Ana | Jaén | 2024-01-10 | 1, 2 | 100000 |
| 55555555H | P03 | Teclado | 20 | Luis | Granada | 2024-01-11 | 3 | 200000 |
| 66666666N | P04 | Monitor | 10 | Marta | Málaga | 2024-01-12 | 1 | 500000 |

*Cada cliente se identifica por su DNI y vive en una única provincia. Cada producto se identifica por su `IdP` y tiene un `Stock` propio. `FechaVenta` y `Unds` son la fecha y las unidades de esa venta de ese producto a ese cliente. `Habitantes` es la población de la provincia. Las claves primarias de la tabla es `(DNI, IdP)`.*


**Se pide:** normaliza la tabla hasta cumplir la **Tercera Forma Normal (3FN)**. Muestra todas las tablas resultantes y sus claves primarias (y claves foráneas), indicando el paso en que se obtiene cada una (1FN, 2FN y 3FN).


