# El arte de la evasión de filtros XSS

Eliminar solamente las cadenas `<script>` y `</script>` no elimina la capacidad de ejecutar JavaScript. El navegador también ejecuta código en atributos de eventos de otros elementos HTML. Si la aplicación inserta un comentario como HTML sin codificarlo, el atacante puede crear un elemento con uno de esos atributos y el filtro de etiquetas `script` no lo tocará. Tampoco alcanza con buscar una lista fija de palabras: el resultado depende del contexto donde aparece el dato y de cómo lo interpreta el navegador.

Dos ejemplos distintos, suponiendo que el comentario se interpreta como HTML y que la política de seguridad de la página permite manejadores de eventos en línea:

1. **Error de carga de imagen:** `<img src=x onerror=alert(1)>`. La ruta `x` no apunta a una imagen válida; al fallar la carga se dispara `onerror` y se ejecuta `alert(1)`.
2. **Carga de SVG:** `<svg onload=alert(1)>`. Al cargarse el elemento SVG se dispara `onload`, que ejecuta la misma demostración sin usar una etiqueta `script`.

`alert(1)` es solo una prueba visible e inocua de ejecución. La viabilidad concreta de cada ejemplo depende de la ubicación exacta del comentario en el HTML, del filtrado adicional y de la CSP. Si el comentario se muestra como texto mediante `textContent` o se codifica para el contexto de salida, esos caracteres no se convierten en elementos ejecutables.

La defensa es tratar los comentarios como datos: usar una API que inserte texto o aplicar codificación de salida adecuada al contexto HTML. Si el producto *necesita* permitir HTML enriquecido, debe sanitizarlo con una biblioteca mantenida, además de aplicar una CSP estricta como segunda capa. Una lista negra que solo borra etiquetas concretas no resuelve el problema.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html): codificación según contexto, sinks seguros y límites de los filtros.
- [PortSwigger Web Security Academy, Cross-site scripting contexts](https://portswigger.net/web-security/cross-site-scripting/contexts): ejecución mediante elementos y eventos distintos de `script`.
- [OWASP, Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html): CSP como defensa adicional.
