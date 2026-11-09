# Cómo funciona un CSRF

Un CSRF ocurre cuando una página ajena consigue que el navegador de alguien que ya inició sesión mande una petición al sitio legítimo. El atacante no necesita saber la contraseña ni leer la cookie: el navegador puede adjuntarla automáticamente al pedir una URL de ese sitio.

Por ejemplo, para cambiar el correo de una cuenta:

1. La persona entra a `cuenta.ejemplo` y queda autenticada con una cookie de sesión.
2. Después visita `atacante.ejemplo`. Esa página prepara un formulario que apunta al cambio de correo de `cuenta.ejemplo`, con una dirección elegida por el atacante.
3. Si las reglas de la cookie permiten enviarla en esa petición entre sitios, el navegador la adjunta aunque la petición haya salido de la página atacante.
4. El servidor ve una sesión válida. Si procesa el cambio sin comprobar que la persona lo pidió, modifica el correo.

La falla está en dar por hecho que **tener la cookie implica querer hacer esa acción**. La política de mismo origen normalmente impide que la página atacante lea la respuesta, pero no siempre impide enviar un formulario. Usar HTTPS o hacer el cambio con `POST` no verifica por sí solo la intención del usuario.

La bandera `SameSite` limita cuándo viaja la cookie:

- **`Strict`**: no la envía en peticiones iniciadas desde otro sitio, ni siquiera en la navegación inicial desde un enlace externo. Protege más, pero puede interferir con algunos flujos normales.
- **`Lax`**: la envía en el mismo sitio y en una navegación principal entre sitios con un método seguro, como `GET`. Un formulario `POST` entre sitios normalmente queda sin esa cookie, pero una acción que cambia datos mediante `GET` todavía puede ser atacable.

Además, `SameSite` distingue sitios, no todos los orígenes o subdominios. Cuando no se declara el atributo, algunos navegadores aplican un valor por defecto con excepciones; conviene configurarlo explícitamente.

Para proteger una acción que modifica datos, usaría la defensa CSRF del framework o un token impredecible asociado a la sesión, validado en el servidor. También evitaría modificar datos con `GET`. `SameSite=Lax` o `Strict` suma una capa, pero no reemplaza esa validación.

## Fuentes

- [OWASP, Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
- [MDN, atributo SameSite de Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value).
