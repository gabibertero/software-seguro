# Manejo Inseguro de Errores (Stack Traces)

Si una aplicación falla, al usuario le alcanza con saber que no pudo completar la operación y, si corresponde, recibir un número de incidente para consultar a soporte. Eso es un **mensaje de error genérico**. Un **stack trace**, en cambio, muestra la cadena de llamadas que terminó en el error. Puede incluir la excepción, clases, métodos, rutas de archivos y líneas de código.

Mostrar esa traza en producción le ahorra trabajo a un atacante. Puede revelar el lenguaje, el framework, bibliotecas, versiones y el punto exacto donde falló algo. Con esas pistas puede buscar fallas conocidas y probar entradas más precisas. Ver un stack trace no prueba por sí solo que exista una vulnerabilidad explotable, pero sí facilita el reconocimiento.

La solución es mostrar una respuesta breve al usuario y guardar el detalle técnico en registros internos con acceso restringido. También hay que manejar las excepciones de manera centralizada, desactivar el modo de depuración en producción y usar un identificador de incidente que conecte la respuesta con el registro. Esos registros tampoco deberían guardar contraseñas, tokens ni otros datos sensibles.
