# XSS vs. CSRF

Los dos ataques pueden afectar la cuenta de una persona que usa una aplicación web, pero explotan confianzas distintas. En **XSS**, una entrada no confiable termina ejecutándose como JavaScript dentro del sitio legítimo. En **CSRF**, una página externa induce al navegador de una persona autenticada a enviar una petición al sitio legítimo y el backend la acepta sin comprobar la intención de esa persona.

| Aspecto | XSS (Cross-Site Scripting) | CSRF (Cross-Site Request Forgery) |
|---|---|---|
| Punto de entrada | Datos controlados por el atacante insertados de forma insegura en HTML o procesados por JavaScript de la aplicación. | Una petición inducida desde otro sitio, por ejemplo, un formulario con parámetros elegidos por el atacante. |
| Dónde actúa | El código malicioso se ejecuta en el navegador **bajo el origen de la aplicación vulnerable**. | El navegador envía una petición al servidor legítimo; el código de la página atacante sigue bajo **su propio origen**. |
| Confianza abusada | El navegador trata como código de la aplicación contenido que debería ser solo dato. | El servidor toma la sesión enviada automáticamente con la petición como prueba suficiente de intención. |
| Qué puede conseguir | Leer o alterar contenido accesible a la página, ejecutar acciones como la víctima y, si la cookie no es `HttpOnly`, leerla. | Realizar una acción que el usuario autenticado podría hacer, si la operación carece de protección; normalmente no puede leer la respuesta por la política de mismo origen. |
| Defensas principales | Codificación de salida según el contexto, APIs seguras para el DOM y una CSP como defensa adicional. | Token anti-CSRF validado por el servidor, `SameSite` según el caso y no cambiar estado con `GET`. |

**Ejemplo directo:** si el comentario de un foro se muestra sin tratamiento y ejecuta JavaScript al abrirlo, es XSS. Si una página ajena consigue que mi navegador envíe un cambio de correo a un sitio donde sigo autenticado, sin ejecutar código dentro de ese sitio, es CSRF.

La relación entre ambos importa: un XSS dentro de la aplicación puede permitir que el atacante lea un token anti-CSRF accesible al JavaScript de esa aplicación y emita peticiones desde el origen legítimo. Por eso protegerse de CSRF no corrige un XSS y hay que abordar cada falla en su propio límite de confianza.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).
- [OWASP, Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html).
- [OWASP, Cross Site Scripting (XSS)](https://community.owasp.org/attacks/xss/).
