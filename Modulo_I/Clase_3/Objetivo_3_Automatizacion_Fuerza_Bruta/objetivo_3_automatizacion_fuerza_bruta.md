# Automatización y Fuerza Bruta

En un formulario de login se pueden automatizar intentos para probar contraseñas hasta encontrar la correcta. La diferencia principal entre la fuerza bruta tradicional y el ataque de diccionario está en cómo se eligen las contraseñas que se prueban.

Fuerza bruta tradicional: prueba combinaciones de manera sistemática hasta dar con la contraseña. Puede requerir muchos intentos, especialmente si es larga y poco predecible.

Ataque de diccionario: prueba una lista preparada de contraseñas probables, como palabras comunes y variantes típicas. No siempre implica investigar a una persona en particular, se basa en que mucha gente elige claves predecibles. Si la contraseña está entre esas opciones, suele encontrarla con menos intentos que la fuerza bruta exhaustiva, aunque no está garantizado.

En resumen, la fuerza bruta recorre combinaciones posibles, mientras que el diccionario prioriza contraseñas que parecen más probables.

Del lado del desarrollador, lo más directo para frenar estos intentos es limitar cuántas veces se puede probar el login por usuario o por IP en un período (*rate limiting*). Por ejemplo, después de cinco intentos fallidos se puede pedir un paso extra o esperar unos minutos. También sirve sumar doble factor (2FA): aunque alguien adivine la contraseña, todavía le faltaría ese segundo paso para entrar.
