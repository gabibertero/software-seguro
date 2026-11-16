# Resolución de laboratorios prácticos

## Consulta de multas

1. Inicié sesión con la cuenta de Ignacio y revisé el tráfico del perfil con Burp. La cookie `auth` contenía un JWT firmado con HS256. Al decodificarlo observé los campos `user_id`, `username`, `email`, `exp` y `orig_iat`.
2. Revisé la solicitud `GET /jsonp/?callback=procesarDatos` y encontré que la respuesta exponía el valor de `SECRET_KEY`, utilizado para firmar el token.
3. Como el editor web rechazaba esa clave por ser menor a 256 bits, instalé la extensión JWT Editor de Burp. Agregué la clave del laboratorio como una clave simétrica existente.
4. En la solicitud del perfil edité los campos `username` y `email` del JWT para que usaran el correo indicado para el administrador. Conservé los demás campos y volví a firmar el token con HS256 desde JWT Editor.
5. Envié la solicitud con el token actualizado. La aplicación mostró “SOS ADMINISTRADOR” y confirmó que el desafío estaba superado.
6. El laboratorio devolvió la flag `e470ca488c867e223fb`.

## Notas Universitarias

1. Ingresé con las credenciales de Benjamin y confirmé que la nota de Análisis Matemático I era 2.00. Después revisé `/admin/`, que mostraba el inicio de sesión de Django.
2. Probé las credenciales por defecto `admin:admin` indicadas en la consigna y pude entrar al panel. En “Blacklisted Tokens” encontré el token refresh entregado para el laboratorio y eliminé su registro de la lista negra. En “Outstanding Tokens” comprobé que el token correspondía al usuario Graciela Romero.
3. Envié el token refresh a `POST /api/token/refresh/`. Antes de quitarlo de la lista negra la solicitud respondía `401`; después devolvió un access token nuevo para el usuario 36.
4. Usé ese access token al consultar `/home/`. La respuesta mostró las notas de Física II de varios alumnos y formularios para guardar cambios. En el HTML observé que el formulario enviaba `materia_id`, `estudiante_id` y `nota` como campos modificables.
5. Reutilicé el formulario con `materia_id=2`, `estudiante_id=8` y `nota=9.0` para cambiar la nota de Benjamin en Análisis Matemático I. El servidor respondió con una redirección a `/home/` y el laboratorio confirmó que el objetivo se había completado.
6. El laboratorio devolvió la flag `660b416167e7fd839bc06c61bb5a184b`.

## ECommerce

1. El escenario planteaba que tenía las credenciales filtradas de juan (admin), pero su cuenta usa 2FA por email, así que con la contraseña sola no podía entrar. Mi usuario era maria.
2. Revisé el endpoint `PUT /profile/` y vi que el cuerpo JSON acepta un campo `id`. Como maria (id 2), cambié el `id` a 1 (juan) y le reescribí el `email` por uno que controlo (`gabilab@yopmail.com`). El servidor aceptó el cambio sin validar que el `id` fuera el de mi sesión (IDOR).
3. Como el `PUT` exige todos los campos obligatorios, completé `first_name` y `last_name` con valores de relleno, ya que para el objetivo solo importaba el email.
4. Inicié sesión como juan con la contraseña filtrada. El código de 2FA llegó a mi casilla de yopmail (porque el email de juan ahora era el mío), lo ingresé y completé el login como administrador.
5. Ya como juan, compré el producto "Memoria RAM 16GB DDR4" desde su cuenta, cumpliendo el objetivo del laboratorio.
6. El laboratorio devolvió la flag `fe01348e4bf437fe03688896f7889107`.

## Snow Storm

1. Inicié sesión con mi cuenta de Carlos y revisé el perfil, las publicaciones y las solicitudes disponibles en Burp. La aplicación mostraba los posts de Juan Pérez, pero sus identificadores correspondían a publicaciones y no permitían modificar directamente su perfil.
2. Revisé el flujo de recuperación de contraseña. La solicitud `POST /api/forgot/` acepta el correo electrónico y confirma el envío del enlace, pero no era necesario acceder al buzón de Juan para completar el laboratorio.
3. Solicité enlaces de recuperación para una cuenta de prueba propia y observé que los tokens tenían formato MD5. Al compararlos, comprobé que se generaban a partir de números consecutivos, lo que permitía probar candidatos.
4. En Burp Intruder preparé una solicitud `GET /recovery?t=...`, marqué únicamente el valor del parámetro `t` y configuré una lista numérica del 1506 al 1706. A cada número le apliqué la transformación MD5.
5. Comparé las respuestas de Intruder y abrí las que tenían una longitud diferente. La respuesta válida mostró el formulario “Recuperar Contraseña”, con los campos para establecer y repetir una nueva clave.
6. Usé ese formulario para establecer una nueva contraseña para Juan. Luego inicié sesión con su correo, abrí el perfil y cambié su nombre a `Soy un idiota`.
7. El laboratorio confirmó el objetivo y devolvió la flag `17968af07cf621117b36cfbc35b51361`.

## Local Storage and Cookie

1. Abrí las herramientas de desarrollo del navegador y entré en la pestaña “Application”. En el almacenamiento del dominio del laboratorio encontré el valor `local_challenge` en Local Storage y la cookie del desafío.
2. Calculé el hash MD5 de la palabra `hackertech`, que es `6397550e14d36fc51b5049e28c40876f`.
3. Reemplacé por ese hash el valor de la cookie del desafío y el valor de `local_challenge`, sin modificar sus nombres.
4. Presioné “Validar”. La página confirmó que el desafío estaba resuelto y mostró la flag `2a67fed05c1362c6f39e7ddb95d96101`.
