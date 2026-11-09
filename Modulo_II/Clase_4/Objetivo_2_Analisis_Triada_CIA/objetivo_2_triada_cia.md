# Análisis de la Tríada CIA

La **confidencialidad** busca que solo vea los datos quien tiene permiso.

La **integridad** busca que los datos no se modifiquen sin autorización y sigan siendo correctos.

La **disponibilidad** significa que el sistema y sus datos estén accesibles cuando se los necesita.

Un IDOR ayuda a ver cómo se relacionan los tres pilares. Si cambio el `id` de una petición y la aplicación me muestra el registro de otra persona sin comprobar permisos, falla la confidencialidad. Si también me deja editarlo, falla la integridad. Y si puedo borrarlo, la persona autorizada puede perder el acceso a ese registro, lo que afecta la disponibilidad.

Otros ejemplos, uno por pilar:

- **Confidencialidad:** un IDOR que permite ver datos de otra persona cambiando el identificador de la petición.
- **Integridad:** un CSRF que modifica datos de una cuenta sin que su dueño haya pedido ese cambio.
- **Disponibilidad:** un ataque de denegación de servicio (DoS) que satura el servidor y deja la aplicación inaccesible para sus usuarios.
