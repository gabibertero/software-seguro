# ¿Qué es un proxy y en qué se diferencia de una VPN?

Un **proxy** queda entre mi navegador y el sitio al que entro: recibe la petición, la reenvía y después me devuelve la respuesta. En este curso uso Burp Suite como proxy para detener una petición y ver qué datos manda el navegador antes de que lleguen al laboratorio. Se configura para esa aplicación o navegador; por sí solo, un proxy no garantiza que el tráfico vaya cifrado.

Una **VPN** crea un túnel cifrado desde mi equipo hasta el servidor de la VPN. Normalmente se configura a nivel del sistema y puede llevar por ese túnel el tráfico de distintas aplicaciones. Protege ese tramo de la conexión, aunque el servidor de la VPN y el sitio de destino siguen siendo partes importantes del recorrido; usar una VPN no vuelve segura automáticamente a una página vulnerable.

La diferencia que me importa para los laboratorios es esta: **Burp me deja inspeccionar y modificar las peticiones del navegador**. Una VPN se usa para enrutar y proteger la conexión hacia su servidor, no para trabajar con cada petición HTTP como hago en Burp.

