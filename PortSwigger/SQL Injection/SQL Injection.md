## ¿Qué es la inyección SQL (SQLi)?

La inyección SQL (SQLi) es una vulnerabilidad de seguridad web que permite a un atacante interferir con las consultas que realiza una aplicación a su base de datos. Esto puede permitir que un atacante vea datos que normalmente no puede recuperar. Esto podría incluir datos que pertenecen a otros usuarios o cualquier otro dato al que pueda acceder la aplicación. En muchos casos, un atacante puede modificar o eliminar estos datos, provocando cambios persistentes en el contenido o el comportamiento de la aplicación.

En algunas situaciones, un atacante puede escalar un ataque de inyección SQL para comprometer el servidor subyacente u otra infraestructura de back-end. También puede permitirles realizar ataques de denegación de servicio.

## Cómo detectar vulnerabilidades de inyección SQL

Puede detectar la inyección de SQL manualmente utilizando un conjunto sistemático de pruebas en cada punto de entrada de la aplicación. Para hacer esto, normalmente enviarías:

- El carácter de comilla simple `'`y busca errores u otras anomalías.
- Alguna sintaxis específica de SQL que evalúa el valor base (original) del punto de entrada y un valor diferente, y busca diferencias sistemáticas en las respuestas de la aplicación.
- Condiciones booleanas como `OR 1=1`y `OR 1=2`y busque diferencias en las respuestas de la aplicación.
- Cargas útiles diseñadas para desencadenar retrasos de tiempo cuando se ejecutan dentro de una consulta SQL y buscar diferencias en el tiempo necesario para responder.
- Cargas útiles de OAST diseñadas para desencadenar una interacción de red fuera de banda cuando se ejecuta dentro de una consulta SQL y monitorear cualquier interacción resultante.

Alternativamente, puede encontrar la mayoría de las vulnerabilidades de inyección SQL de forma rápida y confiable utilizando Burp Scanner.

## Inyección SQL en diferentes partes de la consulta.

La mayoría de las vulnerabilidades de inyección SQL ocurren dentro de la `WHERE`cláusula de una `SELECT`consulta. Los evaluadores más experimentados están familiarizados con este tipo de inyección SQL.

Sin embargo, las vulnerabilidades de inyección SQL pueden ocurrir en cualquier ubicación dentro de la consulta y dentro de diferentes tipos de consulta. Algunas otras ubicaciones comunes donde surge la inyección SQL son:

- En `UPDATE`declaraciones, dentro de los valores actualizados o de la `WHERE`cláusula.
- En `INSERT`declaraciones, dentro de los valores insertados.
- En `SELECT`declaraciones, dentro del nombre de la tabla o columna.
- En `SELECT`declaraciones, dentro de la `ORDER BY`cláusula.

## Recuperar datos ocultos

Imagine una aplicación de compras que muestra productos en diferentes categorías. Cuando el usuario hace clic en la categoría **Regalos** , su navegador solicita la URL:

`https://insecure-website.com/products?category=Gifts`

Esto hace que la aplicación realice una consulta SQL para recuperar detalles de los productos relevantes de la base de datos:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

Esta consulta SQL solicita a la base de datos que devuelva:

- Todos los detalles ( `*`)
- de la `products`mesa
- donde `category`esta`Gifts`
- y `released`es `1`.

La restricción `released = 1`se utiliza para ocultar productos que no se comercializan. Podríamos suponer que para productos inéditos, `released = 0`.

## Recuperar datos ocultos - Continuación

La aplicación no implementa ninguna defensa contra ataques de inyección SQL. Esto significa que un atacante puede construir el siguiente ataque, por ejemplo:

`https://insecure-website.com/products?category=Gifts'--`

Esto da como resultado la consulta SQL:

`SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

Fundamentalmente, tenga en cuenta que `--`es un indicador de comentario en SQL. Esto significa que el resto de la consulta se interpreta como un comentario, eliminándolo efectivamente. En este ejemplo, esto significa que la consulta ya no incluye `AND released = 1`. Como resultado, se muestran todos los productos, incluidos aquellos que aún no se han lanzado.

Puedes utilizar un ataque similar para hacer que la aplicación muestre todos los productos en cualquier categoría, incluidas las categorías que no conocen:

`https://insecure-website.com/products?category=Gifts'+OR+1=1--`

