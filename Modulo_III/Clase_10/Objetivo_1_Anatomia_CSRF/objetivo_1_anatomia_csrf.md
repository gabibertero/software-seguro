# Anatomía de un ataque CSRF

Un **Cross-Site Request Forgery (CSRF)** sucede cuando una página controlada por un atacante consigue que el navegador de una persona autenticada envíe una petición no deseada a otra aplicación. La página maliciosa no necesita conocer la contraseña ni leer la respuesta: aprovecha que el navegador puede adjuntar las cookies de sesión al solicitar el sitio de destino. Si el servidor interpreta la cookie como autorización suficiente para cambiar datos, ejecuta la acción como si la hubiera pedido la víctima.

## Flujo de un ataque

1. La víctima inicia sesión en `cuenta.ejemplo` y recibe una cookie de sesión.
2. Sin cerrar la sesión, visita `atacante.ejemplo`. Esa página induce una petición hacia una operación de `cuenta.ejemplo`, por ejemplo, un formulario de cambio de correo. El atacante elige los parámetros de la operación.
3. Si la política de cookies permite enviarlas en esa petición entre sitios, el navegador adjunta automáticamente la cookie correspondiente a `cuenta.ejemplo`. La página atacante no necesita leer su valor.
4. El servidor reconoce la sesión, pero, si no comprueba la intención del usuario mediante una defensa CSRF, acepta los parámetros elegidos por el atacante y cambia el correo.

El punto de fallo está en confundir **una petición autenticada** con **una petición autorizada por el usuario**. La política de mismo origen puede impedir que la página atacante lea la respuesta, pero eso no impide necesariamente que provoque el envío de un formulario. Por eso usar `POST` o HTTPS por sí solos no resuelve el problema.

## Qué cambia con SameSite

El atributo `SameSite` de la cookie le indica al navegador cuándo adjuntarla a peticiones originadas desde otro *sitio*:

| Valor | Comportamiento relevante para CSRF |
|---|---|
| `SameSite=Strict` | No envía la cookie en peticiones iniciadas desde otro sitio, incluso durante la navegación inicial hacia la aplicación. Es la opción más restrictiva, aunque puede afectar flujos legítimos desde enlaces externos. |
| `SameSite=Lax` | La envía en peticiones del mismo sitio y en navegaciones principales hacia el sitio de destino que usan un método seguro, como `GET`. No la envía en un `POST` entre sitios normal ni en recursos embebidos como imágenes. |

`Lax` frena muchos CSRF basados en formularios `POST`, pero una operación que cambia estado mediante `GET` todavía puede ser vulnerable: una navegación principal `GET` sí puede llevar la cookie. Además, el comportamiento por defecto cuando falta el atributo puede ser más permisivo que declarar `Lax` explícitamente en algunos navegadores. `SameSite` trabaja por *sitio*, no equivale a una separación estricta entre todos los subdominios.

La defensa completa consiste en no modificar estado con métodos seguros como `GET`, usar la protección CSRF del framework o tokens impredecibles vinculados a la sesión y **validarlos en el servidor** para cada petición que cambie estado. `SameSite=Lax` o `Strict` agrega una capa de defensa según lo permita el flujo de la aplicación; no reemplaza la validación del token.

## Fuentes

- [OWASP, Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
- [MDN, atributo SameSite de Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value).
