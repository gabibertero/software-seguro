# Objetivo 2: Ingeniería Inversa a JSON Web Tokens (JWT)

Revisé en Burp el token devuelto por la aplicación de prueba DummyJSON después de iniciar sesión.

El token analizado fue el access token recibido en la respuesta de `POST /auth/login`. Su header contiene `alg: HS256` y `typ: JWT`. `HS256` identifica el algoritmo de firma HMAC que utiliza SHA-256; `typ` indica que el formato del token es JWT.

El payload incluye `id: 1` y `username: emilys`. También muestra el correo electrónico `emily.johnson@x.dummyjson.com`, el nombre Emily, el apellido Johnson, el género `female` y la dirección de la imagen de perfil de la cuenta. Además, aparecen `iat` y `exp`, que representan la fecha de emisión y la fecha de vencimiento del token en formato Unix.

No encontré un campo que indique el rol del usuario. El header y el payload se pueden leer al decodificarlos; no están cifrados.
