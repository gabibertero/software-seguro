# Anatomía de una Condición de Carrera (Race Condition)

Una condición de carrera pasa cuando dos o más operaciones trabajan sobre el mismo dato al mismo tiempo y el resultado final depende del orden en que se ejecutan. En seguridad se aprovecha cuando hay una ventana de tiempo entre que la aplicación comprueba una condición y que la actualiza (lo que se conoce como TOCTOU, time-of-check to time-of-use).

El caso del código de descuento de un solo uso funciona así: cuando se canjea, el servidor primero chequea si el código sigue disponible y después lo marca como usado. Entre esos dos pasos hay un instante en el que el código todavía figura como válido. Si en ese instante llegan varias peticiones, todas pasan el chequeo antes de que la primera alcance a marcarlo como usado.

Con una herramienta como Turbo Intruder de Burp Suite, el atacante prepara la petición de canje y manda unas 50 casi al mismo tiempo. Si varias llegan dentro de esa ventana, pueden pasar el chequeo antes de que el servidor actualice la base de datos. Ahí un código de un solo uso termina aplicándose más de una vez. No basta con enviar muchas peticiones: la falla depende de cómo el servidor coordina ese chequeo y la actualización.

Para evitarlo, el chequeo y el cambio de estado tienen que ser una operación atómica. Por ejemplo, una actualización condicional dentro de una transacción puede hacer que solo un canje marque el código como usado; los demás deben fallar.
