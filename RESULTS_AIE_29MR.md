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

### chunks más chicos
Resumen de 35 casos (juez local):
  recall@5: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.72
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 28677 de entrada, 14130 de salida

### menos fragmentos
Resumen de 35 casos (juez local):
  recall@3: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.50
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 24264 de entrada, 14565 de salida

### más fragmentos
Resumen de 35 casos (juez local):
  recall@8: 1.00   intención: 0.97   fuentes: 0.97   hechos: 0.69
  apoyo: 5.0/5   relevancia: 5.0/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 35069 de entrada, 16444 de salida

### base local, sin reranker
Resumen de 35 casos (juez local):
  recall@8: 1.00   intención: 0.94   fuentes: 0.97   hechos: 0.72
  apoyo: 5.0/5   relevancia: 4.9/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 33318 de entrada, 15033 de salida

### deployment decision-1
  Resumen de 35 casos (juez azure_evaluation):
  recall@5: 1.00   intención: 1.00   fuentes: 0.97   hechos: 0.69
  apoyo: 5.0/5   relevancia: 4.9/5   cita correcta: 1.0
  fugas de datos personales: 0   citas de fuentes prohibidas: 1
  tokens: 28663 de entrada, 15033 de salida

## Sesión 6 — Tools del MCP en el chat

Dominio: `loans`. MCP: el mock (`http://127.0.0.1:8200/mcp`) y, si ya tienes `MCP_URL`, el compartido (`bank-mcp`).

### Tools de tu dominio

| MCP | Tools que lista `dev/mcp/mcp_tools.py` |
|---|---|
| Mock | `get_credit_profile`, `simulate_installment`, `submit_loan_application` |
| Compartido | `mcp_tools.py` recibió HTTP 404 al listar; en las pruebas del chat se observaron `simulate_installment` y `submit_loan_application`, pero no se confirmó el listado completo. |

### Conversación en `dev/chat.py` (paso 12)

| Prueba | Mensaje | Tool pedida (`→`) | Qué pasó | Primer texto / respuesta completa (ms) |
|---|---|---|---|---|
| Consulta de tu dominio | ¿Cuánto pagaría al mes por un préstamo personal de S/ 20,000 a 24 meses? | `simulate_installment` ×3; probó TEA de 14.9%, 18% y 49.9% | Las tres llamadas devolvieron resultados, pero el agente los ignoró y dijo que no había simulación en los fragmentos. | 26,185 / 26,322 |
| Acción confirmada | Confirmo: registra un préstamo personal de S/ 8,000 a 12 meses para CUS-1003. | `submit_loan_application` | Quedó pendiente de aprobación; la acción no se ejecutó ni registró. El agente lo comunicó correctamente. | 16,034 / 16,185 |
| Sin tool que la resuelva | ¿Qué temperatura hará mañana en Lima? | Ninguna | Indicó que estaba fuera del alcance y no inventó un pronóstico. | 4,058 / 4,199 |
| Tool de otro dominio | ¿Cuál es el estado de mi solicitud de reposición REQ-6001? | Ninguna (`get_request_status` no está disponible en `loans`) | Dijo que no podía consultar el estado; no inventó uno. | 8,383 / 8,629 |

### ¿El modelo inventó algún dato cuando ninguna tool lo resolvía?

En las dos pruebas sin una tool disponible, no inventó la respuesta solicitada (pronóstico ni estado de solicitud). Sin embargo, en la consulta de préstamo sí propuso por su cuenta TEA de 14.9% y 49.9% como argumentos de la tool; además, aunque recibió resultados para las tres tasas, no los usó en la respuesta final.

### Conclusión

La aprobación humana y la falta de una tool se manejaron correctamente en estas pruebas. La consulta de préstamo no pasó: el modelo inventó parámetros, hizo llamadas redundantes y contradijo sus propios resultados. Revisar la instrucción y el flujo de tools antes de dar por validado el paso 12. Tiempos de una sola corrida; pueden variar.