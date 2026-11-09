# Output Encoding y CSP contra XSS

Para proteger una página de XSS usaría dos capas. La primera es **codificar la salida según el lugar donde se muestra el dato**. Si un comentario contiene `<img ...>` y se va a mostrar como texto HTML, el `<` debe representarse como `&lt;`. Así el navegador muestra los caracteres en pantalla sin crear una etiqueta. En un atributo HTML hay que tratar también las comillas; para una URL, una cadena JavaScript o CSS se aplican reglas distintas. Un único "escape" para toda la aplicación puede fallar.

Cuando el dato se coloca desde JavaScript, lo más simple para mostrar texto es usar `element.textContent = dato` en vez de `element.innerHTML = dato`. Evitaría poner datos del usuario dentro de manejadores de eventos o código JavaScript. Si el producto permite escribir HTML enriquecido, hace falta sanitizar las etiquetas y atributos permitidos con una biblioteca mantenida.

La segunda capa es una **Content Security Policy (CSP)**, enviada en la cabecera HTTP `Content-Security-Policy`. Sirve para limitar qué scripts puede ejecutar el navegador. Por ejemplo, el servidor puede generar un *nonce* nuevo y aleatorio para cada respuesta y ponerlo en sus scripts autorizados. Una política ilustrativa sería:

```http
Content-Security-Policy: default-src 'self'; script-src 'nonce-VALOR_ALEATORIO_POR_RESPUESTA'; object-src 'none'; base-uri 'none'
```

`VALOR_ALEATORIO_POR_RESPUESTA` es un marcador: no se debe copiar como valor fijo. El servidor tiene que generar el nonce para cada respuesta e incluirlo en los scripts legítimos. Una política de este tipo puede bloquear manejadores de eventos en línea que no estén autorizados. Hay que probarla con los recursos reales de la página y evitar relajar `script-src` con `unsafe-inline`.

La codificación evita que el dato se interprete como código en el contexto correcto. La CSP puede frenar la ejecución si queda algún punto vulnerable. **CSP sola no arregla el XSS**, y codificar con la regla equivocada tampoco alcanza. Por eso revisaría los lugares donde se insertan datos y usaría ambas capas.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- [OWASP, Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html).
- [MDN, CSP implementation](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CSP).
