# Ejercicios de lenguajes SQL

## Ejercicio 1 — Clasifica las sentencias

Clasifica cada sentencia SQL en su tipo correcto (DDL, DML, DCL o TCL):

```sql
1.  CREATE TABLE clientes (id INT, nombre VARCHAR(50));
2.  SELECT * FROM productos;
3.  INSERT INTO ventas VALUES (1, 3, 150, '2025-01-10');
4.  GRANT SELECT ON clientes TO usuario_consulta;
5.  DROP TABLE movimientos;
6.  UPDATE productos SET unidades = 100 WHERE id = 1;
7.  COMMIT;
8.  REVOKE INSERT ON ventas FROM usuario_consulta;
9.  ALTER TABLE clientes ADD COLUMN email VARCHAR(100);
10. ROLLBACK;
11. DELETE FROM ventas WHERE fecha < '2024-01-01';
```

</details>

---

## Ejercicio 2 — Identifica los niveles ANSI/X3/SPARC

Lee las siguientes descripciones e indica a qué nivel pertenece cada una:

1. "La tabla CLIENTES tiene los campos ID, nombre, apellidos, dirección y teléfono."
2. "Los datos se almacenan en ficheros `.ibd` en la carpeta `/var/lib/mysql/tienda/`."
3. "El personal de almacén solo puede ver productos y proveedores, no los datos de clientes."
4. "La tabla VENTAS tiene los campos ID, fecha, precio e ID cliente."
5. "Los índices se guardan en bloques de 16KB en disco."

---

## Ejercicio 3 — Arquitectura de ejecución

Indica qué arquitectura (monocapa, bicapa o multicapa) corresponde a cada caso:

1. Un alumno usa SQLite en su portátil para su proyecto personal.
2. La tienda de nuestro amigo tiene MySQL en un servidor y el personal se conecta con MySQL Workbench desde sus PCs.
3. Una tienda online donde los clientes acceden desde el navegador, hay un servidor web intermedio y los datos están en otro servidor.



