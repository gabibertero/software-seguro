# El Punto Ciego de los Escáneres Automatizados

Las herramientas automáticas como Nessus, Acunetix o el escáner activo de Burp Suite son muy buenas encontrando fallas técnicas que siguen un patrón conocido: un XSS, una inyección SQL, una cabecera mal configurada, una versión vulnerable. Trabajan mandando payloads y comparando la respuesta contra firmas o comportamientos esperados.

El problema es que la lógica de negocio no tiene un patrón universal: es propia de cada aplicación. Un escáner no sabe cuánto debería costar un producto, que un usuario no tendría que poder aprobar su propio gasto, o que un descuento es de un solo uso. No entiende las reglas ni la intención del sistema, solo prueba entradas y mira salidas. Por eso una falla como cargar una cantidad negativa, saltarse un paso de un flujo de compra o acceder al recurso de otro usuario (IDOR) le pasa desapercibida: técnicamente la petición es válida, lo que está mal es que rompe una regla del negocio.

Por eso la auditoría manual es indispensable en esta etapa. El pentester entiende qué hace la aplicación y qué no debería permitir, y prueba a propósito esos abusos de las reglas, algo que una herramienta automática no puede deducir sola.
