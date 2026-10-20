# Resolución de laboratorios prácticos

## Apagar la IA

1. Abrí el laboratorio y revisé el código fuente de la página. Encontré enlaces a dos páginas bajo la ruta `/codes/`, pero esas páginas mostraban listas de números y no el código final.
2. Usé Burp Suite Intruder para probar rutas del mismo formato y comparé las respuestas. Las páginas que devolvían contenido permitieron continuar el reconocimiento y encontrar otras listas.
3. En una de las páginas encontré el código de 16 dígitos `5524663362514956`.
4. Calculé el hash MD5 del código y envié el resultado al juez. El hash aceptado fue `a8e0e8ff02dde0f62fdf4de5142d7de0`.

## El mejor secreto

Pendiente de realizar y documentar.

## Votación

1. Abrí el laboratorio y observé los votos de UTN y Harvard.
2. Revisé las peticiones de la página y encontré que el envío del voto se realizaba mediante `POST /src/ctl/votacion.ctl.php`, con el parámetro `opUniversidad`.
3. Envié la petición a Burp Suite Intruder. Para variar las solicitudes, marqué la letra `p` de `Apple` dentro del valor de `User-Agent` como posición de payload.
4. Ejecuté el ataque por tandas y revisé las respuestas para seguir el conteo. Continué hasta que la cantidad de votos de UTN superó a la de Harvard.
5. Cuando se cumplió la condición del laboratorio, obtuve y envié al juez el código `sdf7fdj843if3jhg3`.

## Votación nueva versión

Pendiente de realizar y documentar.