Esto da como resultado la consulta SQL:

`SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

La consulta modificada devuelve todos los elementos donde `category`es `Gifts`o `1`es igual a `1`. Como `1=1`siempre ocurre, la consulta devuelve todos los elementos.

#### Advertencia

Tenga cuidado al inyectar la condición `OR 1=1`en una consulta SQL. Incluso si parece inofensivo en el contexto en el que se está inyectando, es común que las aplicaciones utilicen datos de una única solicitud en varias consultas diferentes. Si su condición llega a una declaración `UPDATE`o `DELETE`, por ejemplo, puede resultar en una pérdida accidental de datos.

## Subvirtiendo la lógica de la aplicación

Imagine una aplicación que permita a los usuarios iniciar sesión con un nombre de usuario y contraseña. Si un usuario envía el nombre de usuario `wiener`y la contraseña `bluecheese`, la aplicación verifica las credenciales realizando la siguiente consulta SQL:

`SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

Si la consulta devuelve los detalles de un usuario, entonces el inicio de sesión se realizó correctamente. En caso contrario, se rechaza.

En este caso, un atacante puede iniciar sesión como cualquier usuario sin necesidad de contraseña. Pueden hacer esto usando la secuencia de comentarios SQL `--`para eliminar la verificación de contraseña de la `WHERE`cláusula de la consulta. Por ejemplo, enviar el nombre de usuario `administrator'--`y una contraseña en blanco da como resultado la siguiente consulta:

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

Esta consulta devuelve el usuario cuyo `username`nombre es `administrator`y registra exitosamente al atacante como ese usuario.

## Ataques UNION de inyección SQL

Cuando una aplicación es vulnerable a la inyección SQL y los resultados de la consulta se devuelven dentro de las respuestas de la aplicación, puede usar la `UNION`palabra clave para recuperar datos de otras tablas dentro de la base de datos. Esto se conoce comúnmente como ataque UNION de inyección SQL.

La `UNION`palabra clave le permite ejecutar una o más `SELECT`consultas adicionales y agregar los resultados a la consulta original. Por ejemplo:

`SELECT a, b FROM table1 UNION SELECT c, d FROM table2`

Esta consulta SQL devuelve un único conjunto de resultados con dos columnas, que contiene valores de columnas `a`y `b`en `table1`y columnas `c`y `d`en `table2`.

## Ataques UNION de inyección SQL - Continuación

Para que una `UNION`consulta funcione, se deben cumplir dos requisitos clave:

- Las consultas individuales deben devolver el mismo número de columnas.
- Los tipos de datos de cada columna deben ser compatibles entre las consultas individuales.

Para llevar a cabo un ataque UNION de inyección SQL, asegúrese de que su ataque cumpla con estos dos requisitos. Esto normalmente implica descubrir:

- Cuántas columnas se devuelven de la consulta original.
- Qué columnas devueltas por la consulta original tienen un tipo de datos adecuado para contener los resultados de la consulta inyectada.

## Determinar el número de columnas necesarias

Cuando realiza un ataque UNION de inyección SQL, existen dos métodos efectivos para determinar cuántas columnas se devuelven de la consulta original.

Un método implica inyectar una serie de `ORDER BY`cláusulas e incrementar el índice de la columna especificada hasta que ocurra un error. Por ejemplo, si el punto de inyección es una cadena entre comillas dentro de la `WHERE`cláusula de la consulta original, deberá enviar:

`' ORDER BY 1-- ' ORDER BY 2-- ' ORDER BY 3-- etc.`

Esta serie de cargas útiles modifica la consulta original para ordenar los resultados por diferentes columnas en el conjunto de resultados. La columna de una `ORDER BY`cláusula se puede especificar por su índice, por lo que no es necesario conocer los nombres de ninguna columna. Cuando el índice de columna especificado excede el número de columnas reales en el conjunto de resultados, la base de datos devuelve un error, como por ejemplo:

