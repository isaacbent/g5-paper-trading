# G5 Paper Trading

Registro público y con fecha de las señales de **G5**, una cartera sistemática solo largo y sin apalancamiento. El objetivo es construir un historial forward (TRUE_FORWARD) verificable: cada señal se publica **antes de la apertura** de la sesión en la que se ejecuta y su resultado se añade después del cierre.

## Composición

| Bloque | Peso | Descripción |
|---|---|---|
| Bloque mensual (asset allocation) | 50% | Hasta el 30-10-2026: **G4** (rotación sectorial por momentum + tendencia en QQQ + tendencia en 30 ETFs). Desde el 2-11-2026: **N14** (rotación sectorial con filtro de tendencia + asignación táctica multiactivo con filtro de tendencia). Rebalanceo mensual. |
| Bloque táctico | 50% | 4 señales diarias (E382, M3065, R152, Y12050) sobre QQQ, XLK y SMH. Entrada en la apertura y salida al cierre del mismo día. 0,5x por señal, tope de exposición 1,0x del bloque. Sin señal, el bloque está en efectivo (BIL). |

- Capital virtual inicial: **100.000 $**
- Inicio del registro: **octubre de 2026**
- Fricción supuesta: 0,05% por operación
- Datos: Massive (antes Polygon.io), barras diarias ajustadas y dividendos

Las reglas exactas de cada señal no se publican. Lo que se publica es la señal y su resultado, con la fecha del commit como prueba de que existía antes de la apertura.

## G3VT (registro independiente, desde noviembre de 2026)

Además de G5, se publica una segunda cartera, **G3VT**, con su propio capital virtual de 100.000 $. Es una rotación
multiactivo de ETFs, solo largo y sin apalancamiento, que se revisa cada semana y ajusta su exposición cada día según la
volatilidad (lo no invertido queda en BIL). Las reglas exactas no se publican.

- `g3vt/weights/AAAA-MM-DD.json`: pesos para la apertura de esa sesión, publicados tras el cierre del último día hábil
  de la semana anterior. Primera publicación: cierre del 30-10-2026, para la apertura del 2-11-2026.
- `g3vt/exposure.csv`: exposición decidida en cada cierre; se aplica desde el cierre de la sesión siguiente.
- `g3vt/ledger.csv`: resultado por semana, capital y caída máxima diaria acumulada.

## Estructura

- `signals/AAAA-MM-DD.json`: señales para la sesión de esa fecha, publicadas la tarde anterior (hora de Nueva York).
- `state/monthly/AAAA-MM.json`: pesos del bloque mensual de ese mes, publicados tras el cierre del último día hábil del mes anterior.
- `state/monthly_block.json`: qué estrategia ocupa el bloque mensual en cada periodo.
- `proofs/`: huellas SHA-256 de todos los archivos de datos en cada actualización, selladas con [OpenTimestamps](https://opentimestamps.org) (anclaje en Bitcoin) por GitHub Actions. Un `.ots` permite a cualquiera verificar que esos archivos existían en esa fecha: `ots verify proofs/<archivo>.sha256.ots`.
- `results/ledger.csv`: resultado diario de cada bloque y valor de la cartera. La columna `g4_ret` recoge el resultado del bloque mensual, sea G4 o N14.

## Cambios

- **2026-10-08** — Corregidos los pesos de octubre de G4: la pieza de tendencia multiactivo se había calculado con 5 ETFs en lugar de su universo de 30. No se había publicado ninguna señal ni resultado antes de la corrección.
- **2026-10-08** — Añadido el registro independiente G3VT (empieza el 2-11-2026).
- **2026-10-08** — Anunciado con antelación: desde noviembre de 2026 el bloque mensual pasa de G4 a N14. Motivo: en una prueba de estrés con 2008-2011, G4 tuvo una caída diaria máxima del −24,9 %; N14, del −12,7 %.

## Aviso

Esto es un registro de **paper trading** (simulado). No es asesoramiento de inversión ni una oferta de servicios. Los resultados simulados no garantizan resultados reales.
