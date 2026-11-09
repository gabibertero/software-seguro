# El Punto Ciego de los Escáneres Automatizados

Las herramientas automáticas como Nessus, Acunetix o el escáner activo de Burp Suite ayudan a encontrar fallas que siguen un patrón conocido: un XSS, una inyección SQL, una cabecera mal configurada o una versión vulnerable. Prueban entradas y comparan las respuestas con señales que saben reconocer.

La lógica de negocio es distinta en cada aplicación. El escáner no sabe por sí solo cuánto debería costar un producto, que un usuario no debe aprobar su propio gasto o que un descuento es de un solo uso. Puede que una petición con cantidad negativa reciba una respuesta perfectamente válida desde el punto de vista HTTP, aunque rompa la regla de la compra. Lo mismo pasa si se puede saltar un paso del flujo o consultar un recurso de otro usuario. Sin conocer el funcionamiento esperado, la herramienta puede pasar por alto esos casos.

Por eso necesito revisar el flujo manualmente: entender qué debería permitir la aplicación y probar qué pasa cuando cambio el orden de los pasos, los valores o el usuario. Después puedo automatizar pruebas concretas, pero primero tengo que identificar la regla que quiero comprobar.
