# Tema 2 — Fundamentos de las Bases de Datos

---

## Índice

1. [Introducción](#1-introducción)
2. [Definición y elementos](#2-definición-y-elementos)
3. [Tipos de bases de datos según el modelo](#3-tipos-de-bases-de-datos-según-el-modelo)
4. [Bases de datos centralizadas y distribuidas](#4-bases-de-datos-centralizadas-y-distribuidas)
5. [Fragmentación de la información](#5-fragmentación-de-la-información)
6. [Distribución de la información](#6-distribución-de-la-información)
7. [Seguridad y recuperación](#7-seguridad-y-recuperación)

---

## 1. Introducción

Antes de las bases de datos, cada aplicación gestionaba sus propios ficheros independientes. Esto generaba problemas graves:

```
App. de ventas     →  fichero_ventas.dat
App. de almacén    →  fichero_stock.dat      ← datos duplicados
App. de clientes   →  fichero_clientes.dat   ← inconsistencias
```

> 💡 Las bases de datos nacen para resolver estos problemas: **centralizar la información, eliminar duplicidades y garantizar la consistencia de los datos.**

En los años 60 aparecen las primeras bases de datos con discos magnéticos. En los 80, con la llegada de SQL, se popularizan las bases de datos relacionales que usamos hoy.

---

## 2. Definición y elementos

### ¿Qué es una base de datos?

Una **base de datos** es una herramienta que permite **estructurar el almacenamiento de datos, relacionarlos y ayudar a su gestión, consulta y manipulación, garantizando siempre su integridad**.

El **modelo de base de datos** describe cómo se almacena la información y cómo se interrelaciona — define la arquitectura de la base de datos.

### Elementos que componen una base de datos

```
BASE DE DATOS
│
├── Entidades   → objetos sobre los que guardamos información
│                 Ej: clientes, productos, pedidos
│
├── Tablas      → organizan la información de cada entidad
│                 Ej: tabla CLIENTES con todos sus datos
│
├── Campos      → cada tipo de dato (columnas)
│                 Ej: nombre, DNI, dirección, teléfono
│
└── Registros   → cada fila de la tabla
                  Ej: los datos de un cliente concreto
```

Ejemplo de tabla CLIENTES:

| ID | DNI | NOMBRE | APELLIDOS | LOCALIDAD |
|----|-----|--------|-----------|-----------|
| 1 | 54621345D | Esther | Rodríguez Méndez | Pontevedra |
| 2 | 85542549E | Javier | Ramírez Lorenzo | Zamora |
| 3 | 24581392F | Silvia | Castro Gutiérrez | Ourense |

### Tipos de bases de datos según su uso

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **Individual** | Un solo usuario asume todos los roles | Base de datos personal |
| **Compartida** | Múltiples usuarios acceden a la misma información | Base de datos de una pyme |
| **Bancos de datos** | Grandes volúmenes para consulta | BASE (buscador científico), INE |
| **Pública** | Accesible a cualquier usuario | Base de datos del catastro |

---

## 3. Tipos de bases de datos según el modelo

### Modelos estandarizados

Son los más usados hoy en día. Utilizan estándares como SQL que garantizan interoperabilidad.

#### Relacional
Almacena datos en **tablas relacionadas entre sí**. Es el modelo más usado actualmente. Usa SQL.

```
TABLA VEHÍCULOS              TABLA PROPIETARIOS
───────────────              ──────────────────
Matrícula │ Marca │ ID_prop  ID │ Nombre
7891GHR   │ Seat  │    1      2 │ María
1897DTR   │ Ford  │    2      1 │ Daniel
6375JKR   │ Merc. │    3      3 │ Esther
```

✅ Simple, flexible, ampliamente soportado | Es el que usarás en esta asignatura

#### Orientada a objetos
Almacena información como **objetos** definidos por clases, con atributos y métodos. Surgió en los años 90. Permite almacenar multimedia (vídeos, imágenes, audios).

#### Objeto-relacional
**Combina** lo mejor de las relacionales y las orientadas a objetos. La usan Oracle, Microsoft e IBM. Es hacia donde evolucionan las relacionales.

---

### Modelos especializados

Más rígidos y estructurados. Hoy en día están en desuso o tienen usos muy específicos.

#### Jerárquica
La información se organiza en **árbol** con relación **padre-hijo**. Cada nodo tiene un único padre pero puede tener varios hijos.

```
              CEO
             /   \
       VP Ventas   VP Marketing
       /     \           \
  Gerente N  Gerente S   Gerente Mkt
```

❌ Obsoleta. Demasiado rígida.

#### En red
Evolución de la jerárquica. Las relaciones pueden ser **circulares** — un nodo puede tener varios padres.

❌ En desuso.

---

### Comparativa de modelos

| Modelo | Estado | Uso actual |
|--------|--------|------------|
| Jerárquica | Obsoleta | Prácticamente ninguno |
| En red | En desuso | Muy residual |
| **Relacional** | **Vigente** | **El más usado. Lo verás en esta asignatura** |
| Orientada a objetos | Vigente | Aplicaciones complejas |
| Objeto-relacional | Vigente | Grandes empresas (Oracle) |
| NoSQL | Vigente | Big Data, aplicaciones web a gran escala |

---

## 4. Bases de datos centralizadas y distribuidas

### Centralizada
Toda la información está en **un único servidor**.

```
PC1 ──┐
PC2 ──┼──► [SERVIDOR CENTRAL con la BD]
PC3 ──┘
```

✅ Más simple de gestionar | ✅ Sin problemas de sincronización
❌ Si falla el servidor, todo cae

### Distribuida
La información está **repartida entre varios nodos** conectados en red. Aparecieron en los años 70 para gestionar datos a gran escala.

```
PC1 ──► [Nodo Madrid] ◄──► [Nodo Barcelona] ◄──► [Nodo Valencia]
             BD_1                 BD_2                  BD_3
              └──────────────────┴──────────────────────┘
                         misma base de datos distribuida
```

**Ventajas:**
- Mayor disponibilidad — si falla un nodo, los demás siguen funcionando
- Mejor rendimiento — cada usuario accede al nodo más cercano
- Escalable — se añaden nodos fácilmente

**Inconvenientes:**
- Más compleja de gestionar
- Problemas de sincronización entre nodos
- Más difícil garantizar la consistencia

> 💡 Ejemplos reales de BD distribuidas: Amazon, Google, Facebook. Sus datos están en miles de servidores repartidos por todo el mundo.

### Diseño de una BD distribuida

Al diseñarla hay que decidir tres cosas:

```
[ Fragmentar ] ──► ¿Cómo dividimos los datos entre los nodos?
[ Replicar ]   ──► ¿Qué datos copiamos en varios nodos?
[ Distribuir ] ──► ¿Dónde colocamos cada fragmento?
```

---

## 5. Fragmentación de la información

En una BD distribuida, los datos se dividen en **fragmentos** que se reparten entre los nodos.

### Fragmentación horizontal
Se dividen las **filas** de una tabla entre los nodos.

```
TABLA EMPLEADOS completa:
─────────────────────────────────────────
ID │ Nombre │ Localidad   │ Departamento
1  │ María  │ Madrid      │ Dirección
2  │ Juan   │ Oviedo      │ I+D
3  │ Sofía  │ Barcelona   │ RR.HH.

           ↓ Fragmentación horizontal

Nodo Madrid:                    Nodo Barcelona:
────────────────────────────    ────────────────────────────
ID │ Nombre │ Localidad │ Dept  ID │ Nombre │ Localidad │ Dept
1  │ María  │ Madrid    │ Dir.  3  │ Sofía  │ Barcelona │ RR.HH.
2  │ Juan   │ Oviedo    │ I+D
```

### Fragmentación vertical
Se dividen las **columnas** de una tabla entre los nodos. Cada nodo tiene todos los registros pero solo algunos campos. Se repite el ID en todos los fragmentos para poder reconstruir la tabla.

```
TABLA EMPLEADOS completa:
─────────────────────────────────────────
ID │ Nombre │ Salario │ Localidad │ Dept
1  │ María  │ 2.500   │ Madrid    │ Dir.
2  │ Juan   │ 2.400   │ Oviedo    │ I+D
3  │ Sofía  │ 2.700   │ Barcelona │ RR.HH.

           ↓ Fragmentación vertical

Fragmento 1 (datos personales):   Fragmento 2 (datos laborales):
────────────────────────────      ──────────────────────
ID │ Nombre │ Salario             ID │ Localidad │ Dept
1  │ María  │ 2.500               1  │ Madrid    │ Dir.
2  │ Juan   │ 2.400               2  │ Oviedo    │ I+D
3  │ Sofía  │ 2.700               3  │ Barcelona │ RR.HH.
```

> ⚠️ El ID siempre se repite en todos los fragmentos verticales — es imprescindible para poder reconstruir la tabla original.


### Fragmentación mixta
Combina horizontal y vertical.

---

## 6. Distribución de la información

Los nodos de una BD distribuida pueden configurarse de tres formas:

| Configuración | Descripción | Ventaja | Inconveniente |
|---------------|-------------|---------|---------------|
| **Réplica** | Los datos se copian en cada nodo | Máxima disponibilidad | Mucho espacio, sincronización compleja |
| **Particionado** | Los datos se reparten sin duplicar | Ahorra espacio | Si falla un nodo, se pierden esos datos |
| **Híbrido** | Combina los dos anteriores | Equilibrio | Más complejo de diseñar |

> 💡 El **híbrido** es el más usado en la práctica.

### Objetivos de la distribución de datos

Al diseñar cómo distribuir los datos siempre hay que tener en cuenta estos objetivos:

| Objetivo | Descripción |
|----------|-------------|
| **Localidad** | Acercar los datos a las aplicaciones que los van a usar para reducir tiempos de respuesta |
| **Balanceo de carga** | Repartir el trabajo entre nodos de forma proporcional para que ninguno se sature |
| **Escalabilidad** | Poder añadir más nodos fácilmente cuando el sistema crece |
| **Tolerancia a fallos** | Si un nodo falla, los demás siguen funcionando con los datos disponibles |

---

## 7. Seguridad y recuperación

En una BD distribuida la seguridad es crítica porque hay más puntos de fallo.

### Control de concurrencia

Cuando varios usuarios modifican los mismos datos a la vez pueden surgir conflictos. Se resuelven con:

- **Commit en dos fases:** el nodo coordinador pregunta a todos los nodos si están listos antes de confirmar la operación. Si alguno falla → rollback.
- **Interbloqueo distribuido:** se reservan todos los recursos necesarios antes de iniciar la transacción para evitar bloqueos mutuos.

### Medidas de seguridad

| Medida | Qué hace |
|--------|---------|
| Cifrado de datos | Los datos son ilegibles sin las claves de acceso |
| Firewall / VPN | Protege la comunicación entre nodos |
| Auditorías | Registra todos los accesos y operaciones realizadas |

### Estrategias de recuperación

- **Replicación geográfica** → copias en diferentes ubicaciones físicas
- **Copias de seguridad incrementales** → solo guarda los cambios desde el último backup
- **Conmutación por error** → si un nodo falla, otro asume sus funciones automáticamente

---

## Resumen del tema

```
BASES DE DATOS
│
├── Elementos: Entidades / Tablas / Campos / Registros
│
├── Modelos
│    ├── Estandarizados: Relacional ✅ / Obj. objetos / Obj-relacional
│    └── Especializados: Jerárquica ❌ / En red ❌ / NoSQL
│
├── Centralizada  →  un servidor
└── Distribuida   →  varios nodos
     ├── Fragmentación: horizontal / vertical / mixta
     ├── Distribución: réplica / particionado / híbrido
     ├── Objetivos: localidad / balanceo / escalabilidad / tolerancia
     └── Seguridad: cifrado / auditorías / backups
```
