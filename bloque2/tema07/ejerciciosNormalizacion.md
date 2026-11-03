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

**Se pide:**

1. **1FN.** Indica qué celdas incumplen la primera forma normal y reescribe la tabla para que la cumpla. ¿Cuál es ahora la clave primaria? Razónalo.
2. **Dependencias.** Para cada columna no clave, indica de qué depende y clasifica la dependencia (completa, parcial o transitiva).
3. **2FN.** Descompón la tabla para que cumpla la segunda forma normal. Indica PK y FK de cada tabla.
4. **3FN.** Descompón el resultado para que cumpla la tercera forma normal. Indica PK y FK de cada tabla.

> 💡 Pregúntate, por cada columna no clave: *«¿qué necesito saber para conocer su valor?»*
