# Interceptación de tráfico con Burp Suite

## Laboratorio: Turnero

1. Inicio Burp Suite y verifico que el Proxy Listener esté activo en `127.0.0.1:8080`.
2. En Chrome activo el perfil de FoxyProxy configurado para enviar el tráfico a Burp y abro el laboratorio Turnero.
3. En Burp voy a **Proxy → Intercept** y activo la interceptación.
4. Al cargar la aplicación, Burp detiene una petición antes de enviarla al servidor:

   - **Método:** `GET`
   - **Endpoint:** `/`
   - **Host:** `chl-ff0b2857-c2e2-424e-941d-b6c4ba13e5d4-turnero.softwareseguro.com.ar`
   - **Protocolo:** HTTP/2

5. La ruta `/` es la raíz de la aplicación Turnero. En la petición observada, el `Referer` apunta a `/login?next=%2F`, lo que indica que la navegación a la raíz ocurrió desde el flujo de inicio de sesión. La captura por sí sola no permite confirmar si el servidor devuelve la página principal o redirige al login; para verificarlo habría que revisar el código de estado de la respuesta en el historial de Burp.
6. La petición quedó detenida en **Proxy → Intercept**, lo que confirma que Chrome está pasando el tráfico por Burp y que se puede revisar antes de que continúe al servidor.

**Nota:** Se omitieron cookies y valores de sesión de la documentación porque son datos sensibles.

## Otros endpoints encontrados

Al seguir navegando el lab aparecen estos endpoints:

- `GET /api/appointments/{id}` — trae los datos de un turno según el id.
- `DELETE /api/appointments/{id}` — borra el turno de ese id.
- `POST /login` — manda usuario y contraseña para iniciar sesión.

Se ve que la app identifica cada turno por el id de la ruta, asique cambiando ese número se puede pedir o borrar turnos de otro usuario (IDOR).
