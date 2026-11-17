## Sesión 6 — Tools del MCP en el chat

Dominio: `loans`. MCP: el mock (`http://127.0.0.1:8200/mcp`) y, si ya tienes `MCP_URL`, el compartido (`bank-mcp`).

### Tools de tu dominio

| MCP | Tools que lista `dev/mcp/mcp_tools.py` |
|---|---|
| Mock | _(anotar)_ |
| Compartido | _(anotar)_ |

### Conversación en `dev/chat.py` (paso 12)

| Prueba | Mensaje | Tool pedida (`→`) | Qué pasó | Primer texto / respuesta completa (ms) |
|---|---|---|---|---|
| Consulta de tu dominio | _(¿Cuánto pagaría al mes por un préstamo de 20000 soles a 24 meses?)_ | _(simulate_installment)_ | _(respusta del agente)_ | _( Para S/ 20,000 a 24 meses, la cuota mensual sería S/ 985.64. Total pagado: S/ 23,655.36. Simulación realizada con una TEA de 18 %. ¿Quieres cambiar plazo o tasa para otra simulación?)_ |
| Acción confirmada | _(anotar)_ | _(anotar)_ | _(anotar)_ | _(anotar)_ |
| Sin tool que la resuelva | _(quiero confirmacion de ahorro a plazo fijo)_ | _(out_of_scope)_ | _(no resuelve)_ | _(Eso está fuera del alcance de préstamos. Soy agente de préstamos del Banco Demo. Para confirmar un depósito a plazo fijo, contacta al área de Ahorros del banco vía tu sucursal, la Banca Móvil/Internet o el centro de atención. Ten a mano tu número de cuenta y tu DNI. ¿Quieres que te indique los pasos en la app o cómo contactar al área de Ahorros?)_ |
| Tool de otro dominio | _(anotar)_ | _(anotar)_ | _(anotar)_ | — |

### ¿El modelo inventó algún dato cuando ninguna tool lo resolvía?

_(Solo me comento que no esta dentro de sus acciones, y me dio una guia de como solicitarlo)_

### Conclusión

_(Se debe establecer bien los parametrs para tener respuestas alineadas sin fuera de contexto o alucinación)_