# ¿Qué pasa antes de que el navegador mande una petición HTTPS?

Antes de enviar el primer pedido HTTP, el navegador y el servidor hacen el **TLS Handshake**. Se ponen de acuerdo sobre la versión y los algoritmos que van a usar, el navegador verifica la identidad del servidor y ambos obtienen las claves para cifrar la conexión. Por eso, cuando veo HTTPS, el pedido y la respuesta viajan protegidos entre el navegador y el servidor.

El certificado digital le permite al navegador comprobar la identidad del servidor: incluye su clave pública y está firmado por una Autoridad Certificadora (CA). El navegador revisa que sea válido, que corresponda al dominio al que entré y que la CA sea de confianza. Si algo de eso falla, me muestra una advertencia porque ya no puede asegurar que esté hablando con el servidor correcto.

Se usan mecanismos **asimétricos** durante el handshake para autenticar al servidor y acordar un secreto compartido. En TLS 1.3, por ejemplo, el acuerdo de claves usa un intercambio efímero: la clave simétrica de la sesión no se manda directamente por la red. A partir del secreto, ambos lados derivan las claves con las que cifran los datos. Para el tráfico de la sesión usan **cifrado simétrico**, que es mucho más rápido cuando hay que proteger cada pedido y cada respuesta.

En pocas palabras: el certificado me ayuda a saber con quién me conecté; el intercambio de claves permite que ambos lados lleguen al mismo secreto; y las claves simétricas protegen la comunicación que sigue.

## Fuentes

- [RFC 8446, TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446.html).
- [MDN, Transport Layer Security](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security).

