# Análisis del Ataque de Algoritmo "None"

Un JWT tiene tres partes: header, payload y firma. El header indica con qué algoritmo se firmó el token y la firma es lo que le permite al servidor verificar que el token no fue modificado y que lo emitió él.

El problema aparece cuando el backend acepta el valor None en el header. Ese valor indica que el token no tiene firma, y está pensado para casos donde la integridad se garantiza por otro medio. Si el servidor lo respeta, deja de validar la firma por completo.

Un atacante lo aprovecha así:

1. Toma un token válido y lo decodifica
2. Modifica el payload con lo que quiera, por ejemplo cambia su usuario o se pone `rol admin`.
3. Cambia el header para que diga `none.`
4. Manda el token con la firma vacía.

Como el servidor confía en el algoritmo que le indica el propio token, al ver none no verifica nada y acepta el payload modificado. Así el atacante se puede hacer pasar por cualquier usuario sin conocer la clave secreta del servidor, porque esa clave nunca llega a usarse.

El fallo de fondo es dejar que el cliente decida cómo se valida el token. La mitigación es que el servidor fije de antemano qué algoritmo acepta y rechace cualquier token que no venga firmado con ese algoritmo, descartando siempre none.
