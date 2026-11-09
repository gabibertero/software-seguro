# XSS vs. CSRF

Los dos ataques pueden terminar afectando la cuenta de un usuario, pero llegan por caminos distintos.

En un **XSS**, el atacante consigue que la aplicación muestre o procese un dato suyo como código. Ese JavaScript se ejecuta en el navegador **con el origen del sitio vulnerable**. Por ejemplo, si un comentario del foro se inserta como HTML y ejecuta código cuando alguien abre la página, eso es XSS. El script podría leer datos accesibles a esa página, cambiar lo que se muestra o hacer acciones con la sesión del usuario. Solo podría leer la cookie de sesión desde JavaScript si esa cookie no tiene `HttpOnly`.

En un **CSRF**, el atacante controla una página externa y desde allí provoca una petición al sitio donde la víctima ya inició sesión. Su código sigue en el origen de la página atacante: **no se ejecuta dentro del sitio legítimo**. El navegador puede adjuntar la cookie de sesión automáticamente y el servidor acepta la operación si no verifica la intención del usuario. Un ejemplo sería enviar desde esa página un formulario para cambiar el correo de la cuenta. Por la política de mismo origen, la página atacante normalmente no puede leer la respuesta, pero puede haber provocado el cambio.

La diferencia central es qué confianza se aprovecha: en XSS, el navegador termina tratando datos del atacante como código del sitio; en CSRF, el servidor confunde una petición que trae sesión con una acción que el usuario quiso realizar.

Para XSS usaría codificación de salida adecuada al contexto, puntos de inserción seguros en el DOM y una CSP como capa adicional. Para CSRF usaría tokens validados por el servidor, una política `SameSite` apropiada y operaciones que no modifiquen datos con `GET`.

Un XSS puede incluso facilitar acciones que una defensa CSRF frenaría desde una página externa: el script ya corre en el origen legítimo y podría leer un token anti-CSRF que esté disponible para el JavaScript de la aplicación. Por eso hay que corregir ambas fallas.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- [OWASP, Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
- [OWASP, Cross Site Scripting (XSS)](https://community.owasp.org/attacks/xss/).
