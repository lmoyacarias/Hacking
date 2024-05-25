
## ¿Qué es la SSRF?

La falsificación de solicitudes del lado del servidor es una vulnerabilidad de seguridad web que permite a un atacante hacer que la aplicación del lado del servidor realice solicitudes a una ubicación no deseada.

En un ataque SSRF típico, el atacante podría hacer que el servidor establezca una conexión con servicios exclusivamente internos dentro de la infraestructura de la organización. En otros casos, pueden obligar al servidor a conectarse a sistemas externos arbitrarios. Esto podría filtrar datos confidenciales, como credenciales de autorización.

## ¿Cuál es el impacto de los ataques de la SSRF?

Un ataque SSRF exitoso a menudo puede resultar en acciones no autorizadas o acceso a datos dentro de la organización. Esto puede ser en la aplicación vulnerable o en otros sistemas de back-end con los que la aplicación puede comunicarse. En algunas situaciones, la vulnerabilidad SSRF podría permitir a un atacante realizar la ejecución de comandos arbitrarios.

Un exploit SSRF que provoque conexiones a sistemas externos de terceros podría provocar ataques maliciosos. Puede parecer que estos se originan en la organización que aloja la aplicación vulnerable.

## Ataques comunes de la SSRF

Los ataques SSRF a menudo explotan las relaciones de confianza para escalar un ataque desde la aplicación vulnerable y realizar acciones no autorizadas. Estas relaciones de confianza pueden existir en relación con el servidor o con otros sistemas de back-end dentro de la misma organización.

## Ataques SSRF contra el servidor

En un ataque SSRF contra el servidor, el atacante hace que la aplicación realice una solicitud HTTP al servidor que aloja la aplicación, a través de su interfaz de red loopback. Por lo general, esto implica proporcionar una URL con un nombre de host como `127.0.0.1`(una dirección IP reservada que apunta al adaptador de bucle invertido) o `localhost`(un nombre de uso común para el mismo adaptador).

Por ejemplo, imagine una aplicación de compras que permite al usuario ver si un artículo está disponible en una tienda en particular. Para proporcionar información sobre las acciones, la aplicación debe consultar varias API REST de back-end. Para ello, pasa la URL al punto final API de back-end correspondiente a través de una solicitud HTTP de front-end. Cuando un usuario ve el estado del stock de un artículo, su navegador realiza la siguiente solicitud:

`POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1`

Esto hace que el servidor realice una solicitud a la URL especificada, recupere el estado del stock y lo devuelva al usuario.

En este ejemplo, un atacante puede modificar la solicitud para especificar una URL local al servidor:

`POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://localhost/admin`

El servidor recupera el contenido de la `/admin`URL y se lo devuelve al usuario.

Un atacante puede visitar la `/admin`URL, pero normalmente solo los usuarios autenticados pueden acceder a la funcionalidad administrativa. Esto significa que un atacante no verá nada de interés. Sin embargo, si la solicitud a la `/admin`URL proviene de la máquina local, se omiten los controles de acceso normales. La aplicación otorga acceso completo a la funcionalidad administrativa, porque la solicitud parece originarse en una ubicación confiable.

## Ataques SSRF contra otros sistemas back-end

En algunos casos, el servidor de aplicaciones puede interactuar con sistemas back-end a los que los usuarios no pueden acceder directamente. Estos sistemas suelen tener direcciones IP privadas no enrutables. Los sistemas back-end normalmente están protegidos por la topología de la red, por lo que a menudo tienen una postura de seguridad más débil. En muchos casos, los sistemas internos de back-end contienen funciones confidenciales a las que puede acceder sin autenticación cualquier persona que pueda interactuar con los sistemas.

En el ejemplo anterior, imagine que hay una interfaz administrativa en la URL de back-end `https://192.168.0.68/admin`. Un atacante puede enviar la siguiente solicitud para explotar la vulnerabilidad SSRF y acceder a la interfaz administrativa:

`POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://192.168.0.68/admin`


## Eludir las defensas comunes de la SSRF

Es común ver aplicaciones que contienen comportamiento SSRF junto con defensas destinadas a prevenir la explotación maliciosa. A menudo, estas defensas pueden eludirse.


## SSRF con filtros de entrada basados ​​en listas negras

Algunas aplicaciones bloquean entradas que contienen nombres de host como `127.0.0.1`y `localhost`o URL confidenciales como `/admin`. En esta situación, a menudo puedes eludir el filtro utilizando las siguientes técnicas:

