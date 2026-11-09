# Manipulación de una petición con Repeater

## Laboratorio: Turnero

1. Cancelé una cita en el lab y capturé la petición que generó el navegador:

   - **Método:** `DELETE`
   - **Endpoint original:** `/api/appointments/2`
   - **Host:** `chl-a5eec477-10cd-4da7-b0de-5edaceff6a67-turnero.softwareseguro.com.ar`

2. Desde **Proxy → HTTP history** la mandé a **Repeater**.
3. Cambié el ID de la ruta de `/api/appointments/2` a `/api/appointments/4` y apreté **Send**.
4. El servidor respondió `HTTP/2 200 OK` con un JSON (`Content-Length: 44`). Después comprobé en el lab que la cita con ID 4 se había eliminado.

La prueba me mostró que el ID de la ruta define sobre qué cita actúa el `DELETE`. Cambiarlo desde Repeater alcanzó para afectar otra cita.

## Captura

En la imagen se ve la petición modificada y la respuesta. Tapé la cabecera `Cookie` para no dejar el valor de sesión en el informe.

![Petición DELETE modificada en Repeater y respuesta HTTP 200](captura_repeater_redactada.png)
