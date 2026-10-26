# Resolución de laboratorios prácticos

## Apagar la IA

1. Abrí el laboratorio y revisé el código fuente de la página. Encontré enlaces a dos páginas bajo la ruta `/codes/`, pero esas páginas mostraban listas de números y no el código final.
2. Usé Burp Suite Intruder para probar rutas del mismo formato y comparé las respuestas. Las páginas que devolvían contenido permitieron continuar el reconocimiento y encontrar otras listas.
3. En una de las páginas encontré el código de 16 dígitos `5524663362514956`.
4. Calculé el hash MD5 del código y envié el resultado al juez. El hash aceptado fue `a8e0e8ff02dde0f62fdf4de5142d7de0`.

## El mejor secreto

1. Abrí el laboratorio y era un video de un jefe de estado tecleando la clave del `secreto.zip` en un teclado numérico trucho, donde cada tecla era un dígito distinto que no sabía cuál era. Había que descifrar el zip.
2. Miré el zip con `zipinfo` y vi que estaba cifrado con AES y no con el ZipCrypto viejo, así que `fcrackzip` no me servía y lo tenía que romper offline con john/hashcat.
3. En vez de mirar el video me puse a escuchar el audio, porque cada tecla sonaba distinto. Saqué que eran 12 toques con 5 teclas diferentes.
4. Con eso armé una lista con todas las combinaciones posibles de esos números (`numeros_12digitos_v4.txt`) para usarla como diccionario.
5. Le pasé la lista al zip con john (`zip2john secreto.zip > hash.txt` y `john --wordlist=numeros_12digitos_v4.txt hash.txt`) y me tiró la clave: `547795999937`.
6. Abrí el `secreto.txt` con esa clave y mandé al juez el código `696026dd5bf583f34530a657d896ebea`.

## Votación

1. Abrí el laboratorio y observé los votos de UTN y Harvard.
2. Revisé las peticiones de la página y encontré que el envío del voto se realizaba mediante `POST /src/ctl/votacion.ctl.php`, con el parámetro `opUniversidad`.
3. Envié la petición a Burp Suite Intruder. Para variar las solicitudes, marqué la letra `p` de `Apple` dentro del valor de `User-Agent` como posición de payload.
4. Ejecuté el ataque por tandas y revisé las respuestas para seguir el conteo. Continué hasta que la cantidad de votos de UTN superó a la de Harvard.
5. Cuando se cumplió la condición del laboratorio, obtuve y envié al juez el código `sdf7fdj843if3jhg3`.

## Votación nueva versión

1. Abrí el laboratorio y vi que, a diferencia de la versión anterior, al votar se setea una cookie `voto` y el servidor rechaza los reenvíos.
2. Probé borrar la cookie `voto` y cambiar `PHPSESSID`, pero el servidor volvía a setear el mismo token `voto`, así que el control de duplicados no estaba en las cookies.
3. En Repeater fui cambiando un campo por vez y mirando el `Set-Cookie: voto=` de la respuesta. El `User-Agent` no alteraba el token, pero al agregar el header `X-Forwarded-For` con una IP el token `voto` cambió: la aplicación identifica al votante por la IP que toma de ese header, que es controlable por el cliente.
4. Envié la petición a Intruder (modo Sniper), marqué la IP del `X-Forwarded-For` como posición de payload y usé payload tipo Numbers para iterar muchas IPs distintas, sumando un voto por cada una.
5. Repetí hasta cumplir la condición del laboratorio y envié al juez el código `143885b3abc1012375b3846f84c39203`.
