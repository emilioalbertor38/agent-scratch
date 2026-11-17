## Sesión 5
### golden set 30 casos
Resumen de 30 casos (juez local):
  recall@5: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.70
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 24659 de entrada, 12282 de salida

### golden set 35 casos
Resumen de 35 casos (juez local):
  recall@5: 1.00   intención: 0.97   fuentes: 0.97   hechos: 0.69
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 28677 de entrada, 14647 de salida

## chunks más chicos
Resumen de 35 casos (juez local):
  recall@5: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.72
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 28677 de entrada, 14130 de salida

## menos fragmentos
Resumen de 35 casos (juez local):
  recall@3: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.50
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 24264 de entrada, 14565 de salida

## más fragmentos
Resumen de 35 casos (juez local):
  recall@8: 1.00   intención: 0.97   fuentes: 0.97   hechos: 0.69
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 35069 de entrada, 16444 de salida

## base local, sin reranker
Resumen de 35 casos (juez local):
  recall@8: 1.00   intención: 0.94   fuentes: 0.97   hechos: 0.72
  apoyo: 5.0/5   relevancia: 4.9/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 33318 de entrada, 15033 de salida

## deployment decision-1
Resumen de 35 casos (juez azure_evaluation):
  recall@5: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.69
  apoyo: 5.0/5   relevancia: 4.9/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 28663 de entrada, 15033 de salida

### Resultados de Evaluación - Golden Set (35 casos)

#### 1. Comparativa de Evaluadores

| Control / Métrica | Juez Local | Juez Azure Evaluation | Meta | Estado |
| :--- | :---: | :---: | :---: | :---: |
| **recall@5** | 1.00 | 1.00 | ≥ 0.80 | ✅ Cumple |
| **hechos** | 0.69 | 0.69 | ≥ 0.75 | ❌ No cumple |
| **apoyo** | 5.0 / 5 | 5.0 / 5 | ≥ 4.0 | ✅ Cumple |
| **fugas de datos personales** | 0 | 0 | 0 | ✅ Cumple |
| **citas de fuentes prohibidas** | 1 | 1 | 0 | ❌ No cumple |
| *Intención (referencia)* | 0.97 | 1.00 | - | - |
| *Fuentes (referencia)* | 0.97 | 0.97 | - | - |
| *Relevancia (referencia)* | 5.0 / 5 | 4.9 / 5 | - | - |
| *Cita correcta (referencia)* | 1.00 | 1.00 | - | - |
| *Tokens (entrada / salida)* | 28,677 / 14,647 | 28,663 / 15,033 | - | - |

#### 2. Conclusión (Deployment Decision - 1)
- **Estado de despliegue:** **NO APTO (No-Go)**.
- Ambos evaluadores coinciden en que la recuperación (`recall@5 = 1.00`), seguridad PII (`0` fugas) y calidad de apoyo (`5.0/5`) superan las metas establecidas.
- Sin embargo, no se alcanzan los criterios mínimos en **fidelidad de hechos (`0.69 < 0.75`)** ni en **restricción de fuentes prohibidas (`1 > 0`)**.

#### 3. Acciones Correctivas Propuestas
1. **Para mejorar la métrica de `hechos` (≥ 0.75):**
   - **Prompting estricto (*Grounding*):** Instruir explícitamente al modelo para que limite sus afirmaciones exclusivamente a los datos presentes en el contexto recuperado y declare falta de información si un dato no consta, evitando deducciones propias.
   - **Ajuste de hiperparámetros:** Configurar `temperature = 0` para minimizar extrapolaciones en la generación.
   - **Refinamiento de contexto:** Reducir el ruido en los fragmentos enviados al LLM (ajustando el *chunking* o priorizando los top-3 fragmentos tras *reranking*).

2. **Para eliminar las `citas de fuentes prohibidas` (= 0):**
   - **Filtrado a nivel de recuperación (*Pre-filtering*):** Aplicar filtros de metadatos en la búsqueda vectorial/híbrida para excluir documentos marcados como restringidos o prohibidos antes de construir el prompt.
   - **Guardrail de salida (*Post-processing*):** Validar las citas generadas contra una lista de exclusión de fuentes prohibidas antes de entregar la respuesta final al usuario.
