# Defensa en profundidad contra XSS: Output Encoding y CSP

La primera capa es **codificar la salida según el contexto donde se inserta el dato no confiable**. El objetivo es que el navegador lo muestre como dato, sin interpretarlo como HTML o JavaScript. Por ejemplo, en el texto de un elemento HTML, el carácter `<` se representa como `&lt;`: un comentario que diga `<img ...>` se ve en pantalla, pero no crea un elemento. En un atributo HTML entre comillas hay que codificar también las comillas y otros caracteres especiales. Una URL, una cadena de JavaScript o un valor CSS requieren reglas distintas; no existe una única función de "escape" que sirva para todos los contextos.

En el DOM, cuando solo quiero mostrar texto, prefiero `element.textContent = dato` en lugar de `element.innerHTML = dato`. Evito insertar datos no confiables en manejadores de eventos (`onclick`), URLs con esquemas peligrosos o fragmentos de código JavaScript. Si el usuario debe escribir HTML enriquecido, además de elegir el contexto correcto tengo que sanitizar las etiquetas y atributos permitidos con una biblioteca mantenida.

La segunda capa es una **Content Security Policy (CSP)** enviada en la cabecera HTTP `Content-Security-Policy`. Una política estricta puede limitar qué scripts ejecuta el navegador. Por ejemplo, `script-src` basado en un *nonce* aleatorio generado por el servidor para cada respuesta permite los scripts autorizados que llevan ese nonce y bloquea, entre otras cosas, los manejadores de eventos en línea que no están autorizados. Directivas como `object-src 'none'` y `base-uri 'none'` reducen otras vías de abuso. Una política ilustrativa sería:

```http
Content-Security-Policy: default-src 'self'; script-src 'nonce-VALOR_ALEATORIO_POR_RESPUESTA'; object-src 'none'; base-uri 'none'
```

`VALOR_ALEATORIO_POR_RESPUESTA` es un marcador, no un valor fijo para copiar: el servidor debe generar uno impredecible para cada respuesta e incluir el mismo nonce en los scripts legítimos. La política final debe ajustarse y probarse según los recursos reales de la aplicación, sin relajarla con `unsafe-inline` para scripts.

Las capas se complementan: la codificación evita que el dato se convierta en código en el contexto correcto; CSP dificulta la ejecución si aparece un punto de inyección. **CSP no elimina el bug ni garantiza una mitigación definitiva por sí sola**, y una codificación equivocada de contexto también puede fallar. Hay que corregir los sinks inseguros, probar los distintos contextos de salida y mantener ambas defensas.

## Fuentes

- [OWASP, Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html): reglas de codificación por contexto, sinks seguros y sanitización.
- [OWASP, Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html): políticas estrictas, nonces y defensa en profundidad.
- [MDN, CSP implementation](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CSP): ejemplo de CSP con nonce y límites de la política.
