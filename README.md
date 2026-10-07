# G5 Paper Trading

Registro público y con fecha de las señales de **G5**, una cartera sistemática solo largo y sin apalancamiento. El objetivo es construir un historial forward (TRUE_FORWARD) verificable: cada señal se publica **antes de la apertura** de la sesión en la que se ejecuta y su resultado se añade después del cierre.

## Composición

| Bloque | Peso | Descripción |
|---|---|---|
| G4 (asset allocation) | 50% | Rotación sectorial por momentum + tendencia en QQQ + tendencia multiactivo. Rebalanceo mensual. |
| Bloque táctico | 50% | 4 señales diarias (E382, M3065, R152, Y12050) sobre QQQ, XLK y SMH. Entrada en la apertura y salida al cierre del mismo día. 0,5x por señal, tope de exposición 1,0x del bloque. Sin señal, el bloque está en efectivo (BIL). |

- Capital virtual inicial: **100.000 $**
- Inicio del registro: **octubre de 2026**
- Fricción supuesta: 0,05% por operación
- Datos: Massive (antes Polygon.io), barras diarias ajustadas y dividendos

Las reglas exactas de cada señal no se publican. Lo que se publica es la señal y su resultado, con la fecha del commit como prueba de que existía antes de la apertura.

## Estructura

- `signals/AAAA-MM-DD.json`: señales para la sesión de esa fecha, publicadas la tarde anterior (hora de Nueva York).
- `results/ledger.csv`: resultado diario de cada bloque y valor de la cartera.

## Aviso

Esto es un registro de **paper trading** (simulado). No es asesoramiento de inversión ni una oferta de servicios. Los resultados simulados no garantizan resultados reales.
