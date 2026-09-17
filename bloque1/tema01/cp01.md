# Casos Prácticos — Tema 1
# Sistemas de Representación de la Información. Ficheros

---

## Caso Práctico 1 — Gestión de tienda y almacén

Un amigo ha abierto una tienda de productos de informática y necesita ayuda para controlar el stock de su almacén. Como solución rápida, construimos un **fichero maestro** con el stock actual y dos **ficheros de movimientos** — uno con las ventas del día y otro con las entradas de material.

Al final del día se han producido los siguientes movimientos:

**Stock inicial (fichero maestro):**

| ID | DESCRIPCIÓN | Uds. |
|----|-------------|------|
| 1 | Cable CAT6A | 5.000 |
| 2 | Conector RJ45 CAT6A | 100 |
| 3 | Panel de 24p RJ45 | 10 |

**Entradas al almacén:**

| ID | DESCRIPCIÓN | Uds. |
|----|-------------|------|
| 4 | Fibra óptica | 1.000 |

**Ventas del día:**

| ID | DESCRIPCIÓN | Uds. vendidas |
|----|-------------|--------------|
| 1 | Cable CAT6A | 1.000 |
| 2 | Conector RJ45 CAT6A | 30 |
| 3 | Panel de 24p RJ45 | 2 |

**¿Cuál es el stock resultante al final del día?**

<details>
<summary>Ver solución</summary>

**Cálculo:**

| ID | DESCRIPCIÓN | Stock inicial | Entradas | Ventas | Stock final |
|----|-------------|--------------|----------|--------|-------------|
| 1 | Cable CAT6A | 5.000 | 0 | 1.000 | **4.000** |
| 2 | Conector RJ45 CAT6A | 100 | 0 | 30 | **70** |
| 3 | Panel de 24p RJ45 | 10 | 0 | 2 | **8** |
| 4 | Fibra óptica | 0 | 1.000 | 0 | **1.000** |

**Fichero maestro actualizado:**

| ID | DESCRIPCIÓN | Uds. |
|----|-------------|------|
| 1 | Cable CAT6A | 4.000 |
| 2 | Conector RJ45 CAT6A | 70 |
| 3 | Panel de 24p RJ45 | 8 |
| 4 | Fibra óptica | 1.000 |

Una vez actualizado el fichero maestro, los ficheros de movimientos pueden eliminarse — ya no son necesarios.

</details>

---

## Caso Práctico 2 — Elección del sistema de almacenamiento

El mismo amigo usa un único disco duro externo para guardar los ficheros de su tienda — el mismo en el que guarda películas. El negocio es pequeño, no tiene intención de crecer a corto plazo, y le preocupan principalmente la **seguridad** y la **disponibilidad** de la información.

**¿Qué sistema de almacenamiento le recomendarías y por qué?**

<details>
<summary>Ver solución</summary>

Analizamos las opciones disponibles:

| Opción | ¿Adecuada? | Por qué |
|--------|-----------|---------|
| Disco duro en PC | Solo con redundancia | Riesgo de fallo, poco profesional |
| SAN | No | Cara y compleja para un negocio pequeño |
| **NAS con RAID 1** | **Sí** | Económico, seguro, fácil de gestionar |

La mejor opción es un sistema **NAS con RAID 1** (espejo):

- **RAID 1** → copia exacta en dos discos. Si falla uno, el otro tiene todos los datos.
- **NAS** → almacenamiento en red, accesible desde cualquier equipo de la tienda.
- Precio razonable y fácil de gestionar sin conocimientos técnicos avanzados.

> 🔗 [Calculadora RAID de Synology](https://www.synology.com/es-es/support/RAID_calculator)

</details>

