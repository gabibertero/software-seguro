# El Espejismo del Cifrado en el Cliente (Frontend Encryption)

La afirmación de que cifrar el body en el navegador hace imposible interceptar y manipular los datos es una falsa sensación de seguridad. El fallo de fondo es simple: si el cifrado se hace en el navegador, entonces todo lo necesario para cifrar y descifrar (el algoritmo, la lógica y la clave) tiene que estar presente en el cliente. Y todo lo que está en el cliente, el usuario lo controla y lo puede leer.

El cifrado en tránsito ya lo resuelve HTTPS/TLS. Agregar un cifrado propio en JavaScript no protege contra alguien sentado en su propia máquina, porque ese alguien está de este lado del cifrado, no en el medio del camino.

Los pasos que haría un atacante para evadir esta protección:

1. Abrir las DevTools del navegador y revisar el JavaScript de la página para encontrar la función que cifra el body antes de enviarlo.
2. Usar esa misma función desde la consola del navegador para cifrar los valores que quiera, o para descifrar lo que ve viajar y entender el formato.
3. Como alternativa, poner un breakpoint justo antes del cifrado para ver y modificar los datos en texto plano, o justo después para leer lo que se manda.
4. Con eso arma el payload cifrado con sus valores manipulados y lo envía, desde la consola o reenviando la petición con Burp.

Como el servidor recibe algo que se cifró con su propia lógica, lo descifra y lo procesa como válido. El proxy sigue sirviendo: aunque en Burp se vea el texto cifrado, la manipulación se hace en el navegador, que es donde está la llave. La conclusión es que la validación y la seguridad real tienen que estar en el servidor; el cliente nunca es un lugar confiable para proteger datos.
