# Automatización y Fuerza Bruta

En un formulario de login se pueden automatizar intentos para probar contraseñas hasta encontrar la correcta. La diferencia principal entre la fuerza bruta tradicional y el ataque de diccionario está en cómo se eligen las contraseñas que se prueban.

Fuerza bruta tradicional: prueba combinaciones posibles de manera sistemática hasta adivinarla. No depende necesariamente del azar si no que intenta recorrer un espacio de combinaciones posibles. Puede requerir muchos intentos, especialmente si la contraseña es larga y poco predecible.

Ataque de diccionario: prueba una lista preparada de contraseñas probables, como palabras comunes y variantes típicas. No siempre implica investigar a una persona en particular, se basa en que mucha gente elige claves predecibles. Si la contraseña está entre esas opciones, suele encontrarla con menos intentos que la fuerza bruta exhaustiva, aunque no está garantizado.

En resumen, la fuerza bruta recorre combinaciones posibles, mientras que el diccionario prioriza contraseñas que parecen más probables.

Del lado del desarrollador, lo más directo para frenar estas automatizaciones es el rate limiting: limitar la cantidad de intentos de login por usuario o por IP en una ventana de tiempo y bloquear o pedir un paso extra cuando se pasa de ese límite. Por ejemplo, después de 5 intentos fallidos se bloquea la cuenta unos minutos o se pide un CAPTCHA, asique probar miles de contraseñas deja de ser viable. Se puede reforzar con doble factor (2FA), osea que aunque adivinen la clave no les alcanza para entrar.
