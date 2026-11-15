    # Resultados AIE-31SC

## Sesión 5 — Comparar los dos jueces

Mismo golden set (3x casos), mismo agente (`gpt-5-mini`), dos jueces.

- Juez local (G-Eval): `data/eval/cards-20261113-101356.json`
- Juez azure_evaluation (decision-1): `data/eval/cards-20261113-100526.json`

| Métrica | G-Eval (`JUDGE=local`) | decision-1 (`azure_evaluation`) |
|---|---|---|
| Apoyo (1–5) | _(5.0/5)_ | _(5.0/5)_ |
| Relevancia (1–5) | _(5.0/5)_ | _(4.9/5)_ |
| Cita correcta (0–1) | _(1.0)_ | _(1.0)_ |
| Tokens (entrada / salida) | _(28847)_ | _(28841)_ |
| Tiempo de la corrida | _(anotar)_ | _(anotar)_ |

### ¿Las notas de apoyo y de cita correcta van en la misma dirección?

_(sI)_

### ¿Cuánto tardó cada corrida?

- Local (G-Eval): _(5)_.
- azure_evaluation (decision-1): _(4)_.

### Casos donde no coinciden

_(Relevancia)_

### Diferencias de la corrida local que no vienen del juez

El agente respondió distinto entre corridas (el modelo no es determinista):

- `cards-xy`: _(anotar)_.
- `cards-xx` (ruido): _(anotar)_.

### Conclusión

_(Hay ligera comparación)_
 