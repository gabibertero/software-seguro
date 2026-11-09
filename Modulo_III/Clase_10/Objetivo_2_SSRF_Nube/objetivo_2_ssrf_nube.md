# SSRF y metadatos de una instancia en AWS

En una vulnerabilidad **Server-Side Request Forgery (SSRF)**, la aplicación toma una dirección controlada por el usuario y hace una petición desde el **servidor**. El riesgo es que ese servidor puede alcanzar destinos internos que el atacante no puede consultar directamente desde Internet. Una entrada que parece ser solo una URL de imagen, por ejemplo, puede terminar haciendo que el backend consulte un recurso interno.

## ¿Qué hay en `169.254.169.254`?

Si la aplicación corre en una instancia **Amazon EC2**, `169.254.169.254` es la dirección local del *Instance Metadata Service* (IMDS). Allí se pueden consultar datos de la instancia, como su identidad y configuración, y, si tiene asociado un rol IAM, obtener sus **credenciales temporales**. Esas credenciales incluyen un identificador de clave de acceso, una clave secreta, un token de sesión y una fecha de expiración. El atacante querría recibir esos datos a través de la respuesta de la aplicación vulnerable para poder usar los permisos que tenga el rol. También podrían quedar expuestos datos de `user-data` si se colocaron secretos allí, algo que se debe evitar.

No basta con que la aplicación esté alojada «en AWS»: el escenario concreto de esta dirección corresponde al servicio de metadatos de **EC2** y a lo que esa instancia tenga configurado. Si no hay un rol IAM asociado, no habrá credenciales de ese rol que extraer. Si el rol tiene permisos amplios, el impacto potencial aumenta; si tiene los mínimos necesarios, se limita.

## Por qué un GET puede funcionar o fallar

Con **IMDSv1** habilitado, una petición `GET` del servidor a una ruta de metadatos puede recibir la información. En cambio, si la instancia **exige IMDSv2**, primero hace falta iniciar una sesión con una petición `PUT` a `/latest/api/token` y luego incluir el token obtenido en la cabecera de cada consulta. Por eso un SSRF que solo permite elegir la URL de un `GET` normalmente no alcanza para leer metadatos protegidos por IMDSv2. IMDSv2 es una defensa adicional, no una garantía universal: importa qué métodos y cabeceras puede controlar el atacante y si la aplicación o la red exponen otros recursos internos.

Para prevenir el problema de raíz, la aplicación debería restringir estrictamente los destinos que puede solicitar, validar el destino final tras redirecciones y resolución DNS, y bloquear el acceso del backend a direcciones internas y de enlace local cuando no sea necesario. En EC2 corresponde exigir IMDSv2 y asignar al rol IAM solo los permisos indispensables.

## Fuentes

- [AWS EC2, acceso a metadatos de instancia](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html).
- [AWS EC2, credenciales de seguridad en metadatos](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-metadata-security-credentials.html).
- [AWS EC2, configuración de IMDS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html).
- [OWASP, Server-Side Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html).
