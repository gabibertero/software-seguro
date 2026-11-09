# Análisis del Ataque de Algoritmo "None"

Un JWT tiene tres partes: header, payload y firma. En el header se indica el algoritmo, y con la firma el servidor comprueba que nadie modificó el token y que fue emitido por quien corresponde.

El problema aparece si el backend acepta el valor none indicado por el propio token. Ese valor significa que no hay firma. Si el servidor lo admite en una aplicación que esperaba tokens firmados, deja de comprobar la integridad del JWT.

Un atacante lo aprovecha así:

1. Toma un token válido y lee su contenido.
2. Cambia el payload, por ejemplo, para atribuirse el rol de administrador.
3. Cambia el algoritmo indicado en el header a none.
4. Envía el token con la firma vacía.

Si el servidor confía en esa indicación sin imponer su propia política, acepta el payload cambiado sin verificar una firma. Así se puede falsificar la identidad o el rol sin conocer la clave secreta del servidor: en ese recorrido, la clave ni se usa.

El fallo de fondo es dejar que el cliente decida cómo se valida su token. El servidor tiene que fijar de antemano los algoritmos permitidos para esa aplicación, verificar la firma y rechazar los JWT sin firma cuando se espera autenticación con tokens firmados.
