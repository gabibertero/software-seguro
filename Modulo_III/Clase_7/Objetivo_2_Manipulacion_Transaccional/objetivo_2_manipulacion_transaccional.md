# Análisis de Manipulación Transaccional (Cantidades Negativas)

En un e-commerce deja elegir la cantidad de un producto y calcula el total multiplicando esa cantidad por el precio. Al interceptar la petición y cambiar la cantidad de 1 a -5, el servidor hace la cuenta igual: precio por -5 da un número negativo, y ese negativo se resta del total en vez de sumarse. Si el sistema interpreta un total negativo como saldo a favor, termina acreditando dinero en la cuenta del atacante en lugar de cobrarle.

Esto ocurre porque la aplicación confía en un dato que viene del cliente sin validarlo. La cantidad se puede modificar en la petición y el backend la usa directamente en el cálculo sin chequear que tenga sentido.

Para evitarlo, el desarrollador tiene que validar el campo del lado del servidor, no solo en el frontend:

* Aceptar únicamente números enteros positivos (mayores a 0).
* Rechazar la petición si la cantidad es negativa, cero o no numérica.
* Fijar un máximo razonable por producto para que no se abuse con valores enormes.
* Recalcular el total siempre en el servidor con esos valores validados, nunca confiar en un total que mande el cliente.

La idea general es que todo lo que venga del usuario se considere no confiable y se valide antes de usarlo en una operación.
