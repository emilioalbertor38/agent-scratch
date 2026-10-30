Las dos primeras filas se midieron con el mock local de Foundry. La última corresponde a la ejecución compartida por el usuario; la latencia depende de su entorno.

| Mensaje | Intención | latency_ms | prompt_tokens | completion_tokens |
|---|---|---:|---:|---:|
| Hola, buenos días | greeting | 3565 | 365 | 118 |
| ¿A cuánto está el dólar hoy? | out_of_scope | 3418 | 369 | 266 |
| A cuanto está el dolar hoy? | out_of_scope | 4509 | 578 | 287 |