# Anatomía del DOM XSS

En un DOM XSS, el JavaScript de la página lee un dato que puede controlar un atacante y lo inserta en un lugar que interpreta HTML o código. Por ejemplo, una página puede leer `location.hash` para mostrar un mensaje y después asignarlo a `innerHTML`.

El detalle importante está en el `#` de la URL. En `https://ejemplo.test/perfil#mensaje`, la parte `#mensaje` queda en el navegador: **no viaja en la petición HTTP al servidor**. Si alguien comparte un enlace con un fragmento malicioso y el código de la página hace esto, aparece la falla:

```js
const mensaje = decodeURIComponent(location.hash.slice(1));
document.querySelector("#resultado").innerHTML = mensaje; // Interpreta HTML
```

Si el fragmento contiene `<img src=x onerror=alert(1)>`, la imagen falla y el navegador puede ejecutar `onerror`. Esto depende de que no haya otra protección eficaz, como una CSP que bloquee ese evento.

Un WAF o IPS que revisa las peticiones que llegan al servidor no ve el fragmento `#`, así que puede pasar por alto este caso. Eso no vuelve invisible a todo DOM XSS: una herramienta que analiza el JavaScript o prueba la página en un navegador sí puede encontrarlo.

Si el mensaje debe mostrarse como texto, cambiaría el punto de inserción:

```js
const mensaje = decodeURIComponent(location.hash.slice(1));
document.querySelector("#resultado").textContent = mensaje; // Lo muestra como texto
```

Si la página necesita mostrar HTML aportado por usuarios, habría que sanitizarlo antes de usar un punto de inserción que interprete HTML. "DOM-based" describe dónde ocurre el procesamiento inseguro; un dato que llega por enlace o queda guardado también puede terminar en ese mismo tipo de falla.

## Fuentes

- [MDN, URI fragment](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Fragment).
- [OWASP, DOM based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html).
- [PortSwigger, DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based).
