# Automatización vs. explotación manual (sqlmap)

sqlmap automatiza la detección y explotación de SQLi, pero para usarlo bien primero hay que entender cómo llega la aplicación al parámetro vulnerable y cómo se reconoce una respuesta exitosa. No alcanza con decir que falla porque hay un token, un JSON, un WAF o una inyección de segundo orden: la herramienta tiene opciones para manejar muchos de esos casos (`-r`, `--csrf-token`, `--eval`, `--base64`, `--tamper`, `--second-url` y `--second-req`, entre otras). Los siguientes son dos escenarios en los que una prueba automática **sin adaptación al flujo concreto** puede fallar y una investigación manual con Burp Suite puede encontrar la SQLi.

## 1. Secuencia de varios pasos con valores dependientes del estado

Supongamos un carrito en el que primero se crea una orden, luego se obtiene un token de un solo uso y finalmente se envía un parámetro junto con una firma calculada sobre el contenido de la orden. Si repito solo la última petición cambiando ese parámetro, el servidor rechaza la firma o el token antes de construir la consulta vulnerable. sqlmap recibe respuestas de rechazo y puede concluir que no hay inyección, aunque el parámetro sí llegue a SQL dentro de un flujo válido.

Con Burp puedo recorrer los pasos, comparar peticiones, identificar de dónde sale cada valor y probar una modificación controlada en el momento adecuado. Una vez entendido el flujo, podría automatizarlo o configurar sqlmap con una petición capturada, recuperación del token y lógica personalizada. El obstáculo no es que sqlmap sea incapaz de usar tokens: es que una petición aislada no reproduce automáticamente toda la lógica específica de esta aplicación.

## 2. Filtro y señal de respuesta propios de la aplicación

Imaginemos que un WAF bloquea una sintaxis concreta y, además, la página cambia en cada visita por publicidad o un contador. Una ejecución estándar puede recibir bloqueos o comparar páginas variables sin distinguir la condición verdadera de la falsa. En Burp puedo probar manualmente qué carácter o estructura provoca el bloqueo y hallar una señal estable: un texto, código HTTP, cambio de longitud o demora que sí depende de la condición inyectada.

Después puedo trasladar esos hallazgos a sqlmap mediante un ajuste del punto de inyección, un *tamper script* apropiado y opciones como `--string`, `--not-string`, `--regexp` o `--code`. Si la transformación que exige el filtro es específica o la respuesta requiere interpretar varios pasos, quizá convenga continuar con pruebas manuales o escribir una integración propia. Un WAF o un cuerpo JSON por sí solos **no** prueban que sqlmap vaya a fallar.

También existe la SQLi de segundo orden: el valor se guarda en una petición y se usa después en otra pantalla. sqlmap admite `--second-url` y `--second-req`, pero primero hay que descubrir dónde se manifiesta el efecto y si requiere acciones intermedias. Esa comprensión de la aplicación es el aporte del pentester.

**Conclusión:** la explotación manual sirve para descubrir el contexto, las dependencias y la señal de éxito. sqlmap puede automatizar gran parte del trabajo una vez que se configura con esa información.

## Fuentes

- [sqlmap, documentación de uso](https://github.com/sqlmapproject/sqlmap/wiki/Usage): peticiones capturadas, tokens, `--eval`, formatos codificados, *tamper scripts*, comparación de respuestas y segundo orden.