`The ORDER BY position number 3 is out of range of the number of items in the select list.`

En realidad, la aplicación podría devolver el error de la base de datos en su respuesta HTTP, pero también puede emitir una respuesta de error genérica. En otros casos, es posible que simplemente no arroje ningún resultado. De cualquier manera, siempre que pueda detectar alguna diferencia en la respuesta, puede inferir cuántas columnas se devuelven de la consulta.

## Determinar el número de columnas necesarias - Continuación

El segundo método implica enviar una serie de `UNION SELECT`cargas útiles que especifican un número diferente de valores nulos:

```
' UNION SELECT NULL-- 
' UNION SELECT NULL,NULL-- 
' UNION SELECT NULL,NULL,NULL-- etc.
```

Si el número de valores nulos no coincide con el número de columnas, la base de datos devuelve un error, como por ejemplo:

`All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.`

Usamos `NULL`como valores devueltos por la `SELECT`consulta inyectada porque los tipos de datos en cada columna deben ser compatibles entre las consultas originales y las inyectadas. `NULL`es convertible a todos los tipos de datos comunes, por lo que maximiza la posibilidad de que la carga útil tenga éxito cuando el recuento de columnas sea correcto.

Al igual que con la `ORDER BY`técnica, la aplicación puede devolver el error de la base de datos en su respuesta HTTP, pero puede devolver un error genérico o simplemente no devolver ningún resultado. Cuando el número de valores nulos coincide con el número de columnas, la base de datos devuelve una fila adicional en el conjunto de resultados, que contiene valores nulos en cada columna. El efecto sobre la respuesta HTTP depende del código de la aplicación. Si tiene suerte, verá contenido adicional dentro de la respuesta, como una fila adicional en una tabla HTML. De lo contrario, los valores nulos podrían desencadenar un error diferente, como un archivo `NullPointerException`. En el peor de los casos, la respuesta podría tener el mismo aspecto que una respuesta provocada por un número incorrecto de valores nulos. Esto haría que este método fuera ineficaz.

## Sintaxis específica de la base de datos

En Oracle, cada `SELECT`consulta debe utilizar la `FROM`palabra clave y especificar una tabla válida. Hay una tabla integrada en Oracle llamada `dual`que se puede utilizar para este propósito. Entonces, las consultas inyectadas en Oracle deberían verse así:

`' UNION SELECT NULL FROM DUAL--`

Las cargas útiles descritas utilizan la secuencia de comentarios de doble guión `--`para comentar el resto de la consulta original después del punto de inyección. En MySQL, la secuencia de doble guión debe ir seguida de un espacio. Alternativamente, `#`se puede utilizar el carácter almohadilla para identificar un comentario.

