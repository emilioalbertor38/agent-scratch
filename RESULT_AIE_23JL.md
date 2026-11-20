## Sesión 6 — Tools del MCP en el chat

Dominio: `loans`. MCP: el mock (`http://127.0.0.1:8200/mcp`) y, si ya tienes `MCP_URL`, el compartido (`bank-mcp`).

### Tools de tu dominio

| MCP | Tools que lista `dev/mcp/mcp_tools.py` |
|---|---|
| Mock | `get_credit_profile`, `simulate_installment`, `submit_loan_application` |
| Compartido | `get_credit_profile`, `simulate_installment`, `submit_loan_application` |

### Conversación en `dev/chat.py` (paso 12)

| Prueba | Mensaje | Tool pedida (`→`) | Qué pasó | Primer texto / respuesta completa (ms) |
|---|---|---|---|---|
| Consulta de tu dominio | ¿Cuánto pagaría al mes por un préstamo de 20000 soles a 24 meses? | `simulate_installment` | El agente calculó la cuota con el MCP; no fue una estimación del modelo. | No medido / Para S/ 20,000 a 24 meses, la cuota mensual sería S/ 985.64. Total pagado: S/ 23,655.36. Simulación realizada con una TEA de 18 %. ¿Quieres cambiar plazo o tasa para otra simulación? |
| Acción confirmada | Confirmación del usuario de una solicitud de préstamo | `submit_loan_application` | La herramienta fue identificada como acción sensible y quedó pendiente de aprobación; no se ejecutó ni registró. | No medido |
| Sin tool que la resuelva | quiero confirmacion de ahorro a plazo fijo | Ninguna (`out_of_scope`) | El agente indicó que no podía resolver la solicitud y derivó al área de Ahorros. | No medido / Eso está fuera del alcance de préstamos. Soy agente de préstamos del Banco Demo. Para confirmar un depósito a plazo fijo, contacta al área de Ahorros del banco vía tu sucursal, la Banca Móvil/Internet o el centro de atención. Ten a mano tu número de cuenta y tu DNI. ¿Quieres que te indique los pasos en la app o cómo contactar al área de Ahorros? |
| Tool de otro dominio | quiero bloquear mi tarjeta | `block_card` no está disponible en el dominio `loans` | El agente no recibió una tool que resolviera la solicitud y respondió dentro de su alcance; no ejecutó el bloqueo. | No medido |

### ¿El modelo inventó algún dato cuando ninguna tool lo resolvía?

No inventó datos ni simuló la operación. Solo indicó que estaba fuera del alcance de préstamos y dio una guía para consultar el depósito a plazo fijo con el área de Ahorros.

### Conclusión

La configuración de herramientas por dominio evitó mezclar préstamos con tarjetas u ahorros. Conviene mantener esta separación y agregar una respuesta clara cuando la solicitud no tenga una tool disponible.

## Sesión 7 — Skills, memoria de trabajo y Agent Framework

Dominio: `loans`. Misma secuencia en ambos runtimes, con Foundry `gpt-5-mini` y el MCP mock local.

| | `react` | `agent_framework` |
|---|---|---|
| Tools pedidas en la consulta | `simulate_installment` | `simulate_installment` |
| La acción sensible quedó **NO SE EJECUTA** | Sí: `get_credit_profile` y `submit_loan_application`; la segunda quedó pendiente de aprobación. | Sí: `get_credit_profile` y `submit_loan_application`; la segunda quedó pendiente de aprobación. |
| Pregunta de memoria respondida sin `→` | Sí: recordó S/ 20,000 a 24 meses. | Sí: recordó S/ 20,000 a 24 meses. |
| Primer texto (ms) | 19,128 | 19,137 |
| Respuesta completa (ms) | 19,343 | 19,190 |
| Tokens (entrada / salida) | 3,335 / 804 | 3,335 / 741 |

### ¿La skill frenó la tool de otro dominio?

Mensaje: «¿Puedes bloquear mi tarjeta?». Sí: ninguno pidió `block_card`. `react` dijo que no podía bloquear tarjetas desde el servicio de préstamos y recomendó usar la app, la banca telefónica o una agencia. `agent_framework` también indicó que el bloqueo está fuera de su alcance y remitió a los canales de tarjetas del banco.

### Diferencias entre los dos runtimes

En esta ejecución ambos usaron las mismas tools para la consulta y la solicitud, respetaron la aprobación pendiente, recordaron el monto y plazo sin otra tool y rechazaron el bloqueo de tarjeta. La cuota y el total coincidieron: S/ 985.64 y S/ 23,655.36. `agent_framework` terminó la primera respuesta 153 ms antes y generó 63 tokens menos; el primer texto llegó 9 ms después. Es una sola ejecución, así que las diferencias de latencia y tokens no son un benchmark concluyente.

### Conclusión

La skill limitó correctamente las tools al dominio `loans`; la memoria de conversación funcionó y la acción sensible no se ejecutó en ninguno de los runtimes. En esta prueba ambos dieron resultados funcionalmente equivalentes. Las métricas son de una sola corrida con el MCP mock local.