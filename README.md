# G5 Paper Trading

Registro público y con fecha de las señales de **G5**, una cartera sistemática solo largo y sin apalancamiento. El objetivo es construir un historial forward (TRUE_FORWARD) verificable: cada señal se publica **antes de la apertura** de la sesión en la que se ejecuta y su resultado se añade después del cierre.

## Composición

| Bloque | Peso | Descripción |
|---|---|---|
| Bloque mensual (asset allocation) | 50% | Hasta el 30-10-2026: **G4** (rotación sectorial por momentum + tendencia en QQQ + tendencia en 30 ETFs). Desde el 2-11-2026: **P10** (tres reglas publicadas de asignación táctica multiactivo con alarmas de riesgo y filtros de tendencia, un tercio cada una). Rebalanceo mensual. |
| Bloque táctico | 50% | 4 señales diarias (E382, M3065, R152, Y12050) sobre QQQ, XLK y SMH. Entrada en la apertura y salida al cierre del mismo día. 0,5x por señal, tope de exposición 1,0x del bloque. Sin señal, el bloque está en efectivo (BIL). |

- Capital virtual inicial: **100.000 $**
- Inicio del registro: **octubre de 2026**
- Fricción supuesta: 0,05% por operación
- Datos: Massive (antes Polygon.io), barras diarias ajustadas y dividendos

Las reglas exactas de cada señal no se publican. Lo que se publica es la señal y su resultado, con la fecha del commit como prueba de que existía antes de la apertura.

## G3VT (registro independiente, desde el 12 de octubre de 2026)

Además de G5, se publica una segunda cartera, **G3VT**, con su propio capital virtual de 100.000 $. Es una rotación
multiactivo de ETFs, solo largo y sin apalancamiento, que se revisa cada semana y ajusta su exposición cada día según la
volatilidad (lo no invertido queda en BIL). Las reglas exactas no se publican.

- `g3vt/weights/AAAA-MM-DD.json`: pesos para la apertura de esa sesión, publicados tras el cierre del último día hábil
  de la semana anterior. Primera publicación: cierre del 9-10-2026, para la apertura del 12-10-2026.
- `g3vt/exposure.csv`: exposición decidida en cada cierre; se aplica desde el cierre de la sesión siguiente.
- `g3vt/ledger.csv`: resultado por semana, capital y caída máxima diaria acumulada.

## C2 (seguimiento en paper, desde el 12 de octubre de 2026)

Tercera cartera virtual (100.000 $), **solo de seguimiento**: no sustituye a G5 ni a G3VT. Es una rotación multiactivo
de ETFs, solo largo y sin apalancamiento, repartida en 5 subcarteras del 20 % que se revisan cada una un día distinto
de la semana, con control diario de volatilidad (lo no invertido queda en BIL). En el histórico cumplía el objetivo
(8 % anual con caída máxima de −10 %), pero no superó dos de cuatro pruebas de robustez (costes dobles y simulación de
caídas): se publica para medir su comportamiento real. Las reglas exactas no se publican y quedan congeladas.

- `c2/weights/AAAA-MM-DD.json`: pesos agregados para la apertura de esa sesión y qué subcartera se revisa.
- `c2/ledger.csv`: resultado diario, capital y caída máxima diaria acumulada.

## J1 (seguimiento en paper, desde el 12 de octubre de 2026)

Cuarta cartera virtual (100.000 $), **solo de seguimiento**. Combina un 80 % de rotación multiactivo de ETFs repartida en
5 subcarteras semanales con control diario de volatilidad y un 20 % de un componente de calendario (SPY en fechas
fijas del mes, BIL el resto). Es la única candidata que alcanzó el nivel "paper" del estándar de robustez del engine
(supera 7 de 8 pruebas; falla la de costes dobles). Reglas congeladas y no publicadas.

- `j1/weights/AAAA-MM-DD.json`: pesos agregados para esa sesión (incluye si el componente de calendario está en SPY).
- `j1/ledger.csv`: resultado diario, capital y caída máxima diaria acumulada.

## S4 (seguimiento en paper, desde el 12 de octubre de 2026)

Quinta cartera virtual (100.000 $), **solo de seguimiento**. No es una estrategia elegida por su historial, sino por un
método: el engine reparte la mitad entre las combinaciones de estrategias más estables de su historial y la otra mitad a
partes iguales entre todas. Ese método se comprobó eligiendo cada año solo con datos pasados y midiendo el año
siguiente: 2012-2023, 8,0 % anual frente a 7,6 % del reparto igual. Sin control de volatilidad, en 5 subcarteras
semanales. Puede tener pequeñas posiciones cortas. Reglas congeladas y no publicadas; se volverá a seleccionar una vez al
año, en enero, avisándolo aquí antes.

