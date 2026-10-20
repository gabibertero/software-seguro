# Análisis de la Tríada CIA

La confidencialidad significa que la información solo puede ser vista por los usuarios que tienen permiso para acceder a ella.

La integridad significa que la información no debe modificarse sin autorización y debe mantenerse correcta.

La disponibilidad significa que el sistema y su información deben estar accesibles cuando se los necesite.

Un ejemplo que puede afectar estos tres pilares es una vulnerabilidad IDOR. Esta ocurre cuando una aplicación no comprueba correctamente si el usuario tiene permiso para acceder al recurso solicitado. Por ejemplo, un atacante cambia el identificador de un registro en una petición y la aplicación le muestra información de otra persona. Esto afecta la confidencialidad. Si además puede modificar ese registro, afecta la integridad; y si puede borrarlo, la persona autorizada deja de tener acceso a esa información, afectando su disponibilidad.

Para verlo separado, un ejemplo por cada pilar:

- Confidencialidad: un IDOR que te deja ver datos de otro usuario cambiando el id.
- Integridad: un CSRF que hace una acción en nombre de la víctima sin que ella quiera, modificando datos de su cuenta.
- Disponibilidad: un ataque de denegación de servicio (DoS) que satura el servidor y deja la app caída para los usuarios legítimos.
