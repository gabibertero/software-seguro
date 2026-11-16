# Resolución de laboratorios prácticos

## El Analista

1. Abrí la lista de ventas y vendedores. Había cuatro ventas y cuatro vendedores; todas aparecían como no asociadas. Guardé la captura del estado inicial.
2. Desde el formulario asocié la Venta 1 con Juan Pérez. La solicitud `POST /asociar/` envió `id_venta: "1"` e `id_vendedor: "1"`, y el servidor respondió `"Venta asociada al vendedor."`.
3. Intenté asociar la Venta 2 con Juan Pérez mediante otra solicitud normal. El servidor respondió `"Vendedor ya asociado."`, confirmando la restricción de una venta por vendedor en el flujo secuencial.
4. En Burp Repeater preparé dos solicitudes para el mismo vendedor, María García: una para la Venta 2 (`id_venta: "2"`, `id_vendedor: "2"`) y otra para la Venta 3 (`id_venta: "3"`, `id_vendedor: "2"`). Las agrupé y utilicé **Send group (parallel)**.
5. Al actualizar la página, las ventas 2 y 3 aparecían asociadas y María figuraba como asociada. Luego asocié la Venta 4 con Carlos López.
6. La pantalla final mostró las cuatro ventas asociadas a tres vendedores y confirmó que el desafío estaba superado.
7. El laboratorio devolvió la flag `db1ab6987f0624b58ae72fa69aba4d14`.

## Venta de autos

1. Consulté la documentación inicial de la API y localicé `GET /api/ventas?fecha=YYYY-MM-DD` y `GET /api/venta/{id}`.
2. Consulté las ventas del `2025-10-01` con la API key indicada en el laboratorio. La respuesta autorizada contenía 45 registros asociados a los vendedores accesibles (IDs 1 a 4), con IDs de venta entre `100901` y `101048`.
3. Consulté un ID ausente de la respuesta (`100922`) y recibí `403 Forbidden`. Como control, consulté un ID inexistente (`999999`) y recibí `404 Not Found`; así distinguí ventas existentes pero no autorizadas de IDs sin registro.
4. En Burp Intruder enumeré el rango de IDs `100901`–`101048`. Los resultados mostraron 99 respuestas `403` correspondientes a ventas fuera del alcance autorizado y con `101047` como ID más alto; hubo 45 respuestas autorizadas y 4 IDs inexistentes (`404`).
5. Concatené la cantidad de ventas externas y el ID más alto: `99` + `101047` = `99101047`.
6. Calculé el MD5 de `99101047` y el juez aceptó la flag `5064f37401dd1c8710edf69cd492784e`.


