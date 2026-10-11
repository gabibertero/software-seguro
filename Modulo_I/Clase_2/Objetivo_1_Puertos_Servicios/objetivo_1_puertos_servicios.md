# Puertos y Servicios Esenciales



**Puerto 20 - FTP (datos):** Es el canal por el que FTP transfiere el contenido de los archivos. Trabaja junto al puerto 21, que se encarga de los comandos, y se usa principalmente en el modo activo de FTP. Desde el punto de vista de la seguridad, los datos viajan sin cifrar, por lo que cualquiera que intercepte el tráfico puede ver los archivos transferidos.

**Puerto 21 - FTP (control):** Es el puerto de control del protocolo FTP (File Transfer Protocol). Por acá el cliente se autentica con usuario y contraseña y envía los comandos para listar, subir o descargar archivos. El problema es que las credenciales viajan en texto plano, por eso hoy se recomienda usar SFTP o FTPS, y revisar que no esté habilitado el acceso anónimo.

**Puerto 22 - SSH:** Corresponde a SSH (Secure Shell), que permite administrar un servidor de forma remota a través de una consola, con toda la comunicación cifrada. También se usa para transferir archivos de forma segura mediante SFTP y SCP. Aunque es seguro por diseño, al estar expuesto suele recibir ataques de fuerza bruta, por lo que conviene usar autenticación por claves y deshabilitar el login directo como root.

**Puerto 23 - Telnet:** Es el antecesor de SSH y también permite conectarse remotamente a un equipo por consola. La diferencia es que no cifra nada, así que usuario, contraseña y comandos pueden ser leídos por cualquiera en la red. Por eso se considera obsoleto e inseguro, y encontrarlo abierto en un servidor es una mala señal.

**Puerto 25 - SMTP:** Lo usa SMTP (Simple Mail Transfer Protocol), el protocolo encargado de enviar correos electrónicos y de transferirlos entre servidores de correo. En su forma original no cifra ni exige autenticación, lo que puede derivar en un servidor "open relay" usado para mandar spam o correos falsificados. Hoy se complementa con STARTTLS y con mecanismos como SPF, DKIM y DMARC.

**Puerto 53 - DNS:** Corresponde al servicio DNS (Domain Name System), que traduce nombres de dominio como google.com a direcciones IP. Las consultas normales usan UDP, mientras que TCP se usa para respuestas grandes y transferencias de zona entre servidores. A nivel de seguridad puede ser blanco de envenenamiento de caché (DNS spoofing) o filtrar información si permite transferencias de zona a cualquiera.

**Puerto 80 - HTTP:** Es el puerto por defecto de HTTP (HyperText Transfer Protocol), el protocolo con el que el navegador pide y recibe páginas web. Toda la información viaja en texto plano, incluidas contraseñas o cookies de sesión si el sitio no está protegido. Lo recomendable es que solo se use para redirigir al usuario hacia HTTPS.

**Puerto 110 - POP3:** Lo usa POP3 (Post Office Protocol v3), que permite a un cliente de correo descargar los mensajes desde el servidor, generalmente borrándolos de este una vez descargados. Al igual que FTP y Telnet, envía las credenciales sin cifrar, por eso se reemplaza por su versión segura POP3S en el puerto 995.

**Puerto 443 - HTTPS:** Corresponde a HTTPS, que es HTTP funcionando sobre TLS. Es el que usa hoy la gran mayoría de los sitios web, ya que cifra la comunicación entre el navegador y el servidor y valida la identidad de este mediante un certificado digital. Aun así, que el canal esté cifrado no significa que la aplicación sea segura: las vulnerabilidades propias de la web siguen existiendo detrás.

**Puerto 3306 - MySQL:** Es el puerto por defecto de los motores de base de datos MySQL y MariaDB, por donde las aplicaciones se conectan para consultar y modificar datos. Este puerto no debería estar expuesto a internet, sino accesible solo desde el servidor de la aplicación o la red interna, ya que si queda abierto es un objetivo directo de fuerza bruta y robo de información.
