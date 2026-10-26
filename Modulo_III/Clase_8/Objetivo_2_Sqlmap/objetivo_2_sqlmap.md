# Automatización vs Explotación Manual (sqlmap)

sqlmap es la mejor herramienta para explotar inyecciones SQL y automatiza todo el trabajo pesado, pero no entiende la aplicación: manda peticiones sueltas y prueba payloads contra un parámetro. Por eso hay situaciones donde se come la vulnerabilidad de largo y un atacante a mano, manipulando la petición en Burp, sí la explota. Van dos casos concretos.

## 1. Cuando la inyección depende de una secuencia o de la lógica de negocio

Hay vulnerabilidades que solo son alcanzables después de una serie de pasos: un carrito de varios pasos, un token que te dan en una petición anterior y tenés que mandar en la siguiente, un valor que hay que calcular, o un parámetro que recién aparece cuando el flujo está en cierto estado. sqlmap dispara sus peticiones de forma aislada y no sabe reproducir ese camino: le pega al endpoint sin el contexto previo y la app lo rebota antes de llegar al punto inyectable.

A mano es otra cosa: en Burp sigo el flujo completo, veo de dónde sale cada token, lo arrastro fresco a la petición siguiente y meto el payload justo en el paso que corresponde. Como entiendo la secuencia, llego al parámetro vulnerable que sqlmap nunca alcanza.

## 2. Cuando hay un WAF o un formato de petición que a sqlmap no le sale

Los payloads por defecto de sqlmap son conocidos, así que un WAF o un filtro medianamente decente los bloquea de entrada. También pasa cuando el parámetro vulnerable está en un lugar raro o con un encoding particular: dentro de un JSON anidado, en base64, con doble URL-encode, en un header. sqlmap tiene tamper scripts para algunas de esas cosas, pero son genéricos y no cubren un filtro hecho a medida.

Manualmente puedo observar exactamente qué me bloquea y qué me deja pasar, y armar un bypass a medida: cambiar comentarios, jugar con mayúsculas y minúsculas, reemplazar espacios, partir las palabras clave, codificar justo el carácter que el filtro no revisa. Veo la respuesta de cada intento y voy ajustando, cosa que sqlmap a ciegas no hace.

Un tercer caso clásico es la inyección de segundo orden: el payload se guarda en una petición y recién se ejecuta en otra parte de la app más tarde. sqlmap prueba entrada y salida en el mismo lugar, así que no ve que el payload disparó en otro endpoint; alguien que conoce la aplicación sí sabe dónde ir a mirar.

La conclusión es que sqlmap es excelente para lo repetitivo y para confirmar rápido, pero no reemplaza entender la aplicación. Cuando la explotación depende del contexto, del flujo o de un filtro específico, el que gana es el que manipula la petición a mano sabiendo qué está haciendo.
