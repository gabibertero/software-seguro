# La mitigación de SQLi con consultas parametrizadas

Bloquear palabras como `SELECT` o `UNION` y escapar comillas a mano no es una defensa confiable: hay distintas sintaxis, codificaciones y contextos de consulta. Para los valores provenientes de usuarios, la defensa principal es definir la estructura SQL y pasar esos valores mediante parámetros de la API de base de datos.

## Cómo funciona

El código define una consulta con marcadores de posición:

```sql
SELECT * FROM usuarios WHERE user = ? AND pass = ?
```

Luego vincula los valores concretos a esos dos marcadores usando la API del controlador, en vez de concatenarlos al texto SQL. Así, `?` representa un **valor de dato** dentro de una estructura ya definida por el programa, no un lugar donde insertar un fragmento de sintaxis SQL. Por ejemplo, si el usuario ingresa `' OR 1=1--` en el primer campo, la comparación busca literalmente ese texto como nombre de usuario; no agrega una condición `OR` a la consulta.

En una preparación nativa el controlador y el servidor gestionan la plantilla y los parámetros según su protocolo. Algunos controladores también admiten preparaciones emuladas en el cliente. Por eso la separación entre código y datos es la propiedad de seguridad que importa; **no** hay que suponer que siempre viajan en dos mensajes de red distintos ni que el servidor compila un plan antes de recibir cualquier valor.

## Qué protege y qué requiere atención

Cuando el programa vincula correctamente **todos los valores no confiables**, los caracteres enviados por el usuario se tratan como datos y no pueden alterar la estructura SQL en esas posiciones. Preparar una consulta que ya contiene datos concatenados no corrige esa inyección. Los marcadores tampoco reemplazan nombres de tablas, columnas ni palabras clave como `ASC` o `DESC`: si esas partes deben variar, se elige entre opciones fijas permitidas por la aplicación.

La consulta parametrizada evita la SQLi en los valores vinculados, pero no sustituye otras medidas como permisos mínimos de la cuenta de base de datos y validaciones propias de la lógica de negocio.

## Fuentes

- [OWASP, SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet): consultas parametrizadas, límites de los marcadores y listas permitidas para partes estructurales.
- [PHP, documentación de `PDO::prepare`](https://www.php.net/manual/en/pdo.prepare.php): vinculación de parámetros y diferencias entre preparación nativa y emulada.
