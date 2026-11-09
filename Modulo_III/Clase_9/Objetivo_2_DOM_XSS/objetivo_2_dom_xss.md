# Anatomía del DOM XSS

En un DOM XSS, el JavaScript de la propia página toma un dato controlado por un tercero —por ejemplo, `location.hash`— y lo coloca en un punto del DOM que interpreta HTML o código. La falla está en ese procesamiento del lado del cliente. "DOM-based" describe dónde ocurre la interpretación insegura; "reflejado" y "almacenado" describen cómo llega el dato a la víctima, por lo que las categorías pueden combinarse.

En una URL como `https://ejemplo.test/perfil#mensaje`, el fragmento `#mensaje` se procesa en el navegador y **no se envía al servidor en la petición HTTP**. Un atacante puede compartir un enlace con un fragmento preparado. Si la aplicación lee `location.hash` y lo inserta mediante `innerHTML`, el navegador interpretará sus etiquetas y atributos. Por ejemplo:

```js
const mensaje = decodeURIComponent(location.hash.slice(1));
document.querySelector("#resultado").innerHTML = mensaje; // Vulnerable
```

Con un fragmento que, al decodificarse, contenga `<img src=x onerror=alert(1)>`, el atributo `onerror` puede ejecutarse cuando falle la carga de la imagen. Esto supone que no hay otra defensa eficaz, como una CSP que bloquee ese manejador de eventos.

Un WAF o IPS que inspecciona **solo las peticiones que llegan al servidor** no ve el fragmento en esa petición; por eso puede pasar por alto este vector. No significa que todo DOM XSS sea imposible de detectar: las herramientas que analizan el código o el comportamiento del navegador sí pueden descubrir la fuente, el sink y la ejecución.

Si el valor debe mostrarse como texto, el cambio central es usar un sink que no interprete HTML:

```js
const mensaje = decodeURIComponent(location.hash.slice(1));
document.querySelector("#resultado").textContent = mensaje; // Texto, no HTML
```

Si hace falta contenido HTML enriquecido, hay que sanitizarlo con una biblioteca adecuada antes de insertarlo y evitar que los datos no confiables lleguen a sinks como `innerHTML` sin control. También se puede agregar una CSP estricta como capa complementaria.

## Fuentes

- [MDN, URI fragment](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Fragment): el fragmento no viaja en la petición al servidor.
- [OWASP, DOM based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html): fuentes, sinks inseguros y alternativas de texto.
- [PortSwigger Web Security Academy, DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based): flujo de datos del navegador y límites de la inspección del servidor.
