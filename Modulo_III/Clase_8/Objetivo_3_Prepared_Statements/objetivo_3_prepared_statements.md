# Cómo evitan la SQLi las consultas parametrizadas

Si intento frenar una SQLi borrando palabras como `SELECT` o `UNION`, el atacante puede cambiar la sintaxis o la codificación. Escapar comillas a mano también es fácil de hacer mal. La solución para los **valores** que vienen del usuario es armar la estructura de la consulta por un lado y pasar esos valores como parámetros por otro.

## Cómo funciona

Por ejemplo, el programa define esta consulta:

```sql
SELECT id FROM usuarios WHERE email = ?
```

El `?` marca el lugar de un dato. Después vinculo ahí el email recibido mediante la API de la base de datos, sin pegarlo al texto SQL. Si alguien escribe `' OR 1=1--`, la consulta busca literalmente ese texto como email: el `OR` no se convierte en una condición nueva.

Lo que protege es la separación entre la estructura SQL y el valor vinculado. Según el controlador, la preparación puede ser nativa o emulada en el cliente. Por eso no diría que siempre viajan dos mensajes de red distintos ni que el servidor compila la consulta antes de recibir cualquier dato: esos detalles cambian, pero la API debe tratar el parámetro como dato.

## El límite de esta defensa

Tengo que vincular **todos los valores no confiables**. Si primero concateno un dato del usuario dentro del SQL y después preparo la consulta, la inyección ya quedó en la estructura. Tampoco puedo usar `?` para un nombre de tabla, de columna o para elegir `ASC` o `DESC`. Si esas partes deben variar, selecciono una opción de una lista permitida que define la aplicación.

Así neutralizo la inyección en los valores bien vinculados, sin importar que contengan comillas o palabras de SQL. Igual necesito permisos mínimos en la base de datos y validar las reglas del negocio: parametrizar no resuelve esos otros problemas.

## Fuentes

- [OWASP, SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet): consultas parametrizadas, límites de los marcadores y listas permitidas para partes estructurales.
- [PHP, documentación de `PDO::prepare`](https://www.php.net/manual/en/pdo.prepare.php): vinculación de parámetros y diferencias entre preparación nativa y emulada.
