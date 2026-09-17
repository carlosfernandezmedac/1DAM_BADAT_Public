# Tema 1 — Sistemas de Representación de la Información. Ficheros

---

## Índice

1. [Introducción](#1-introducción)
2. [Representación de la información](#2-representación-de-la-información)
3. [Concepto de fichero](#3-concepto-de-fichero)
4. [Tipos de ficheros según su organización](#4-tipos-de-ficheros-según-su-organización)
5. [Tipos de ficheros según su contenido](#5-tipos-de-ficheros-según-su-contenido)
6. [Tipos de ficheros según su uso](#6-tipos-de-ficheros-según-su-uso)
7. [Sistemas de almacenamiento](#7-sistemas-de-almacenamiento)
8. [Ficheros vs. Bases de datos](#8-ficheros-vs-bases-de-datos)

---

## 1. Introducción

Cada vez que reservas una cita médica, usas redes sociales o haces una llamada con el móvil, estás interactuando con sistemas de almacenamiento y gestión de información.

> 💡 **La información es el recurso más valioso de cualquier organización.** Saber almacenarla, organizarla y recuperarla eficientemente marca la diferencia entre un sistema que funciona y uno que no.

En este tema veremos cómo los ordenadores representan y almacenan la información, desde el nivel más básico (bits) hasta los sistemas modernos, y por qué las bases de datos superan a los ficheros tradicionales.

---

## 2. Representación de la información

Todo lo que almacena un ordenador se reduce a **bits** — la unidad mínima de información. Un bit solo puede valer `0` o `1`.

Para representar información más compleja (letras, imágenes, sonido) se usan **patrones de bits**: grupos de bits consecutivos que los dispositivos interpretan y decodifican en formas entendibles para nosotros.


### Múltiplos del bit

| Unidad | Símbolo | Equivalencia |
|--------|---------|--------------|
| Bit | b | 0 ó 1 |
| Byte | B | 8 bits |
| Kilobyte | KB | 1.024 B |
| Megabyte | MB | 1.024 KB |
| Gigabyte | GB | 1.024 MB |
| Terabyte | TB | 1.024 GB |
| Petabyte | PB | 1.024 TB |
| Exabyte | EB | 1.024 PB |

> ⚠️ **Importante:** Los archivos binarios (.jpg, .mp4, .exe) requieren software específico para ser interpretados — el ordenador no puede "leerlos" directamente como texto plano.

---

## 3. Concepto de fichero

Un **fichero** (o archivo) es la forma más simple de almacenar información en un sistema informático. Es un **conjunto ordenado de bits que contiene información relacionada con un tema específico**.

Para ser identificado necesita:
- Un **nombre** — para reconocerlo
- Una **extensión** — indica el tipo de contenido (`.txt`, `.jpg`, `.sql`, `.exe`...)

### Estructura interna

Internamente, un fichero se organiza en **registros**, y cada registro tiene **campos** (atributos):

```
FICHERO: instrumentos.txt
┌────────────────────────────────┐
│  ┌────┬─────────────┬────────┐ │
│  │ N° │ INSTRUMENTO │  TIPO  │ │◄── Campos (atributos)
│  ├────┼─────────────┼────────┤ │
│  │  1 │ Guitarra    │ Cuerda │ │◄── Registro
│  │  2 │ Gaita       │ Viento │ │◄── Registro
│  │  3 │ Tambor      │ Percusión│ │◄── Registro
│  └────┴─────────────┴────────┘ │
└────────────────────────────────┘
```

---

## 4. Tipos de ficheros según su organización

Hay **3 formas** de organizar la información dentro de un fichero:

### 4.1 Secuencial

Los registros se almacenan **uno detrás de otro**. Para llegar al registro 100, hay que leer los 99 anteriores.

```
[Reg.1] ──► [Reg.2] ──► [Reg.3] ──► [Reg.4] ──► [FIN]
```

✅ Simple de implementar  
❌ Lento para acceder a registros concretos

**Ejemplo real:** Un fichero de matrículas de vehículos guardado en texto plano:
```
0305DOR#AUDI A8#Madrid#Sofía#Sánchez#7609ERC#KIA EV6#Pontevedra#...
```

---

### 4.2 Acceso directo

Cada registro ocupa un **tamaño fijo** predefinido. Esto permite saltar directamente a cualquier registro sin leer los anteriores.

```
Posición 0: [0305DOR | AUDI A8   | Madrid    | Sofía  ]  ← 50 bytes
Posición 1: [7609ERC | KIA EV6   | Pontevedra| Darío  ]  ← 50 bytes
Posición 2: [2909CTQ | SEAT LEON | Mallorca  | Esteban]  ← 50 bytes
                                    ↑
                          Acceso directo a cualquier posición
```

✅ Acceso rápido a cualquier registro  
❌ Desperdicia espacio si el registro no llena el espacio reservado

---

### 4.3 Indexado

Usa un **índice separado** (como el índice de un libro) que apunta a dónde está cada registro.

```
ÍNDICE                    DATOS
──────                    ─────
"0305DOR" ──► posición 0 ──► [AUDI A8, Madrid, Sofía...]
"7609ERC" ──► posición 2 ──► [KIA EV6, Pontevedra, Darío...]
"2909CTQ" ──► posición 1 ──► [SEAT LEON, Mallorca, Esteban...]
```

✅ Acceso rápido y flexible  
❌ Mayor complejidad de implementación

---

### Comparativa rápida

| Tipo | Velocidad acceso | Espacio | Complejidad |
|------|-----------------|---------|-------------|
| Secuencial | Lenta | Eficiente | Baja |
| Acceso directo | Rápida | Puede desperdiciar | Media |
| Indexado | Muy rápida | Necesita índice extra | Alta |

---

## 5. Tipos de ficheros según su contenido

### 5.1 Ficheros de texto (planos)

Legibles directamente por personas. Usan codificación **ASCII/UTF-8**.

```
archivo.html  ──► puedes abrirlo con el Bloc de notas y leerlo
script.sql    ──► puedes ver las consultas directamente
config.ini    ──► puedes editar la configuración
```

**Extensiones habituales:** `.html`, `.php`, `.xml`, `.sql`, `.java`, `.js`, `.cfg`, `.ini`

### 5.2 Ficheros binarios

Necesitan una aplicación específica para interpretarlos. Las bases de datos los usan internamente.

```
foto.jpg   ──► necesitas un visor de imágenes
datos.docx ──► necesitas Word o LibreOffice
app.exe    ──► necesitas el sistema operativo para ejecutarlo
bbdd.myd   ──► necesitas MySQL para leerlo (fichero interno de MySQL)
```

**Extensiones habituales:** `.docx`, `.pdf`, `.exe`, `.jar`, `.jpg`, `.mp4`

---

## 6. Tipos de ficheros según su uso

```
FICHEROS
│
├── 📌 PERMANENTES ──────────────────────────────────────────────
│    │
│    ├── Maestros      Estado ACTUAL del sistema
│    │                 Ej: stock actual del almacén, lista de usuarios
│    │
│    ├── Históricos    Información del PASADO
│    │                 Ej: ventas del año anterior, contabilidad histórica
│    │
│    └── Constantes    Datos que CASI NO CAMBIAN
│                      Ej: códigos postales, países, tipos de IVA
│
└── ⏱️ TEMPORALES ───────────────────────────────────────────────
     │
     ├── Movimientos   Transacciones RECIENTES O EN CURSO
     │                 Ej: ventas del día de hoy
     │                 → Una vez procesados, se pueden eliminar
     │
     └── Maniobra      Datos DURANTE el procesamiento
                       Ej: fichero intermedio al calcular nóminas
                       → Se eliminan al terminar la operación
```

> 💡 **Ejemplo del día a día:**
> Al final del día en una tienda, el **fichero de movimientos** (ventas del día) se combina con el **fichero maestro** (stock) para actualizar el inventario. Una vez actualizado el stock, el fichero de movimientos ya no hace falta → se elimina.

---

## 7. Sistemas de almacenamiento

### 7.1 Discos duros: HDD vs SSD

| | HDD (magnético) | SSD (estado sólido) |
|--|----------------|---------------------|
| Velocidad | Más lento | Más rápido |
| Precio | Más barato | Más caro |
| Durabilidad | Menor (partes mecánicas) | Mayor |
| Uso típico | Almacenamiento masivo | Sistema operativo, apps |

> 💡 **Sabías que...** El primer disco duro comercial fue el IBM 305 RAMAC (1956). Pesaba **una tonelada**, ocupaba el espacio de dos refrigeradores y tenía una capacidad de **5 megabytes**. El coste por megabyte era de unos 10.000 dólares.

---

### 7.2 NAS y SAN

Sistemas de almacenamiento en **red**:

```
NAS (Network Attached Storage)        SAN (Storage Area Network)
──────────────────────────────        ──────────────────────────
PC1 ──┐                               Servidor 1 ──┐
PC2 ──┼──► [NAS] ◄── Red local        Servidor 2 ──┼──► [SAN] ◄── Red dedicada
PC3 ──┘                               Servidor 3 ──┘

Como un disco duro en red local       Red completa de almacenamiento
Fácil de gestionar, económico         Potente, caro, complejo
Ideal para PYMES                      Ideal para grandes empresas
```

---

### 7.3 RAID — Redundancia de discos

**RAID** (Redundant Array of Independent Disks) combina varios discos para mejorar **velocidad** y/o **seguridad**.

| Nivel | Cómo funciona | Para qué sirve |
|-------|--------------|----------------|
| **RAID 0** | Datos repartidos entre discos | Más velocidad, sin seguridad |
| **RAID 1** | Copia exacta en otro disco (espejo) | Máxima seguridad, mitad de capacidad |
| **RAID 5** | Datos + paridad distribuida | Equilibrio velocidad/seguridad (mínimo 3 discos) |
| **RAID 6** | Como RAID 5 pero con doble paridad | Tolera el fallo de 2 discos |

> 🔗 Prueba la [Calculadora RAID de Synology](https://www.synology.com/es-es/support/RAID_calculator)

---

### 7.4 Los 4 pilares de la gestión de sistemas de almacenamiento

```
              CAPACIDAD
              ¿Cuánto puedo guardar?
                    ▲
                    │
  RECUPERABILIDAD ◄─┼─► RENDIMIENTO
  ¿Puedo restaurar  │   ¿Qué velocidad
  si algo falla?    │   de acceso tengo?
                    │
                    ▼
              FIABILIDAD
              ¿Es estable y seguro?
```

---

## 8. Ficheros vs. Bases de datos

¿Por qué no usamos solo ficheros? Aquí está la respuesta:

| Nivel | Ficheros | Bases de datos |
|---------|----------|----------------|
| **Organización** | Carpetas sin estructura interna relacional | Tablas con relaciones entre ellas |
| **Acceso** | Directo por el SO, búsqueda limitada | A través del SGBD con SQL, muy eficiente |
| **Manipulación** | Básica, manual, requiere programación | Operaciones avanzadas con SQL |
| **Seguridad** | La del sistema operativo | Roles, permisos, encriptación |
| **Escalabilidad** | Ineficiente con muchos datos | Diseñadas para escalar |
| **Redundancia** | Datos repetidos en varios ficheros | Datos centralizados sin repetición |

**Ventajas de los ficheros:** coste bajo, compatibilidad con el SO, implementación rápida.

**Desventajas de los ficheros:** organización limitada, acceso ineficiente, escalabilidad nula con grandes volúmenes.

> 💡 **Conclusión:** Los ficheros fueron el punto de partida histórico. Las bases de datos nacen para superar sus limitaciones cuando el volumen y la complejidad de los datos crecen. A lo largo de esta asignatura aprenderás a diseñarlas, crearlas y gestionarlas.

---

## Resumen visual del tema

```
TEMA 1

  Bit ──► Byte ──► KB ──► MB ──► GB ──► TB ──► PB
                    │
                    ▼
              FICHEROS
         ┌────────────────────────────┐
         │ Por organización:          │
         │  • Secuencial              │
         │  • Acceso directo          │
         │  • Indexado                │
         │                            │
         │ Por contenido:             │
         │  • Texto plano             │
         │  • Binario                 │
         │                            │
         │ Por uso:                   │
         │  • Permanentes             │
         │    - Maestros              │
         │    - Históricos            │
         │    - Constantes            │
         │  • Temporales              │
         │    - Movimientos           │
         │    - Maniobra              │
         └────────────────────────────┘
                    │
                    ▼
           ALMACENAMIENTO
         ┌────────────────────────────┐
         │  HDD / SSD                 │
         │  NAS / SAN                 │
         │  RAID (0, 1, 5, 6)         │
         │  Gestión: capacidad,       │
         │  fiabilidad, rendimiento,  │
         │  recuperabilidad           │
         └────────────────────────────┘
                    │
                    ▼
        FICHEROS vs. BASES DE DATOS
     Las BBDD superan a los ficheros en
     organización, acceso, seguridad
     y escalabilidad
```
