# Casos Prácticos — Tema 3
# Sistemas Gestores de Bases de Datos (SGBD)

---

## Caso Práctico 1 — Niveles ANSI/X3/SPARC en la tienda

La tienda de nuestro amigo ya tiene su base de datos funcionando con MySQL. 

**Describe qué habría en cada nivel de la arquitectura ANSI/X3/SPARC para este caso concreto.**

<details>
<summary>Ver solución</summary>

**Nivel interno (físico):**

MySQL guarda los datos en ficheros internos en disco. En nuestro caso los almacenaríamos en un **NAS con RAID 1** para garantizar la redundancia — tal y como decidimos en el Tema 1.

```
/var/lib/mysql/tienda/
├── productos.ibd
├── clientes.ibd
├── ventas.ibd
├── compras.ibd
└── proveedores.ibd
```

**Nivel conceptual (lógico):**

La estructura de la base de datos — las tablas y sus campos:

```
PRODUCTOS        CLIENTES          PROVEEDORES
─────────        ────────          ───────────
ID producto      ID cliente        ID proveedor
Concepto         Nombre            Nombre
Unidades         Apellidos         Apellidos
                 Dirección         Dirección
                 Teléfono          Teléfono

VENTAS           COMPRAS
──────           ───────
ID venta         ID compra
ID producto      ID producto
Unidades         Unidades
Precio           Precio
Fecha            Fecha
ID cliente       ID proveedor
```

**Nivel externo (usuarios):**

Cada perfil de usuario ve solo lo que necesita:

| Perfil | Qué ve |
|--------|--------|
| Administrador | Todo — tablas, usuarios, configuración |
| Personal de tienda | Productos, clientes y ventas |
| Personal de almacén | Productos, proveedores y compras |
| Usuario consulta | Solo puede consultar, no modificar |

</details>

---

## Caso Práctico 2 — ¿Qué SGBD usar?

Nuestro amigo necesita elegir un SGBD para su tienda. El negocio es pequeño, con presupuesto limitado, y necesita algo fácil de instalar y mantener.

**Analiza las opciones y justifica cuál recomendarías.**

<details>
<summary>Ver solución</summary>

| SGBD | ¿Adecuado? | Justificación |
|------|-----------|---------------|
| Access | No | No escala, no multiplataforma |
| SQLite | No | Sin servidor, sin claves foráneas — demasiado limitado |
| **MySQL** | **Sí** | Gratuito, fácil de instalar, ampliamente documentado, más que suficiente |
| PostgreSQL | Posible | Más potente que MySQL pero más complejo para un negocio pequeño |
| Oracle XE | Posible | Gratuito pero más complejo de configurar |
| MongoDB | No | NoSQL — no encaja con la estructura relacional que necesitamos |

**Conclusión:**

- **MySQL** es la mejor opción → gratuito, fácil de gestionar, con amplia comunidad de soporte.
- La arquitectura será **bicapa** → MySQL instalado en un servidor y el personal se conecta desde sus PCs.
- Si en el futuro el negocio crece → PostgreSQL u Oracle según las necesidades.

</details>

---

