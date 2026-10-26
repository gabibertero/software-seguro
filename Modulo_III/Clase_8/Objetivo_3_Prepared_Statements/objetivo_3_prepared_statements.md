# La Única Mitigación Definitiva (Prepared Statements)

Muchos intentan tapar las inyecciones con listas negras (bloquear palabras como SELECT o UNION) o escapando las comillas. Eso no alcanza: las listas negras se evaden con comentarios, cambiando mayúsculas, con sintaxis alternativa o codificando los caracteres, y el escape de comillas se cae cuando el input va en un contexto numérico o con otra codificación. Son parches sobre el síntoma. La solución de fondo es la consulta parametrizada.

## Cómo funciona por dentro

La clave está en que la consulta viaja a la base en dos momentos separados, por dos canales distintos.

Primero mando a la base solo la plantilla de la consulta, con huecos donde van los datos:

```
SELECT * FROM usuarios WHERE user = ? AND pass = ?
```

La base recibe eso y lo compila: arma el plan de la consulta y decide la estructura (que es un SELECT, de qué tabla, con qué condiciones) antes de que exista ningún dato del usuario. En ese momento los `?` son nada más que marcadores de posición.

Recién después mando los parámetros por separado, como datos puros, y la base los mete en el plan que ya tenía compilado.

## Por qué neutraliza cualquier inyección

Como la estructura de la consulta queda fijada antes de que llegue el dato, lo que mande el usuario ya no se interpreta como código SQL: entra como un valor literal para ese parámetro y nada más. Si en el campo de usuario escribo `' OR 1=1--`, con prepared statements todo ese texto se trata tal cual como "el usuario que buscás es la cadena ' OR 1=1--", y como ningún usuario se llama así, no encuentra nada. El motor nunca lo lee como una condición, porque el momento de interpretar la estructura ya pasó.

Esa es la diferencia con escapar o filtrar: la inyección existe porque el dato del usuario logra cambiar la estructura de la consulta. Con prepared statements el dato y el código van por canales distintos y la estructura ya está armada cuando el dato aparece, así que no hay forma de que un carácter "se escape" y pase a ser código. No importa qué mande el usuario: siempre es dato.

Un detalle: esto aplica a los valores. Los nombres de tabla o de columna no se pueden parametrizar, así que si una de esas partes es dinámica hay que validarla aparte contra una lista blanca. Pero para los datos del usuario, que es de donde salen casi todas las inyecciones, la consulta parametrizada las corta por completo.
