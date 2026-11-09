# El cifrado en el frontend no impide manipular una petición

El body puede viajar cifrado y, aun así, el usuario que controla su navegador puede cambiar los valores antes de que se cifren. Ese es el error de diseño: el backend toma como confiable algo que preparó el cliente. HTTPS ya protege el trayecto entre navegador y servidor; agregar otra capa de cifrado en JavaScript no convierte al navegador en una fuente confiable.

Yo lo probaría así en una auditoría autorizada:

1. Revisaría el JavaScript y el tráfico para ubicar dónde se construye el body y en qué punto se cifra.
2. Pondría un breakpoint antes de esa función para ver los valores en claro y cambiarlos. También podría modificar el formulario o llamar a la misma función con otros valores desde la consola.
3. Dejaría que el código legítimo cifre el body modificado y enviaría la petición. En Burp vería el resultado cifrado y podría repetir la solicitud, aunque no necesite descifrarlo en el proxy.
4. Compararía la respuesta para comprobar si el servidor valida de nuevo los valores, permisos y reglas del negocio. Si acepta una operación indebida, ahí está la vulnerabilidad.

No siempre hay una clave secreta dentro del JavaScript: el cliente podría cifrar con una clave pública y solo el servidor tener la privada. Tampoco tendría por qué poder descifrar en el navegador toda respuesta. Nada de eso impide que quien usa ese navegador cambie el dato **antes** de cifrarlo. La protección real para esta falla es validar y autorizar cada operación en el backend, sin confiar en que el frontend envió valores correctos.

## Fuentes

- [OWASP, Web Frontend Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Web_Frontend_Security_Cheat_Sheet.html): reglas de negocio en el servidor y límites de la criptografía en el navegador.
- [PortSwigger, Bypassing client-side controls](https://portswigger.net/burp/documentation/desktop/testing-workflow/vulnerabilities/input-validation/client-side-controls): control del cliente y manipulación de peticiones.
