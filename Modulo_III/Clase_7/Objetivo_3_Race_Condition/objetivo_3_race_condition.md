# Anatomía de una Condición de Carrera (Race Condition)

Una condición de carrera pasa cuando dos o más operaciones trabajan sobre el mismo dato al mismo tiempo y el resultado final depende del orden en que se ejecutan. En seguridad se aprovecha cuando hay una ventana de tiempo entre que la aplicación comprueba una condición y que la actualiza (lo que se conoce como TOCTOU, time-of-check to time-of-use).

El caso del código de descuento de un solo uso funciona así: cuando se canjea, el servidor primero chequea si el código sigue disponible y después lo marca como usado. Entre esos dos pasos hay un instante en el que el código todavía figura como válido. Si en ese instante llegan varias peticiones, todas pasan el chequeo antes de que la primera alcance a marcarlo como usado.

Con una herramienta como el Turbo Intruder de Burp Suite, el atacante prepara la petición de canje y la envía unas 50 veces de forma simultánea (el Turbo Intruder permite mandarlas casi en paralelo e incluso sincronizarlas para que lleguen juntas). Como todas caen dentro de esa ventana, el servidor procesa varias antes de actualizar la base de datos, y el código de un solo uso termina aplicándose muchas veces.

Para mitigarlo, la operación tiene que ser atómica: el chequeo y la actualización deben hacerse como una sola unidad, por ejemplo con bloqueos a nivel de base de datos o una transacción que impida que dos canjes del mismo código se procesen a la vez.