- Utilice una representación IP alternativa de `127.0.0.1`, como `2130706433`, `017700000001`o `127.1`.
- Registre su propio nombre de dominio que resuelva `127.0.0.1`. Puedes utilizar `spoofed.burpcollaborator.net`para este propósito.
- Ofusque las cadenas bloqueadas mediante codificación de URL o variación de mayúsculas y minúsculas.
- Proporcione una URL que usted controle, que redireccione a la URL de destino. Intente utilizar diferentes códigos de redireccionamiento, así como diferentes protocolos para la URL de destino. Por ejemplo, se ha demostrado que cambiar de una URL `http:`a otra `https:`durante la redirección evita algunos filtros anti-SSRF.

## SSRF con filtros de entrada basados ​​en listas blancas

Algunas aplicaciones solo permiten entradas que coincidan con una lista blanca de valores permitidos. El filtro puede buscar una coincidencia al comienzo de la entrada o contenida en ella. Es posible que puedas evitar este filtro aprovechando las inconsistencias en el análisis de URL.

La especificación de URL contiene una serie de características que probablemente se pasen por alto cuando las URL implementen análisis y validación ad hoc mediante este método:

- Puede incrustar credenciales en una URL antes del nombre de host, utilizando el `@`carácter. Por ejemplo:
    
    `https://expected-host:fakepassword@evil-host`
- Puede utilizar el `#`carácter para indicar un fragmento de URL. Por ejemplo:
    
    `https://evil-host#expected-host`
- Puede aprovechar la jerarquía de nombres DNS para colocar la entrada requerida en un nombre DNS completo que usted controle. Por ejemplo:
    
    `https://expected-host.evil-host`
- Puede codificar caracteres en URL para confundir el código de análisis de URL. Esto es particularmente útil si el código que implementa el filtro maneja caracteres codificados en URL de manera diferente que el código que realiza la solicitud HTTP de fondo. También puedes probar con caracteres de doble codificación; algunos servidores decodifican recursivamente la URL de la entrada que reciben, lo que puede generar más discrepancias.
- Puedes utilizar combinaciones de estas técnicas juntas.


## Omitir filtros SSRF mediante redirección abierta

A veces es posible eludir las defensas basadas en filtros explotando una vulnerabilidad de redireccionamiento abierto.

En el ejemplo anterior, imagine que la URL enviada por el usuario está estrictamente validada para evitar la explotación maliciosa del comportamiento de la SSRF. Sin embargo, la aplicación cuyas URL están permitidas contiene una vulnerabilidad de redireccionamiento abierto. Siempre que la API utilizada para realizar la solicitud HTTP de back-end admita redirecciones, puede construir una URL que satisfaga el filtro y dé como resultado una solicitud redirigida al destino de back-end deseado.

Por ejemplo, la aplicación contiene una vulnerabilidad de redirección abierta en la que se muestra la siguiente URL:

`/product/nextProduct?currentProductId=6&path=http://evil-user.net`

devuelve una redirección a:

`http://evil-user.net`

Puede aprovechar la vulnerabilidad de redirección abierta para omitir el filtro de URL y explotar la vulnerabilidad SSRF de la siguiente manera:

`POST /product/stock HTTP/1.0 Content-Type: application/x-www-form-urlencoded Content-Length: 118 stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin`

Este exploit SSRF funciona porque la aplicación primero valida que la `stockAPI`URL proporcionada esté en un dominio permitido, y así es. Luego, la aplicación solicita la URL proporcionada, lo que activa la redirección abierta. Sigue la redirección y realiza una solicitud a la URL interna elegida por el atacante.


# Vulnerabilidades ciegas de la SSRF

Las vulnerabilidades ciegas de SSRF ocurren si puede hacer que una aplicación emita una solicitud HTTP de back-end a una URL proporcionada, pero la respuesta de la solicitud de back-end no se devuelve en la respuesta de front-end de la aplicación.

Blind SSRF es más difícil de explotar, pero a veces conduce a la ejecución remota completa de código en el servidor u otros componentes de back-end.

## ¿Cuál es el impacto de las vulnerabilidades ciegas de la SSRF?

El impacto de las vulnerabilidades ciegas de la SSRF es a menudo menor que el de las vulnerabilidades de la SSRF completamente informadas debido a su naturaleza unidireccional. No pueden explotarse trivialmente para recuperar datos confidenciales de sistemas back-end, aunque en algunas situaciones pueden explotarse para lograr la ejecución remota completa de código.

## Cómo encontrar y explotar vulnerabilidades ciegas de SSRF

