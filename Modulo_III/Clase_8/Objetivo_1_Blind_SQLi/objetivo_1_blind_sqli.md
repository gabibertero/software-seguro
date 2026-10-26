# El Arte de la Inyección Ciega (Blind SQLi)

Una inyección es "ciega" cuando la aplicación no me devuelve ni el resultado de la consulta ni un error en pantalla. No veo los datos directamente, así que no puedo hacer un UNION y leer la respuesta como en una inyección clásica. En vez de eso tengo que deducir la información mirando cómo se comporta la aplicación ante condiciones que armo para que den verdadero o falso.

Hay dos formas de hacerlo, y la diferencia está en qué señal uso para saber si mi condición fue verdadera o falsa.

## Boolean-Based (basada en verdadero/falso)

Acá me apoyo en que la página cambia según si la condición que inyecto es verdadera o falsa. Si inyecto una condición que siempre es verdadera la página carga normal y muestra el contenido, y si meto una que es falsa la página sale distinta: aparece vacía, falta un producto, cambia un mensaje, algo. No me importa el texto en sí, me importa que haya dos estados distinguibles. Cada petición me devuelve un bit de información: verdadero o falso.

## Time-Based (basada en tiempo)

El problema es cuando la página sale exactamente igual pase lo que pase: siempre el mismo "Cargando..." genérico, sin diferencia entre verdadero y falso. Ahí el contenido no me sirve de señal, así que uso el tiempo. Inyecto una condición que, si es verdadera, obliga a la base de datos a esperar unos segundos antes de responder (un SLEEP). Entonces, si la respuesta tarda, mi condición era verdadera; si vuelve al toque, era falsa. Sigo sacando un bit por petición, pero ahora lo leo cronometrando la respuesta en vez de mirar la pantalla.

La diferencia técnica entre las dos es esa: la boolean lee la diferencia en el contenido o el comportamiento de la respuesta, y la time-based lee la diferencia en el tiempo que tarda. La time-based es más lenta y ruidosa, pero es el plan B para cuando la app no muestra ninguna diferencia visible.

## Sacar el nombre de la base letra por letra

Si solo puedo preguntar cosas de verdadero/falso, no puedo pedir "dame el nombre de la base" de una. Lo que hago es ir sacando cada carácter del nombre, uno a la vez, convirtiendo cada letra en una serie de preguntas sí/no.

La idea es:

1. Agarro el primer carácter del nombre con una función tipo SUBSTRING (`SUBSTRING(database(),1,1)` me da la primera letra).
2. Lo paso a su número ASCII y empiezo a comparar: "¿el ASCII de esa letra es mayor que 109?". Según la página cargue normal o no (boolean), o tarde o no (time-based), sé si la respuesta fue sí o no.
3. Con esa respuesta parto el rango al medio y vuelvo a preguntar (búsqueda binaria). Así, en unas 7 preguntas, acorralo el valor exacto de esa letra.
4. Paso al carácter 2 (`SUBSTRING(database(),2,1)`), repito, después el 3, y así hasta reconstruir todo el nombre.

En time-based la pregunta queda envuelta en algo como `IF(condición, SLEEP(5), 0)`: si es verdadera duerme 5 segundos y si no responde al instante. Es tedioso y por eso normalmente se automatiza con un script, pero el concepto es ese: convertir un dato que no puedo ver en un montón de preguntas de sí/no que la aplicación me contesta sin querer, con su comportamiento o con su demora.
