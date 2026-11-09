# Evasión de un filtro que borra `<script>`

Si el filtro solo elimina `<script>` y `</script>`, todavía deja pasar otras formas de ejecutar JavaScript. Un navegador también puede ejecutar código desde los atributos de eventos de una etiqueta HTML. Por eso, si un comentario se inserta en la página como HTML, borrar esas dos cadenas no alcanza.

Dos ejemplos que no usan `<script>`:

1. `<img src=x onerror=alert(1)>`: el navegador intenta cargar la imagen desde `x`. Si falla, se dispara `onerror` y aparece el `alert`.
2. `<svg onload=alert(1)>`: al cargarse el elemento SVG se dispara `onload`, que también ejecuta el `alert`.

Son ejemplos para explicar la falla, no pruebas hechas contra una aplicación concreta. Funcionan si el dato llega a un contexto HTML que interpreta esas etiquetas y si la CSP de la página no bloquea los manejadores de eventos en línea. Según dónde se inserte el comentario, el payload tendría que adaptarse.

El problema de fondo es tratar un comentario del usuario como si fuera código HTML confiable. Para mostrar texto, usaría una API como `textContent` o codificaría la salida según el contexto. Si la aplicación necesita aceptar HTML, hay que sanitizarlo con una biblioteca mantenida. Una CSP estricta suma protección, pero no reemplaza esa corrección.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- [PortSwigger, Cross-site scripting contexts](https://portswigger.net/web-security/cross-site-scripting/contexts).
- [OWASP, Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html).
