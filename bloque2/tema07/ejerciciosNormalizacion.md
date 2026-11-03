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