La forma más confiable de detectar vulnerabilidades SSRF ciegas es utilizar técnicas fuera de banda (OAST). Esto implica intentar activar una solicitud HTTP a un sistema externo que usted controla y monitorear las interacciones de la red con ese sistema.

La forma más sencilla y eficaz de utilizar técnicas fuera de banda es utilizar Burp Collaborator. Puede utilizar Burp Collaborator para generar nombres de dominio únicos, enviarlos en cargas útiles a la aplicación y monitorear cualquier interacción con esos dominios. Si se observa una solicitud HTTP entrante proveniente de la aplicación, entonces es vulnerable a SSRF.

#### Nota

Cuando se prueban vulnerabilidades SSRF, es común observar una búsqueda de DNS para el dominio colaborador proporcionado, pero ninguna solicitud HTTP posterior. Esto suele ocurrir porque la aplicación intentó realizar una solicitud HTTP al dominio, lo que provocó la búsqueda DNS inicial, pero la solicitud HTTP real fue bloqueada por el filtrado a nivel de red. Es relativamente común que la infraestructura permita el tráfico DNS saliente, ya que es necesario para muchos propósitos, pero bloquee las conexiones HTTP a destinos inesperados.

## Cómo encontrar y explotar vulnerabilidades ciegas de SSRF - Continuación

La simple identificación de una vulnerabilidad SSRF ciega que puede desencadenar solicitudes HTTP fuera de banda no proporciona en sí misma una ruta hacia la explotabilidad. Dado que no puede ver la respuesta de la solicitud de back-end, el comportamiento no se puede utilizar para explorar contenido en sistemas a los que puede acceder el servidor de aplicaciones. Sin embargo, aún se puede aprovechar para buscar otras vulnerabilidades en el propio servidor o en otros sistemas back-end. Puede barrer ciegamente el espacio interno de direcciones IP y enviar cargas útiles diseñadas para detectar vulnerabilidades conocidas. Si esas cargas útiles también emplean técnicas ciegas fuera de banda, entonces podría descubrir una vulnerabilidad crítica en un servidor interno sin parches.

Otra vía para explotar las vulnerabilidades ciegas de SSRF es inducir a la aplicación a conectarse a un sistema bajo el control del atacante y devolver respuestas maliciosas al cliente HTTP que realiza la conexión. Si puede explotar una vulnerabilidad grave del lado del cliente en la implementación HTTP del servidor, es posible que pueda lograr la ejecución remota de código dentro de la infraestructura de la aplicación.


## Encontrar una superficie de ataque oculta para las vulnerabilidades SSRF

Muchas vulnerabilidades de falsificación de solicitudes del lado del servidor son fáciles de encontrar, porque el tráfico normal de la aplicación involucra parámetros de solicitud que contienen URL completas. Otros ejemplos de la SSRF son más difíciles de localizar.

## URL parciales en solicitudes

A veces, una aplicación coloca solo un nombre de host o parte de una ruta URL en los parámetros de solicitud. Luego, el valor enviado se incorpora del lado del servidor en una URL completa que se solicita. Si el valor se reconoce fácilmente como un nombre de host o una ruta URL, la posible superficie de ataque puede ser obvia. Sin embargo, la explotabilidad como SSRF completa puede estar limitada porque no se controla toda la URL que se solicita.

## URL dentro de formatos de datos

Algunas aplicaciones transmiten datos en formatos con una especificación que permite la inclusión de URL que el analizador de datos podría solicitar para el formato. Un ejemplo obvio de esto es el formato de datos XML, que se ha utilizado ampliamente en aplicaciones web para transmitir datos estructurados del cliente al servidor. Cuando una aplicación acepta datos en formato XML y los analiza, puede ser vulnerable a la inyección XXE. También podría ser vulnerable a la SSRF a través de XXE. Cubriremos esto con más detalle cuando analicemos las vulnerabilidades de inyección XXE.

## SSRF a través del encabezado Referer

Algunas aplicaciones utilizan software de análisis del lado del servidor para realizar un seguimiento de los visitantes. Este software a menudo registra el encabezado Referer en las solicitudes, para poder rastrear los enlaces entrantes. A menudo, el software de análisis visita las URL de terceros que aparecen en el encabezado del Referer. Por lo general, esto se hace para analizar el contenido de los sitios de referencia, incluido el texto ancla que se utiliza en los enlaces entrantes. Como resultado, el encabezado Referer suele ser una superficie de ataque útil para las vulnerabilidades SSRF.

