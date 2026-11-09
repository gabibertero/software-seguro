# Automatización vs. pruebas manuales con sqlmap

sqlmap ahorra muchísimo trabajo al probar una SQLi, pero necesita que la petición llegue al código vulnerable y que la respuesta le dé alguna señal para reconocer el resultado. Si le paso una petición aislada sin entender el flujo de la aplicación, puede fallar aunque haya una inyección. Estos son dos casos donde primero probaría a mano con Burp.

## 1. Una compra que depende de pasos anteriores

Supongamos que el carrito crea una orden, entrega un token de un solo uso y después exige una firma calculada sobre el contenido. Si sqlmap repite únicamente la última petición y cambia un parámetro, el servidor rechaza el token o la firma **antes** de consultar la base de datos. La herramienta ve rechazos, no la SQLi.

En Burp seguiría la compra completa y compararía las peticiones para saber cuándo se genera cada valor. Recién ahí probaría el parámetro dentro de un flujo válido. Luego podría configurar o complementar sqlmap para repetir ese flujo. No es que la herramienta no sepa manejar tokens: tiene opciones como `--csrf-token`, `--eval` y `-r`. Lo difícil es reproducir la lógica particular de esa compra.

## 2. Un filtro y una página que cambia todo el tiempo

Otro caso: el WAF bloquea cierta sintaxis y la página muestra un contador o publicidad variable. Una prueba automática común puede confundir el bloqueo con una respuesta normal, o no distinguir una condición verdadera de una falsa porque el contenido siempre cambia.

Yo compararía manualmente qué entrada dispara el bloqueo y buscaría una señal estable: un texto, un código HTTP o una demora que sí dependa de la condición. Después podría indicársela a sqlmap con opciones como `--string`, `--not-string`, `--regexp` o `--code`, y ajustar el payload con un *tamper script*. Si la respuesta exige interpretar varios pasos, quizá haga falta una integración propia. Un WAF o un JSON por sí solos no vuelven inútil a sqlmap.

La misma idea vale para una SQLi de segundo orden: primero hay que encontrar dónde aparece el efecto de un dato guardado. sqlmap tiene `--second-url` y `--second-req`, pero no puede adivinar por sí solo cómo funciona esta aplicación. La prueba manual me sirve para descubrir el contexto; después automatizo lo que tenga sentido.

## Fuentes

- [sqlmap, documentación de uso](https://github.com/sqlmapproject/sqlmap/wiki/Usage): peticiones capturadas, tokens, `--eval`, comparación de respuestas, *tamper scripts* y segundo orden.