Para obtener más detalles sobre la sintaxis específica de la base de datos, consulte la [hoja de referencia de inyección SQL](https://portswigger.net/web-security/sql-injection/cheat-sheet) .

## Encontrar columnas con un tipo de datos útil

Un ataque UNION de inyección SQL le permite recuperar los resultados de una consulta inyectada. Los datos interesantes que desea recuperar normalmente están en forma de cadena. Esto significa que necesita encontrar una o más columnas en los resultados de la consulta original cuyo tipo de datos sea, o sea compatible con, datos de cadena.

Después de determinar la cantidad de columnas requeridas, puede sondear cada columna para probar si puede contener datos de cadena. Puede enviar una serie de `UNION SELECT`cargas útiles que coloquen un valor de cadena en cada columna por turno. Por ejemplo, si la consulta devuelve cuatro columnas, deberá enviar:

```
' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 
' UNION SELECT NULL,NULL,NULL,'a'--
```

Si el tipo de datos de la columna no es compatible con los datos de cadena, la consulta inyectada provocará un error en la base de datos, como por ejemplo:

`Conversion failed when converting the varchar value 'a' to data type int.`

Si no se produce un error y la respuesta de la aplicación contiene algún contenido adicional, incluido el valor de cadena inyectado, entonces la columna relevante es adecuada para recuperar datos de cadena.

## Usando un ataque UNION de inyección SQL para recuperar datos interesantes

Cuando haya determinado el número de columnas devueltas por la consulta original y haya encontrado qué columnas pueden contener datos de cadena, estará en condiciones de recuperar datos interesantes.

Suponer que:

- La consulta original devuelve dos columnas, las cuales pueden contener datos de cadena.
- El punto de inyección es una cadena entre comillas dentro de la `WHERE`cláusula.
- La base de datos contiene una tabla llamada `users`con las columnas `username`y `password`.

En este ejemplo, puede recuperar el contenido de la `users`tabla enviando la entrada:

`' UNION SELECT username, password FROM users--`

Para poder realizar este ataque, necesitas saber que hay una tabla llamada `users`con dos columnas llamadas `username`y `password`. Sin esta información, tendrías que adivinar los nombres de las tablas y columnas. Todas las bases de datos modernas ofrecen formas de examinar la estructura de la base de datos y determinar qué tablas y columnas contienen.

## Recuperar múltiples valores dentro de una sola columna

En algunos casos, es posible que la consulta del ejemplo anterior solo devuelva una única columna.

Puede recuperar varios valores juntos dentro de esta única columna concatenando los valores. Puede incluir un separador que le permita distinguir los valores combinados. Por ejemplo, en Oracle podrías enviar la entrada:

`' UNION SELECT username || '~' || password FROM users--`

Esto utiliza la secuencia de doble canalización, `||`que es un operador de concatenación de cadenas en Oracle. La consulta inyectada concatena los valores de los campos `username`y `password`, separados por el `~`carácter.

Los resultados de la consulta contienen todos los nombres de usuario y contraseñas, por ejemplo:

`... administrator~s3cure wiener~peter carlos~montoya ...`

Diferentes bases de datos utilizan diferentes sintaxis para realizar la concatenación de cadenas. Para obtener más detalles, consulte la [hoja de referencia de inyección SQL](https://portswigger.net/web-security/sql-injection/cheat-sheet) .

## Examinando la base de datos en ataques de inyección SQL

Para explotar las vulnerabilidades de inyección SQL, a menudo es necesario encontrar información sobre la base de datos. Esto incluye:

- El tipo y versión del software de base de datos.
- Las tablas y columnas que contiene la base de datos.

## Consultar el tipo y la versión de la base de datos.

Potencialmente, puede identificar tanto el tipo como la versión de la base de datos inyectando consultas específicas del proveedor para ver si alguna funciona.

Las siguientes son algunas consultas para determinar la versión de la base de datos para algunos tipos de bases de datos populares:

|   |   |
|---|---|
|Tipo de base de datos|Consulta|
|Microsoft, MySQL|`SELECT @@version`|
|Oráculo|`SELECT * FROM v$version`|
|PostgreSQL|`SELECT version()`|

Por ejemplo, podrías utilizar un `UNION`ataque con la siguiente entrada:

`' UNION SELECT @@version--`

Esto podría devolver el siguiente resultado. En este caso, puedes confirmar que la base de datos es Microsoft SQL Server y ver la versión utilizada:

`Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64) Mar 18 2018 09:11:49 Copyright (c) Microsoft Corporation Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)`


## Listado del contenido de la base de datos.

La mayoría de los tipos de bases de datos (excepto Oracle) tienen un conjunto de vistas llamado esquema de información. Esto proporciona información sobre la base de datos.

Por ejemplo, puede realizar una consulta `information_schema.tables`para enumerar las tablas de la base de datos:

`SELECT * FROM information_schema.tables`

Esto devuelve un resultado como el siguiente:
![[Pasted image 20240622170025.png]]

Este resultado indica que hay tres tablas, llamadas `Products`, `Users`y `Feedback`.

Luego puede realizar consultas `information_schema.columns`para enumerar las columnas en tablas individuales:

`SELECT * FROM information_schema.columns WHERE table_name = 'Users'`

Esto devuelve un resultado como el siguiente:
![[Pasted image 20240622170044.png]]

Este resultado muestra las columnas de la tabla especificada y el tipo de datos de cada columna.

## Inyección SQL ciega

En esta sección, describimos técnicas para encontrar y explotar vulnerabilidades de inyección SQL ciega.

