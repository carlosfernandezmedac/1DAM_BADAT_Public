# Casos Prácticos — Tema 2
# Fundamentos de las Bases de Datos

---

## Caso Práctico 1 — Diseño de base de datos para tienda y almacén

Continuando con el caso de la tienda del Tema 1, ahora vamos a diseñar la base de datos que reemplazará a los ficheros. Antes de implementarla en ningún SGBD hay que diseñarla sobre el papel.

**¿Qué entidades, tablas y campos necesitamos para gestionar el almacén, las ventas y las compras?**

<details>
<summary>Ver solución</summary>

**Entidades identificadas:** Producto, Cliente y Proveedor.

**Tablas y campos:**

```
PRODUCTOS                  VENTAS                    COMPRAS
─────────                  ──────                    ───────
ID producto                ID venta                  ID compra
Concepto                   ID producto ──► PRODUCTOS  ID producto ──► PRODUCTOS
Unidades                   Unidades vendidas          Unidades compradas
                           Precio                     Precio
                           Fecha                      Fecha
                           ID cliente ──► CLIENTES    ID proveedor ──► PROVEEDORES

CLIENTES                   PROVEEDORES
────────                   ───────────
ID cliente                 ID proveedor
Nombre / Apellidos         Nombre / Apellidos
Dirección / Teléfono       Dirección / Teléfono
```

**Relaciones entre tablas:**

- Las VENTAS relacionan PRODUCTOS con CLIENTES
- Las COMPRAS relacionan PRODUCTOS con PROVEEDORES

</details>

---

## Caso Práctico 2 — ¿Qué modelo de datos usar?

Después de diseñar la estructura, hay que decidir qué **modelo de datos** es más adecuado para la tienda de nuestro amigo. El negocio es pequeño, con estructura de datos clara y sin necesidad de almacenar objetos multimedia.

**Analiza los modelos disponibles y justifica cuál usarías.**

<details>
<summary>Ver solución</summary>

| Modelo | ¿Adecuado? | Justificación |
|--------|-----------|---------------|
| Jerárquica | No | Obsoleta. Demasiado rígida. |
| En red | No | En desuso. |
| **Relacional** | **Sí** | Amplio soporte, sencillo de implementar, adecuado para el objetivo. |
| Orientada a objetos | No | Demasiado compleja para este caso. |
| Objeto-relacional | Posible | Para futuras funcionalidades más avanzadas. |
| NoSQL | No | No cumple el requisito de tener una estructura clara. |

**Conclusión:**
- **Modelo relacional** → sencillo, rápido de implementar y con amplio soporte. La mejor opción para empezar.
- **Base de datos centralizada** → al tener una única tienda no necesita distribución.
- En el futuro, si el negocio crece, se podría evolucionar hacia un modelo objeto-relacional.

</details>

---

## Ejercicios de fragmentación

### Ejercicio 1 — Fragmentación horizontal

Dada la siguiente tabla de EMPLEADOS de una empresa con sedes en Madrid y Barcelona, aplica una **fragmentación horizontal** para distribuir los datos entre dos nodos según la localidad del empleado.

| ID | Nombre | Salario | Localidad | Departamento |
|----|--------|---------|-----------|--------------|
| 1 | María | 2.500 | Madrid | Dirección |
| 2 | Juan | 2.400 | Oviedo | I+D |
| 3 | Sofía | 2.700 | Barcelona | RR.HH. |
| 4 | Carlos | 2.300 | Madrid | I+D |
| 5 | Ana | 2.600 | Barcelona | Dirección |

<details>
<summary>Ver solución</summary>

**Nodo Madrid** (empleados de Madrid):

| ID | Nombre | Salario | Localidad | Departamento |
|----|--------|---------|-----------|--------------|
| 1 | María | 2.500 | Madrid | Dirección |
| 2 | Juan | 2.400 | Oviedo | I+D |
| 4 | Carlos | 2.300 | Madrid | I+D |

> Juan trabaja en Oviedo pero se asigna al nodo Madrid por proximidad geográfica.

**Nodo Barcelona** (empleados de Barcelona):

| ID | Nombre | Salario | Localidad | Departamento |
|----|--------|---------|-----------|--------------|
| 3 | Sofía | 2.700 | Barcelona | RR.HH. |
| 5 | Ana | 2.600 | Barcelona | Dirección |

**Verificación:** Los dos fragmentos juntos deben contener todos los registros de la tabla original — 5 registros en total. ✅

</details>

---

### Ejercicio 2 — Fragmentación vertical

Con la misma tabla de EMPLEADOS, aplica una **fragmentación vertical** separando los datos personales de los datos laborales. El ID debe aparecer en ambos fragmentos.

| ID | Nombre | Salario | Localidad | Departamento |
|----|--------|---------|-----------|--------------|
| 1 | María | 2.500 | Madrid | Dirección |
| 2 | Juan | 2.400 | Oviedo | I+D |
| 3 | Sofía | 2.700 | Barcelona | RR.HH. |
| 4 | Carlos | 2.300 | Madrid | I+D |
| 5 | Ana | 2.600 | Barcelona | Dirección |

<details>
<summary>Ver solución</summary>

**Fragmento 1 — Datos personales:**

| ID | Nombre | Localidad |
|----|--------|-----------|
| 1 | María | Madrid |
| 2 | Juan | Oviedo |
| 3 | Sofía | Barcelona |
| 4 | Carlos | Madrid |
| 5 | Ana | Barcelona |

**Fragmento 2 — Datos laborales:**

| ID | Salario | Departamento |
|----|---------|--------------|
| 1 | 2.500 | Dirección |
| 2 | 2.400 | I+D |
| 3 | 2.700 | RR.HH. |
| 4 | 2.300 | I+D |
| 5 | 2.600 | Dirección |

**Por qué el ID aparece en los dos fragmentos:**
El ID es imprescindible para poder reconstruir la tabla original. Si no estuviera, no sabríamos que el salario 2.500 corresponde a María.

```
Fragmento 1 + Fragmento 2  ──► (uniendo por ID) ──► Tabla original completa
```

</details>

---

### Ejercicio 3 — ¿Qué tipo de fragmentación es?

Identifica qué tipo de fragmentación se ha aplicado en cada caso:

**Caso A:**
```
Tabla original: PEDIDOS (ID, Producto, Cliente, Fecha, Importe)

Fragmento 1: ID | Producto | Cliente
Fragmento 2: ID | Fecha | Importe
```

**Caso B:**
```
Tabla original: PEDIDOS (ID, Producto, Cliente, Fecha, Importe)

Fragmento 1: pedidos del año 2024
Fragmento 2: pedidos del año 2025
```

<details>
<summary>Ver solución</summary>

**Caso A → Fragmentación vertical**
Se han dividido las **columnas** de la tabla entre dos fragmentos. El ID aparece en ambos para poder reconstruir la tabla original.

**Caso B → Fragmentación horizontal**
Se han dividido las **filas** de la tabla entre dos fragmentos según el año del pedido. Cada fragmento tiene todas las columnas pero distintos registros.

</details>

