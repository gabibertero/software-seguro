# Peligros del Directory Listing

Si entro a una ruta que apunta a una carpeta, el servidor primero busca un archivo de inicio, como `index.html`. Si no lo encuentra y tiene habilitado el *directory listing*, me muestra una lista de lo que hay dentro. Según la configuración, esa página puede revelar nombres de archivos y subcarpetas, tamaños y fechas. En Apache se habilita con `Options Indexes`; en Nginx, con `autoindex on`.

El listado no me da permisos nuevos, pero sí me muestra archivos que quizás nunca habría encontrado. Si además se pueden abrir, podrían quedar expuestos un `.env`, copias de seguridad, volcados de base de datos, registros, código fuente o archivos comprimidos. Ahí puede haber credenciales, claves, rutas internas o datos de usuarios.

Para evitarlo, deshabilitaría el listado (`Options -Indexes` en Apache o `autoindex off` en Nginx) y pondría un archivo de índice cuando haga falta. Igual, eso no reemplaza los permisos: los archivos sensibles tienen que quedar fuera del directorio público, las copias temporales se deben borrar y el servidor debe bloquear el acceso directo a rutas privadas. La comprobación final es sencilla: entrar desde el navegador a una carpeta sin índice y verificar que no aparezca su contenido.
