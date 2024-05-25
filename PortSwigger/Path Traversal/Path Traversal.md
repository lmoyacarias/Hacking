
# ¿Qué es el recorrido del camino?

El recorrido de ruta también se conoce como recorrido de directorio. Estas vulnerabilidades permiten a un atacante leer archivos arbitrarios en el servidor que ejecuta una aplicación. Esto podría incluir:

- Código y datos de la aplicación.
- Credenciales para sistemas back-end.
- Archivos sensibles del sistema operativo.

En algunos casos, un atacante podría escribir en archivos arbitrarios en el servidor, lo que le permitiría modificar los datos o el comportamiento de la aplicación y, en última instancia, tomar el control total del servidor.

# Lectura de archivos arbitrarios mediante recorrido de ruta

Imagine una aplicación de compras que muestra imágenes de artículos a la venta. Esto podría cargar una imagen usando el siguiente HTML:

`<img src="/loadImage?filename=218.png">`

La `loadImage`URL toma un `filename`parámetro y devuelve el contenido del archivo especificado. Los archivos de imagen se almacenan en el disco en la ubicación `/var/www/images/`. Para devolver una imagen, la aplicación agrega el nombre de archivo solicitado a este directorio base y utiliza una API del sistema de archivos para leer el contenido del archivo. En otras palabras, la aplicación lee la siguiente ruta de archivo:

`/var/www/images/218.png`

Esta aplicación no implementa defensas contra ataques de recorrido de ruta. Como resultado, un atacante puede solicitar la siguiente URL para recuperar el `/etc/passwd`archivo del sistema de archivos del servidor:

`https://insecure-website.com/loadImage?filename=../../../etc/passwd`

Esto hace que la aplicación lea desde la siguiente ruta de archivo:

`/var/www/images/../../../etc/passwd`

La secuencia `../`es válida dentro de una ruta de archivo y significa subir un nivel en la estructura del directorio. Las tres secuencias consecutivas `../`avanzan desde `/var/www/images/`la raíz del sistema de archivos, por lo que el archivo que realmente se lee es:

`/etc/passwd`

En los sistemas operativos basados ​​en Unix, este es un archivo estándar que contiene detalles de los usuarios registrados en el servidor, pero un atacante podría recuperar otros archivos arbitrarios utilizando la misma técnica.

En Windows, ambos `../`y `..\`son secuencias válidas de recorrido de directorio. El siguiente es un ejemplo de un ataque equivalente contra un servidor basado en Windows:

`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`


## Obstáculos comunes para explotar las vulnerabilidades de recorrido de ruta

Muchas aplicaciones que colocan la entrada del usuario en las rutas de los archivos implementan defensas contra ataques de cruce de rutas. A menudo estos pueden evitarse.

Si una aplicación elimina o bloquea secuencias de recorrido de directorio del nombre de archivo proporcionado por el usuario, es posible evitar la defensa utilizando una variedad de técnicas.

Es posible que pueda utilizar una ruta absoluta desde la raíz del sistema de archivos, como `filename=/etc/passwd`, para hacer referencia directamente a un archivo sin utilizar secuencias transversales.


## Obstáculos comunes para explotar las vulnerabilidades de recorrido de ruta - Continuación

Es posible que puedas utilizar secuencias transversales anidadas, como `....//`o `....\/`. Estos vuelven a secuencias transversales simples cuando se elimina la secuencia interna.

## Obstáculos comunes para explotar las vulnerabilidades de recorrido de ruta - Continuación

En algunos contextos, como en una ruta URL o el `filename`parámetro de una `multipart/form-data`solicitud, los servidores web pueden eliminar cualquier secuencia de recorrido del directorio antes de pasar su entrada a la aplicación. A veces puedes evitar este tipo de desinfección mediante la codificación de URL, o incluso la codificación de URL doble, de los `../`caracteres. Esto da como resultado `%2e%2e%2f`y `%252e%252e%252f`respectivamente. También pueden funcionar varias codificaciones no estándar, como `..%c0%af`o `..%ef%bc%8f`.

Para los usuarios de Burp Suite Professional, Burp Intruder proporciona la lista de carga útil predefinida **Fuzzing: recorrido de ruta** . Contiene algunas secuencias de recorrido de ruta codificadas que puedes probar.

## Obstáculos comunes para explotar las vulnerabilidades de recorrido de ruta - Continuación

Es posible que una aplicación requiera que el nombre de archivo proporcionado por el usuario comience con la carpeta base esperada, como `/var/www/images`. En este caso, podría ser posible incluir la carpeta base requerida seguida de secuencias transversales adecuadas. Por ejemplo: `filename=/var/www/images/../../../etc/passwd`.

## Obstáculos comunes para explotar las vulnerabilidades de recorrido de ruta - Continuación

Una aplicación puede requerir que el nombre de archivo proporcionado por el usuario termine con una extensión de archivo esperada, como `.png`. En este caso, podría ser posible utilizar un byte nulo para terminar efectivamente la ruta del archivo antes de la extensión requerida. Por ejemplo: `filename=../../../etc/passwd%00.png`.

# Cómo prevenir un ataque de recorrido de ruta

La forma más eficaz de evitar vulnerabilidades de recorrido de ruta es evitar por completo pasar la entrada proporcionada por el usuario a las API del sistema de archivos. Muchas funciones de aplicaciones que hacen esto se pueden reescribir para ofrecer el mismo comportamiento de una manera más segura.

Si no puede evitar pasar información proporcionada por el usuario a las API del sistema de archivos, le recomendamos utilizar dos capas de defensa para evitar ataques:

- Valide la entrada del usuario antes de procesarla. Lo ideal es comparar la entrada del usuario con una lista blanca de valores permitidos. Si eso no es posible, verifique que la entrada contenga solo contenido permitido, como caracteres alfanuméricos únicamente.
- Después de validar la entrada proporcionada, agregue la entrada al directorio base y use una API del sistema de archivos de la plataforma para canonicalizar la ruta. Verifique que la ruta canonicalizada comience con el directorio base esperado.

A continuación se muestra un ejemplo de código Java simple para validar la ruta canónica de un archivo según la entrada del usuario:

`File file = new File(BASE_DIRECTORY, userInput); if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) { // process file }`

