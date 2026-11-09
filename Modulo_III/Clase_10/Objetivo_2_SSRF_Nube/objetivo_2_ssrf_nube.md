# SSRF y los metadatos de AWS

En un SSRF, una aplicación recibe una URL del usuario y hace la petición desde su propio servidor. Si no controla el destino, alguien puede usar esa función para pedir recursos de la red interna a los que no tendría acceso desde Internet. Por ejemplo, una opción para descargar una imagen externa podría terminar consultando un servicio local en vez de una imagen.

En una instancia **Amazon EC2**, la dirección `169.254.169.254` apunta al servicio de metadatos de la instancia, llamado IMDS. Ahí hay información de identidad y configuración. Si la instancia tiene asociado un rol IAM, también pueden consultarse sus **credenciales temporales**: identificador de clave, clave secreta, token de sesión y vencimiento. Si la aplicación vulnerable devuelve esa respuesta, el atacante podría usar los permisos de ese rol. También podría encontrar datos en `user-data` si alguien guardó información sensible allí.

Esto depende de la infraestructura real. Estar alojado en AWS no significa necesariamente correr en EC2, y una instancia sin rol IAM no tiene credenciales de ese rol para extraer. El alcance del daño depende además de los permisos asignados.

La versión del servicio de metadatos también importa. Con **IMDSv1** habilitado, un `GET` desde la instancia puede alcanzar una ruta de metadatos. Si se exige **IMDSv2**, antes hay que pedir un token con `PUT /latest/api/token` e incluirlo en la cabecera de la consulta posterior. Un SSRF que solo deja elegir la URL de un `GET` normalmente no puede completar ese intercambio. Si permite controlar métodos y cabeceras, el análisis cambia; IMDSv2 no arregla cualquier SSRF ni protege otros servicios internos.

Para prevenirlo, restringiría los destinos permitidos, comprobaría la dirección final después de redirecciones y resolución DNS y bloquearía desde el backend las direcciones internas o de enlace local que no necesita consultar. En EC2 exigiría IMDSv2 y daría al rol IAM solo los permisos necesarios.

## Fuentes

- [AWS EC2, acceso a metadatos de instancia](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html).
- [AWS EC2, credenciales de seguridad en metadatos](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-metadata-security-credentials.html).
- [AWS EC2, configuración de IMDS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html).
- [OWASP, Server-Side Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html).
