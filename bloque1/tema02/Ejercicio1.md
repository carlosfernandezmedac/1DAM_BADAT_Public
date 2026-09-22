# Ejercicio 1: Estructuración y Fragmentación de una Base de Datos Relacional

## Contexto

Una periodista deportiva ha ido apuntando datos sueltos sobre varios clubes de fútbol para un reportaje: por un lado sus notas sobre jugadores, por otro sus notas sobre los clubes, por otro sobre entrenadores, y por otro sobre presidentes. **Cada bloque de notas está por separado**, y solo están conectados por el nombre del club escrito a mano — no hay ningún identificador (ID) en ninguna parte.

Tu trabajo es **diseñar las tablas de una base de datos relacional** a partir de estos 4 bloques de notas, inventando tú mismo/a los identificadores (`id_club`, `id_jugador`...) y usándolos para enlazar los datos correctamente — y después **fragmentar** dos de esas tablas.

No se te entrega ningún archivo de partida: **crea tú un Excel desde cero** con las pestañas que se piden más abajo.

---

## Datos de partida

### Bloque 1 — Notas sobre jugadores

| Jugador | Posición | Dorsal | Club |
|---|---|---|---|
| Kylian Mbappé | Delantero | 9 | Real Madrid |
| Vinícius Júnior | Delantero | 7 | Real Madrid |
| Robert Lewandowski | Delantero | 9 | FC Barcelona |
| Pedri | Centrocampista | 8 | FC Barcelona |
| Antoine Griezmann | Delantero | 7 | Atlético de Madrid |
| Jan Oblak | Portero | 13 | Atlético de Madrid |
| Nico Williams | Delantero | 10 | Athletic Club |
| Unai Simón | Portero | 1 | Athletic Club |
| José Luis Gayà | Defensa | 14 | Valencia CF |
| Hugo Duro | Delantero | 9 | Valencia CF |
| Ousmane Dembélé | Delantero | 10 | Paris Saint-Germain |
| Achraf Hakimi | Defensa | 2 | Paris Saint-Germain |

### Bloque 2 — Notas sobre clubes

| Club | Ciudad | Provincia | Año de fundación |
|---|---|---|---|
| Real Madrid | Madrid | Madrid | 1902 |
| FC Barcelona | Barcelona | Barcelona | 1899 |
| Atlético de Madrid | Madrid | Madrid | 1903 |
| Athletic Club | Bilbao | Vizcaya | 1898 |
| Valencia CF | Valencia | Valencia | 1919 |
| Paris Saint-Germain | París | Francia | 1970 |

### Bloque 3 — Notas sobre entrenadores

| Entrenador | Club |
|---|---|
| Carlo Ancelotti | Real Madrid |
| Hansi Flick | FC Barcelona |
| Diego Simeone | Atlético de Madrid |
| Ernesto Valverde | Athletic Club |
| Rubén Baraja | Valencia CF |
| Luis Enrique | Paris Saint-Germain |

### Bloque 4 — Notas sobre presidentes

| Presidente | Club |
|---|---|
| Florentino Pérez | Real Madrid |
| Joan Laporta | FC Barcelona |
| Enrique Cerezo | Atlético de Madrid |
| Jon Uriarte | Athletic Club |
| Layhoon Chan | Valencia CF |
| Nasser Al-Khelaifi | Paris Saint-Germain |

---

## Parte 1 — Diseña las tablas (entidades)

A partir de los 4 bloques anteriores, identifica y construye 5 tablas: **CIUDADES, CLUBES, PRESIDENTES, JUGADORES Y ENTTRENADORES**


**Tarea:** tú decides qué número le das a cada `id_ciudad`, `id_club`, etc. — no vienen dados en ningún sitio. Lo importante es que, una vez decidido, **lo uses de forma coherente** en todas las tablas donde haga falta esa clave foránea.


> 💡 Pista: para enlazar bien, primero decide el `id_club` de cada club (tabla CLUBES), y solo después ve a los bloques de jugadores/entrenadores/presidentes y busca "a qué id_club corresponde este nombre de club".

---

## Parte 2 — Fragmentación vertical

Elige la tabla **JUGADORES** y divídela en dos fragmentos, separando columnas:

- **JUGADORES_PRINCIPAL** → lo esencial: quién es el jugador y en qué club juega
- **JUGADORES_SECUNDARIA** → información adicional sobre su papel en el equipo

**Regla obligatoria:** la clave primaria (`id_jugador`) debe aparecer en **ambos** fragmentos, para poder reconstruir la tabla completa cuando haga falta.

**Justifica en una frase** por qué tendría sentido esta separación en un caso real (piensa en qué consulta se hace más a menudo: ¿"a qué club pertenece este jugador" o "en qué posición juega y con qué dorsal"?).

---

## Parte 3 — Fragmentación horizontal

Elige la tabla **CLUBES** y divídela en dos fragmentos, separando filas por un criterio con sentido: **si el club juega en España o en el extranjero**.

- **CLUBES_ESPAÑA** → clubes cuya ciudad está en España
- **CLUBES_EXTRANJERO** → clubes cuya ciudad está fuera de España

**Justifica en una frase** por qué tendría sentido esta separación en un caso real (piensa en qué normativa, moneda o idioma podría ser distinto entre un bloque y otro).

---

## Entregable

Un único archivo Excel, **creado por ti desde cero**, con las siguientes pestañas:

```
1. Tablas                     (las 5 tablas normalizadas, con IDs inventados por ti)
2. Fragmentacion_Vertical      (JUGADORES_PRINCIPAL + JUGADORES_SECUNDARIA)
3. Fragmentacion_Horizontal    (CLUBES_ESPAÑA + CLUBES_EXTRANJERO)
```

