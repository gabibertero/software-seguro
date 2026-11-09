# Interceptación de tráfico con Burp Suite

## Laboratorio: Turnero

1. Abrí Burp Suite y comprobé que el Proxy Listener estuviera activo en `127.0.0.1:8080`.
2. En Chrome activé el perfil de FoxyProxy que manda el tráfico a Burp y entré al laboratorio Turnero.
3. En **Proxy → Intercept** activé la interceptación. Al cargar el lab, Burp frenó esta petición antes de enviarla al servidor:

   - **Método:** `GET`
   - **Endpoint:** `/`
   - **Host:** `chl-ff0b2857-c2e2-424e-941d-b6c4ba13e5d4-turnero.softwareseguro.com.ar`
   - **Protocolo:** HTTP/2

4. La ruta `/` es la raíz de Turnero. El `Referer` apunta a `/login?next=%2F`, así que la petición venía del flujo de inicio de sesión. Con esta petición sola no puedo saber si el servidor mostró la página principal o redirigió al login: para eso tendría que mirar el estado de la respuesta en el historial de Burp.
5. Ver la petición detenida en **Proxy → Intercept** me confirmó que Chrome pasaba por Burp y que podía revisar el tráfico antes de dejarlo continuar.