- `s4/weights/AAAA-MM-DD.json`: pesos agregados para esa sesión.
- `s4/ledger.csv`: resultado diario, capital y caída máxima diaria acumulada.

## Familia P: P2, P3, P3-L, P4, P5, P3K5 y P5K5 (seguimiento en paper, desde el 13 de octubre de 2026)

Siete carteras virtuales más, cada una con 100.000 $, **solo de seguimiento**. Son las carteras sistemáticas congeladas
del engine (reglas y pesos sin reajustar desde su congelación): combinan estrategias de días o semanas sobre ETFs (SMH,
IWM, XLP, XLV y una rotación defensiva entre TLT, GLD, HYG, UUP, XLU, XLP y XLV), con límites de exposición. P3K5 y P5K5
añaden un 20 % de tendencia en DBMF. En P2, P3 y P4 lo no invertido queda en efectivo sin remunerar; en P3-L, P5, P3K5 y
P5K5, en BIL. Solo largo y sin apalancamiento. Las reglas exactas no se publican.

- Las señales se deciden con el cierre y se suponen ejecutadas a ese mismo cierre, como en su backtest. Los archivos se
  publican después del cierre, así que en la práctica solo se podrían replicar en la apertura siguiente.
- Fricción supuesta: 0,05 % por lado sobre el peso de cada posición.
- `p/<cartera>/weights/AAAA-MM-DD.json` (carpetas p2, p3, p3l, p4, p5, p3k5, p5k5): pesos para esa sesión y en qué está
  cada estrategia, publicados tras el cierre anterior.
- `p/<cartera>/ledger.csv`: resultado diario, exposición, capital y caída máxima diaria acumulada.

## Estructura

- `signals/AAAA-MM-DD.json`: señales para la sesión de esa fecha, publicadas la tarde anterior (hora de Nueva York).
- `state/monthly/AAAA-MM.json`: pesos del bloque mensual de ese mes, publicados tras el cierre del último día hábil del mes anterior.
- `state/monthly_block.json`: qué estrategia ocupa el bloque mensual en cada periodo.
- `proofs/`: huellas SHA-256 de todos los archivos de datos en cada actualización, selladas con [OpenTimestamps](https://opentimestamps.org) (anclaje en Bitcoin) por GitHub Actions. Un `.ots` permite a cualquiera verificar que esos archivos existían en esa fecha: `ots verify proofs/<archivo>.sha256.ots`.
- `results/ledger.csv`: resultado diario de cada bloque y valor de la cartera. La columna `g4_ret` recoge el resultado del bloque mensual, sea G4 o P10.

## Cambios

- **2026-10-12** — Añadido el seguimiento en paper de la familia P (P2, P3, P3-L, P4, P5, P3K5 y P5K5), desde la sesión del 13-10-2026, en paralelo y sin cambiar las demás carteras.

- **2026-10-11** — Añadido el seguimiento en paper de S4 (empieza el 12-10-2026), en paralelo y sin cambiar las demás carteras.

- **2026-10-10** — Añadido el seguimiento en paper de J1 (empieza el 12-10-2026). C2 se mantiene como comparación; según el estándar de robustez del engine queda descartada.

- **2026-10-10** — Añadido el seguimiento en paper de C2 (empieza el 12-10-2026), en paralelo y sin cambiar G5 ni G3VT.

- **2026-10-08** — Corregidos los pesos de octubre de G4: la pieza de tendencia multiactivo se había calculado con 5 ETFs en lugar de su universo de 30. No se había publicado ninguna señal ni resultado antes de la corrección.
- **2026-10-08** — Añadido el registro independiente G3VT (empieza el 12-10-2026).
- **2026-10-08 (22:16)** — Aviso sobre el bloque táctico: sus 4 señales se validaron con datos 2008-2026. Funcionan en 2020-2026 (el periodo en que se diseñaron) pero no en 2012-2019; una de ellas no generaliza a activos parecidos. Se mantienen sin cambios para medir su comportamiento real, sabiendo que es probable que rindan menos que en su histórico.
- **2026-10-08 (22:13)** — Cambio del anuncio anterior, antes de cualquier operación: desde noviembre de 2026 el bloque mensual pasa de G4 a **P10**, no a N14. Motivo: con datos continuos 2006-2026, P10 cae menos en 2008-11, 2012-19 y 2020-23 (−9,6 %, −7,7 %, −9,0 %) que N14 (−12,7 %, −12,1 %, −14,3 %).
- **2026-10-08** — Anunciado con antelación: desde noviembre de 2026 el bloque mensual pasa de G4 a N14. Motivo: en una prueba de estrés con 2008-2011, G4 tuvo una caída diaria máxima del −24,9 %; N14, del −12,7 %.

## Aviso

Esto es un registro de **paper trading** (simulado). No es asesoramiento de inversión ni una oferta de servicios. Los resultados simulados no garantizan resultados reales.
