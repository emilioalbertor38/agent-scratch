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
 
### ¿Las notas de apoyo y de cita correcta van en la misma dirección?
 
_(sI)_
 
### ¿Cuánto tardó cada corrida?
 
- Local (G-Eval): _(5)_.
- azure_evaluation (decision-1): _(4)_.
 
### Casos donde no coinciden
 
_(Relevancia)_
 
### Diferencias de la corrida local que no vienen del juez
 
El agente respondió distinto entre corridas (el modelo no es determinista):
 
- `cards-xy`: No hay resultados por caso disponibles en el workspace para describir con precisión qué cambió entre corridas.
- `cards-xx` (ruido): Tampoco está disponible el detalle de respuestas de este caso; no es posible comparar las corridas sin esos resultados.
 
### Conclusión
 
_(Hay ligera comparación)_



## Sesión 7 — Skills, memoria de trabajo y Agent Framework

Dominio: `loans`. La misma conversación con los dos runtimes (paso 7).

| | `react` | `agent_framework` |
|---|---|---|
| Tools pedidas en la consulta | _(simulate_installment)_ | _(personal-loan-terms.docx)_ |
| La acción sensible quedó **NO SE EJECUTA** | Sí; la solicitud quedó pendiente de aprobación humana. | Sí; la solicitud quedó pendiente de aprobación humana. |
| Pregunta de memoria respondida sin `→` | _(La cuota mensual que te simulé fue S/ 985.64 (TEA 18%))_ | _(Según los documentos: las comisiones forman parte del costo total del préstamo (TCEA))_ |
| Primer texto (ms) | _(13113)_ | _(14906)_ |
| Respuesta completa (ms) | _(13167)_ | _(15231)_ |
| Tokens (entrada / salida) | _(2136/1247)_ | _(2020/1413)_ |

### ¿La skill frenó la tool de otro dominio?

Mensaje: «Quiero registrar una solicitud de préstamo». Respuesta: indicó que la solicitud requiere confirmación y no afirmó que ya hubiera sido registrada.

### Diferencias entre los dos runtimes

`react` pidió explícitamente `simulate_installment` y mantuvo el resultado calculado en la memoria de trabajo. `agent_framework` obtuvo el contexto principalmente mediante RAG y orquestó el turno con sus funciones como runtime.

### Conclusión

Ambos runtimes permanecieron dentro del dominio `loans`; las acciones sensibles quedaron bloqueadas hasta aprobación humana. `react` mostró mayor trazabilidad de la tool de cálculo y `agent_framework` mostró mayor integración del contexto recuperado por documentos.