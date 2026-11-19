    # Resultados AIE-31SC

## Sesión 5 — Comparar los dos jueces

Mismo golden set (3x casos), mismo agente (`gpt-5-mini`), dos jueces.

- Juez local (G-Eval): `data/eval/loans-20261113-144151.json`
- Juez azure_evaluation (decision-1): `data/eval/loans-20261113-150351.json`

| Métrica | G-Eval (`JUDGE=local`) | decision-1 (`azure_evaluation`) |
|---|---|---|
| Apoyo (1–5) | 5.0/5 | 4.97/5 |
| Relevancia (1–5) | 5.0/5 | 4.87/5 |
| Cita correcta (0–1) | 0.968 | 0.998 |
| Tokens (entrada / salida) | 28847 / 16612 | 28841 / 15433 |
| Tiempo de la corrida | 159804 ms | 148151 ms |

### ¿Las notas de apoyo y de cita correcta van en la misma dirección?

Sí. Ambos jueces puntúan apoyo y citas con resultados muy altos y cercanos: el apoyo es 5.0 frente a 4.97 y la cita correcta es 0.968 frente a 0.998.

### ¿Cuánto tardó cada corrida?

- Local (G-Eval): 159.804 segundos (35 casos; promedio por caso: 4566 ms).
- azure_evaluation (decision-1): 148.151 segundos (35 casos; promedio por caso: 4233 ms).

### Casos donde no coinciden

Coinciden en intención y en la mayoría de las respuestas, pero difieren en varias evaluaciones de exactitud:

- `loans-08` y `loans-18`: G-Eval marca `facts_ok=true`, mientras decision-1 lo marca `facts_ok=false`.
- `loans-19`: G-Eval marca `facts_ok=false`, mientras decision-1 lo marca `facts_ok=true`.
- `loans-30`: G-Eval marca `facts_ok=false`, mientras decision-1 lo marca `facts_ok=true`.
- `loans-24`: G-Eval marca `facts_ok=false`, mientras decision-1 lo marca `facts_ok=true`.
- `loans-AIE23JL-01`: G-Eval marca `sources_ok=false`; decision-1 marca `sources_ok=true`.

La métrica agregada de relevancia es ligeramente superior con G-Eval (5.0 frente a 4.87).

### Diferencias de la corrida local que no vienen del juez

El agente respondió distinto entre corridas (el modelo no es determinista):

- `loans-AIE23JL-01`: G-Eval considera que no se usaron correctamente las fuentes y penaliza `sources_ok`; decision-1 considera que sí se consultó la fuente adecuada, aunque ambos marcan la respuesta como `facts_ok=false`.
- `loans-AIE23JL-04`: ambos jueces marcan `facts_ok=false`, aunque la respuesta aporta información sobre seguros y selección de aseguradora; puede requerir una redacción más breve y estricta respecto del corpus.

Además, ambas corridas tienen cero fugas de datos personales y coinciden en mantener `intent_accuracy=1.0`.

### Conclusión

Las dos corridas son consistentes: ambas obtienen intención perfecta, cero fugas de datos personales y puntuaciones altas de apoyo, relevancia y citas. La diferencia principal es que decision-1 puntúa ligeramente mejor las citas (0.998 frente a 0.968) y G-Eval la relevancia y el apoyo (5.0 frente a 4.87/4.97). Los casos anteriores muestran que las discrepancias provienen principalmente de la evaluación de hechos y fuentes, no de una diferencia relevante en la intención.
 