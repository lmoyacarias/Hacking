El CSRF es un tipo de exploit malicioso de un sitio web en el que comandos no autorizados son transmitidos por un usuario en el cual el sitio web confía.​ Esta vulnerabilidad es conocida también por otros nombres como XSRF, enlace hostil, ataque de un clic, secuestro de sesión, y ataque automático.

## Factores Claves para que se produzca un CSRF:

- **Una acción relevante.** Hay una acción dentro de la aplicación que el atacante tiene motivos para inducir. Esto podría ser una acción privilegiada (como modificar permisos para otros usuarios) o cualquier acción sobre datos específicos del usuario (como cambiar la propia contraseña del usuario).

- **Manejo de sesiones basado en cookies.** Realizar la acción implica emitir una o más solicitudes HTTP, y la aplicación se basa únicamente en cookies de sesión para identificar al usuario que realizó las solicitudes. No existe ningún otro mecanismo para rastrear sesiones o validar solicitudes de usuarios.

- **Sin parámetros de solicitud impredecibles.** Las solicitudes que realizan la acción no contienen ningún parámetro cuyos valores el atacante no pueda determinar o adivinar. Por ejemplo, cuando un usuario cambia su contraseña, la función no es vulnerable si un atacante necesita conocer el valor de la contraseña existente.


## Defensas Comunes contra CSRF:

Hoy en día, encontrar y explotar con éxito las vulnerabilidades CSRF a menudo implica eludir las medidas anti-CSRF implementadas por el sitio web de destino, el navegador de la víctima o ambos. Las defensas más comunes que encontrará son las siguientes:

- **Tokens CSRF** : un token CSRF es un valor único, secreto e impredecible que genera la aplicación del lado del servidor y se comparte con el cliente. Al intentar realizar una acción confidencial, como enviar un formulario, el cliente debe incluir el token CSRF correcto en la solicitud. Esto hace que sea muy difícil para un atacante elaborar una solicitud válida en nombre de la víctima.
    
- **Cookies de SameSite** : SameSite es un mecanismo de seguridad del navegador que determina cuándo las cookies de un sitio web se incluyen en las solicitudes que se originan en otros sitios web. Como las solicitudes para realizar acciones confidenciales generalmente requieren una cookie de sesión autenticada, las restricciones apropiadas de SameSite pueden impedir que un atacante active estas acciones entre sitios. Desde 2021, Chrome aplica `Lax`restricciones de SameSite de forma predeterminada. Como este es el estándar propuesto, esperamos que otros navegadores importantes adopten este comportamiento en el futuro.
    
- **Validación basada en referente** : algunas aplicaciones utilizan el encabezado HTTP Referer para intentar defenderse contra ataques CSRF, normalmente verificando que la solicitud se originó en el propio dominio de la aplicación. Generalmente, esto es menos efectivo que la validación de tokens CSRF.

## ¿Qué es un token CSRF?

Un token CSRF es un valor único, secreto e impredecible que genera la aplicación del lado del servidor y se comparte con el cliente. Al emitir una solicitud para realizar una acción confidencial, como enviar un formulario, el cliente debe incluir el token CSRF correcto. En caso contrario, el servidor se negará a realizar la acción solicitada.

Una forma común de compartir tokens CSRF con el cliente es incluirlos como parámetro oculto en un formulario HTML, por ejemplo:

`<form name="change-email-form" action="/my-account/change-email" method="POST"> <label>Email</label> <input required type="email" name="email" value="example@normal-website.com"> <input required type="hidden" name="csrf" value="50FaWgdOhi9M9wyna8taR1k3ODOR8d6u"> <button class='button' type='submit'> Update email </button> </form>
`
Cuando se implementan correctamente, los tokens CSRF ayudan a proteger contra ataques CSRF al dificultar que un atacante cree una solicitud válida en nombre de la víctima. Como el atacante no tiene forma de predecir el valor correcto del token CSRF, no podrá incluirlo en la solicitud maliciosa.

###### Nota
No es necesario enviar tokens CSRF como parámetros ocultos en una `POST`solicitud. Algunas aplicaciones colocan tokens CSRF en encabezados HTTP, por ejemplo. La forma en que se transmiten los tokens tiene un impacto significativo en la seguridad de un mecanismo en su conjunto. Para obtener más información, consulte Cómo prevenir vulnerabilidades CSRF.


# Defectos comunes en la validación de tokens CSRF

#### La validación del token CSRF depende del método de solicitud

Algunas aplicaciones validan correctamente el token cuando la solicitud utiliza el método POST pero omiten la validación cuando se utiliza el método GET.

En esta situación, el atacante puede cambiar al método GET para omitir la validación y lanzar un ataque CSRF:

`GET /email/change?email=pwned@evil-user.net HTTP/1.1 Host: vulnerable-website.com Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm`


### La validación del token CSRF depende de la presencia del token

Algunas aplicaciones validan correctamente el token cuando está presente, pero omiten la validación si se omite el token.

En esta situación, el atacante puede eliminar todo el parámetro que contiene el token (no solo su valor) para evitar la validación y lanzar un ataque CSRF:

`POST /email/change HTTP/1.1 Host: vulnerable-website.com Content-Type: application/x-www-form-urlencoded Content-Length: 25 Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm email=pwned@evil-user.net`

### El token CSRF no está vinculado a la sesión del usuario

Algunas aplicaciones no validan que el token pertenezca a la misma sesión que el usuario que realiza la solicitud. En cambio, la aplicación mantiene un grupo global de tokens que ha emitido y acepta cualquier token que aparezca en este grupo.

En esta situación, el atacante puede iniciar sesión en la aplicación usando su propia cuenta, obtener un token válido y luego enviar ese token al usuario víctima en su ataque CSRF.


### El token CSRF está vinculado a una cookie que no es de sesión

En una variación de la vulnerabilidad anterior, algunas aplicaciones vinculan el token CSRF a una cookie, pero no a la misma cookie que se utiliza para realizar un seguimiento de las sesiones. Esto puede ocurrir fácilmente cuando una aplicación emplea dos marcos diferentes, uno para el manejo de sesiones y otro para la protección CSRF, que no están integrados entre sí:

`POST /email/change HTTP/1.1 Host: vulnerable-website.com Content-Type: application/x-www-form-urlencoded Content-Length: 68 Cookie: session=pSJYSScWKpmC60LpFOAHKixuFuM4uXWF; csrfKey=rZHCnSzEp8dbI6atzagGoSYyqJqTz5dv csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY&email=wiener@normal-user.com`

### El token CSRF está vinculado a una cookie que no es de sesión - Continuación

Esta situación es más difícil de explotar pero sigue siendo vulnerable. Si el sitio web contiene algún comportamiento que permita a un atacante configurar una cookie en el navegador de la víctima, entonces es posible un ataque. El atacante puede iniciar sesión en la aplicación utilizando su propia cuenta, obtener un token válido y una cookie asociada, aprovechar el comportamiento de configuración de cookies para colocar su cookie en el navegador de la víctima y enviar su token a la víctima en su ataque CSRF.

#### Nota

Ni siquiera es necesario que el comportamiento de configuración de cookies exista dentro de la misma aplicación web que la vulnerabilidad CSRF. Cualquier otra aplicación dentro del mismo dominio DNS general puede aprovecharse para configurar cookies en la aplicación de destino, si la cookie controlada tiene un alcance adecuado. Por ejemplo, se podría aprovechar una función de configuración de cookies `staging.demo.normal-website.com`para colocar una cookie enviada a `secure.normal-website.com`.


### El token CSRF simplemente se duplica en una cookie

En una variación adicional de la vulnerabilidad anterior, algunas aplicaciones no mantienen ningún registro del lado del servidor de los tokens que se han emitido, sino que duplican cada token dentro de una cookie y un parámetro de solicitud. Cuando se valida la solicitud posterior, la aplicación simplemente verifica que el token enviado en el parámetro de solicitud coincida con el valor enviado en la cookie. Esto a veces se denomina defensa de "doble envío" contra CSRF y se recomienda porque es simple de implementar y evita la necesidad de cualquier estado del lado del servidor:

`POST /email/change HTTP/1.1 Host: vulnerable-website.com Content-Type: application/x-www-form-urlencoded Content-Length: 68 Cookie: session=1DQGdzYbOJQzLP7460tfyiv3do7MjyPw; csrf=R8ov2YBfTYmzFyjit8o2hKBuoIjXXVpa csrf=R8ov2YBfTYmzFyjit8o2hKBuoIjXXVpa&email=wiener@normal-user.com`

En esta situación, el atacante puede volver a realizar un ataque CSRF si el sitio web contiene alguna función de configuración de cookies. Aquí, el atacante no necesita obtener su propio token válido. Simplemente inventan un token (quizás en el formato requerido, si se está verificando), aprovechan el comportamiento de configuración de cookies para colocar su cookie en el navegador de la víctima y envían su token a la víctima en su ataque CSRF.


### Evitar las restricciones de cookies de SameSite

SameSite es un mecanismo de seguridad del navegador que determina cuándo las cookies de un sitio web se incluyen en las solicitudes que se originan en otros sitios web. Las restricciones de cookies de SameSite brindan protección parcial contra una variedad de ataques entre sitios, incluidos CSRF, filtraciones entre sitios y algunos exploits CORS.

Desde 2021, Chrome aplica `Lax`restricciones de SameSite de forma predeterminada si el sitio web que emite la cookie no establece explícitamente su propio nivel de restricción. Este es un estándar propuesto y esperamos que otros navegadores importantes adopten este comportamiento en el futuro. Como resultado, es esencial tener una comprensión sólida de cómo funcionan estas restricciones, así como de cómo potencialmente pueden evitarse, para poder probar exhaustivamente los vectores de ataque entre sitios.

En esta sección, primero cubriremos cómo funciona el mecanismo SameSite y aclararemos parte de la terminología relacionada. Luego veremos algunas de las formas más comunes en las que puede eludir estas restricciones, habilitando CSRF y otros ataques entre sitios en sitios web que inicialmente pueden parecer seguros.

# ¿Qué es un sitio en el contexto de las cookies de SameSite?

En el contexto de las restricciones de cookies de SameSite, un sitio se define como el dominio de nivel superior (TLD), generalmente algo así como `.com`o `.net`, más un nivel adicional del nombre de dominio. A esto se le suele denominar TLD+1.

Al determinar si una solicitud es del mismo sitio o no, también se tiene en cuenta el esquema de URL. Esto significa que la mayoría de los navegadores tratan un enlace desde `http://app.example.com`hacia como entre sitios.`https://app.example.com`

![[Pasted image 20240519213940.png]]

#### Nota

Es posible que se encuentre con el término "dominio de nivel superior efectivo" (eTLD). Esta es sólo una forma de tener en cuenta los sufijos reservados de varias partes que en la práctica se tratan como dominios de nivel superior, como `.co.uk`.

## ¿Cuál es la diferencia entre un sitio y un origen?

La diferencia entre un sitio y un origen es su alcance; un sitio abarca varios nombres de dominio, mientras que un origen solo incluye uno. Aunque están estrechamente relacionados, es importante no utilizar los términos indistintamente, ya que combinarlos puede tener graves implicaciones para la seguridad.

Se considera que dos URL tienen el mismo origen si comparten exactamente el mismo esquema, nombre de dominio y puerto. Aunque tenga en cuenta que el puerto a menudo se deduce del esquema.

![[Pasted image 20240519214048.png]]

Como puede ver en este ejemplo, el término "sitio" es mucho menos específico ya que solo representa el esquema y la última parte del nombre de dominio. Fundamentalmente, esto significa que una solicitud de origen cruzado aún puede ser del mismo sitio, pero no al revés.

|   |   |   |   |
|---|---|---|---|
|**Solicitud de**|**Solicitud de**|**¿Mismo sitio?**|**¿Mismo origen?**|
|`https://example.com`|`https://example.com`|Sí|Sí|
|`https://app.example.com`|`https://intranet.example.com`|Sí|No: nombre de dominio no coincidente|
|`https://example.com`|`https://example.com:8080`|Sí|No: puerto no coincidente|
|`https://example.com`|`https://example.co.uk`|No: eTLD no coincidente|No: nombre de dominio no coincidente|
|`https://example.com`|`http://example.com`|No: esquema no coincidente|No: esquema no coincidente|

Esta es una distinción importante, ya que significa que se puede abusar de cualquier vulnerabilidad que permita la ejecución arbitraria de JavaScript para eludir las defensas basadas en sitios en otros dominios que pertenecen al mismo sitio. Veremos un ejemplo de esto en uno de los laboratorios más adelante.

## ¿Cómo funciona SameSite?

Antes de que se introdujera el mecanismo SameSite, los navegadores enviaban cookies en cada solicitud al dominio que las emitía, incluso si la solicitud era activada por un sitio web de terceros no relacionado. SameSite funciona permitiendo a los navegadores y propietarios de sitios web limitar qué solicitudes entre sitios, si las hay, deben incluir cookies específicas. Esto puede ayudar a reducir la exposición de los usuarios a ataques CSRF, que inducen al navegador de la víctima a emitir una solicitud que desencadena una acción dañina en el sitio web vulnerable. Como estas solicitudes normalmente requieren una cookie asociada con la sesión autenticada de la víctima, el ataque fallará si el navegador no la incluye.

Actualmente, todos los principales navegadores admiten los siguientes niveles de restricción de SameSite:

- `Strict`
- `Lax`
- `None`

## ¿Cómo funciona SameSite? - Continuación

Los desarrolladores pueden configurar manualmente un nivel de restricción para cada cookie que establezcan, lo que les brinda más control sobre cuándo se utilizan estas cookies. Para ello, sólo tienen que incluir el `SameSite`atributo en el `Set-Cookie`encabezado de respuesta, junto con su valor preferido:

`Set-Cookie: session=0F8tgdOhi9ynR1M9wa3ODa; SameSite=Strict`

Aunque esto ofrece cierta protección contra ataques CSRF, ninguna de estas restricciones proporciona inmunidad garantizada, como lo demostraremos utilizando laboratorios interactivos deliberadamente vulnerables más adelante en esta sección.

#### Nota

Si el sitio web que emite la cookie no establece explícitamente un `SameSite`atributo, Chrome aplica `Lax`restricciones automáticamente de forma predeterminada. Esto significa que la cookie solo se envía en solicitudes entre sitios que cumplen con criterios específicos, aunque los desarrolladores nunca configuraron este comportamiento. Como se trata de un nuevo estándar propuesto, esperamos que otros navegadores importantes adopten este comportamiento en el futuro.

## Estricto

Si se configura una cookie con el `SameSite=Strict`atributo, los navegadores no la enviarán en ninguna solicitud entre sitios. En términos simples, esto significa que si el sitio de destino de la solicitud no coincide con el sitio que se muestra actualmente en la barra de direcciones del navegador, no incluirá la cookie.

Esto se recomienda al configurar cookies que permiten al portador modificar datos o realizar otras acciones sensibles, como acceder a páginas específicas que solo están disponibles para usuarios autenticados.

Aunque esta es la opción más segura, puede afectar negativamente la experiencia del usuario en los casos en que se desea la funcionalidad entre sitios.

## Flojo

`Lax`Las restricciones de SameSite significan que los navegadores enviarán la cookie en solicitudes entre sitios, pero solo si se cumplen las dos condiciones siguientes:

- La solicitud utiliza el `GET`método.
    
- La solicitud resultó de una navegación de nivel superior por parte del usuario, como hacer clic en un enlace.
    

`POST`Esto significa que la cookie no se incluye , por ejemplo, en las solicitudes entre sitios . Como `POST`las solicitudes se utilizan generalmente para realizar acciones que modifican datos o estados (al menos según las mejores prácticas), es mucho más probable que sean el objetivo de ataques CSRF.

Asimismo, la cookie no se incluye en solicitudes en segundo plano, como las iniciadas por scripts, iframes o referencias a imágenes y otros recursos.

## Ninguno

Si se configura una cookie con el `SameSite=None`atributo, esto efectivamente deshabilita las restricciones de SameSite por completo, independientemente del navegador. Como resultado, los navegadores enviarán esta cookie en todas las solicitudes al sitio que la emitió, incluso aquellas que fueron activadas por sitios de terceros completamente ajenos.

Con la excepción de Chrome, este es el comportamiento predeterminado utilizado por los principales navegadores si no `SameSite`se proporciona ningún atributo al configurar la cookie.

Existen razones legítimas para deshabilitar SameSite, como cuando la cookie está destinada a ser utilizada desde un contexto de terceros y no otorga al portador acceso a ningún dato o funcionalidad confidencial. Las cookies de seguimiento son un ejemplo típico.

## Ninguno - Continuación

Si encuentra un conjunto de cookies con `SameSite=None`o sin restricciones explícitas, vale la pena investigar si es de alguna utilidad. Cuando Chrome adoptó por primera vez el comportamiento "Lax-by-default", esto tuvo el efecto secundario de romper muchas funciones web existentes. Como solución rápida, algunos sitios web han optado por simplemente desactivar las restricciones de SameSite en todas las cookies, incluidas las potencialmente sensibles.

Al configurar una cookie con `SameSite=None`, el sitio web también debe incluir el `Secure`atributo, que garantiza que la cookie solo se envíe en mensajes cifrados a través de HTTPS. De lo contrario, los navegadores rechazarán la cookie y no se establecerá.

`Set-Cookie: trackingId=0F8tgdOhi9ynR1M9wa3ODa; SameSite=None; Secure`


# Como Evitar esta Seguridad (Hacer ByPassing)
#### Evitar las restricciones de SameSite Lax mediante solicitudes GET

En la práctica, los servidores no siempre se preocupan por recibir una solicitud `GET`o `POST`recibir un punto final determinado, incluso aquellos que esperan el envío de un formulario. Si también utilizan `Lax`restricciones para sus cookies de sesión, ya sea explícitamente o debido a la configuración predeterminada del navegador, es posible que aún pueda realizar un ataque CSRF al obtener una `GET`solicitud del navegador de la víctima.

Siempre que la solicitud implique una navegación de nivel superior, el navegador seguirá incluyendo la cookie de sesión de la víctima. El siguiente es uno de los métodos más sencillos para lanzar un ataque de este tipo:

`<script> document.location = 'https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000'; </script>`


#### Evitar las restricciones de SameSite Lax mediante solicitudes GET - Continuación

`GET`Incluso si no se permite una solicitud ordinaria , algunos marcos proporcionan formas de anular el método especificado en la línea de solicitud. Por ejemplo, Symfony admite el `_method`parámetro en formularios, que tiene prioridad sobre el método normal para fines de enrutamiento:

`<form action="https://vulnerable-website.com/account/transfer-payment" method="POST"> <input type="hidden" name="_method" value="GET"> <input type="hidden" name="recipient" value="hacker"> <input type="hidden" name="amount" value="1000000"> </form>`

Otros marcos admiten una variedad de parámetros similares.


#### Evitar las restricciones de SameSite utilizando gadgets en el sitio

Si se configura una cookie con el `SameSite=Strict`atributo, los navegadores no la incluirán en ninguna solicitud entre sitios. Es posible que puedas sortear esta limitación si encuentras un gadget que genere una solicitud secundaria dentro del mismo sitio.

Un posible dispositivo es una redirección del lado del cliente que construye dinámicamente el objetivo de la redirección utilizando entradas controlables por el atacante, como parámetros de URL. Para ver algunos ejemplos, consulte nuestros materiales sobre redirección abierta basada en DOM.

#### Cómo evitar las restricciones de SameSite usando gadgets en el sitio - Continuación

En lo que respecta a los navegadores, estos ***redireccionamientos*** del lado del cliente no son realmente ***redireccionamientos*** en absoluto; la solicitud resultante se trata simplemente como una solicitud ordinaria e independiente. Lo más importante es que se trata de una solicitud del mismo sitio y, como tal, incluirá todas las cookies relacionadas con el sitio, independientemente de las restricciones vigentes.

Si puede manipular este dispositivo para provocar una solicitud secundaria maliciosa, esto puede permitirle eludir por completo cualquier restricción de cookies de SameSite.

Tenga en cuenta que el ataque equivalente no es posible con ***redireccionamientos*** del lado del servidor. En este caso, los navegadores reconocen que la solicitud para seguir la redirección resultó inicialmente de una solicitud entre sitios, por lo que aún aplican las restricciones de cookies apropiadas.


#### Evitar las restricciones de SameSite a través de dominios hermanos vulnerables

Ya sea que esté probando el sitio web de otra persona o tratando de proteger el suyo propio, es esencial tener en cuenta que una solicitud aún puede ser del mismo sitio incluso si se emite con orígenes cruzados.

Asegúrese de auditar minuciosamente toda la superficie de ataque disponible, incluidos los dominios hermanos. En particular, las vulnerabilidades que le permiten provocar una solicitud secundaria arbitraria, como XSS, pueden comprometer completamente las defensas basadas en el sitio, exponiendo todos los dominios del sitio a ataques entre sitios.

Además del CSRF clásico, no olvide que si el sitio web de destino admite WebSockets, esta funcionalidad podría ser vulnerable al secuestro de WebSocket entre sitios (CSWSH), que es esencialmente solo un ataque CSRF dirigido a un protocolo de enlace de WebSocket. Para obtener más detalles, consulte nuestro tema sobre vulnerabilidades de WebSocket.

#### Evitar las restricciones de SameSite Lax con cookies recién emitidas

Las cookies con `Lax`restricciones de SameSite normalmente no se envían en ninguna `POST`solicitud entre sitios, pero existen algunas excepciones.

Como se mencionó anteriormente, si un sitio web no incluye un `SameSite`atributo al configurar una cookie, Chrome aplica `Lax`restricciones automáticamente de forma predeterminada. Sin embargo, para evitar romper los mecanismos de inicio de sesión único (SSO), en realidad no aplica estas restricciones durante los primeros 120 segundos en `POST`las solicitudes de nivel superior. Como resultado, hay una ventana de dos minutos en la que los usuarios pueden ser susceptibles a ataques entre sitios.

#### Nota

Esta ventana de dos minutos no se aplica a las cookies que se configuraron explícitamente con el `SameSite=Lax`atributo.

Es algo poco práctico intentar cronometrar el ataque para que caiga dentro de este breve período. Por otro lado, si puede encontrar un dispositivo en el sitio que le permita obligar a la víctima a recibir una nueva cookie de sesión, puede actualizar preventivamente su cookie antes de continuar con el ataque principal. Por ejemplo, completar un flujo de inicio de sesión basado en OAuth puede resultar en una nueva sesión cada vez, ya que el servicio OAuth no necesariamente sabe si el usuario todavía está conectado en el sitio de destino.

#### Cómo evitar las restricciones de SameSite Lax con cookies recién emitidas - Continuación

Para activar la actualización de las cookies sin que la víctima tenga que volver a iniciar sesión manualmente, debe utilizar una navegación de nivel superior, que garantiza que se incluyan las cookies asociadas con su sesión actual de OAuth. Esto plantea un desafío adicional porque luego necesita redirigir al usuario a su sitio para poder lanzar el ataque CSRF.

Alternativamente, puede activar la actualización de cookies desde una nueva pestaña para que el navegador no abandone la página antes de que pueda realizar el ataque final. Un inconveniente menor de este enfoque es que los navegadores bloquean las pestañas emergentes a menos que se abran mediante una interacción manual. Por ejemplo, el navegador bloqueará la siguiente ventana emergente de forma predeterminada:

`window.open('https://vulnerable-website.com/login/sso');`

Para solucionar esto, puede envolver la declaración en un `onclick`controlador de eventos de la siguiente manera:

`window.onclick = () => { window.open('https://vulnerable-website.com/login/sso'); }`

De esta manera, el `window.open()`método sólo se invoca cuando el usuario hace clic en algún lugar de la página.


### Evitar las defensas CSRF basadas en árbitros

Aparte de las defensas que emplean tokens CSRF, algunas aplicaciones utilizan el `Referer`encabezado HTTP para intentar defenderse contra ataques CSRF, normalmente verificando que la solicitud se originó en el propio dominio de la aplicación. Este enfoque es generalmente menos eficaz y a menudo está sujeto a desvíos.

#### encabezado de referencia

El encabezado HTTP Referer (que inadvertidamente está mal escrito en la especificación HTTP) es un encabezado de solicitud opcional que contiene la URL de la página web que enlaza con el recurso que se solicita. Por lo general, los navegadores lo agregan automáticamente cuando un usuario activa una solicitud HTTP, incluso al hacer clic en un enlace o enviar un formulario. Existen varios métodos que permiten a la página de enlace retener o modificar el valor del `Referer`encabezado. Esto suele hacerse por motivos de privacidad.

#### La validación del árbitro depende de que el encabezado esté presente

Algunas aplicaciones validan el `Referer`encabezado cuando está presente en las solicitudes, pero omiten la validación si se omite el encabezado.

En esta situación, un atacante puede diseñar su exploit CSRF de manera que haga que el navegador del usuario víctima elimine el `Referer`encabezado de la solicitud resultante. Hay varias formas de lograr esto, pero la más sencilla es usar una etiqueta META dentro de la página HTML que alberga el ataque CSRF:

`<meta name="referrer" content="never">`


#### La validación del Referer se puede eludir

Algunas aplicaciones validan el `Referer`encabezado de una manera ingenua que puede omitirse. Por ejemplo, si la aplicación valida que el dominio comienza `Referer`con el valor esperado, entonces el atacante puede colocarlo como un subdominio de su propio dominio:

`http://vulnerable-website.com.attacker-website.com/csrf-attack`

Del mismo modo, si la aplicación simplemente valida que `Referer`contiene su propio nombre de dominio, entonces el atacante puede colocar el valor requerido en otra parte de la URL:

`http://attacker-website.com/csrf-attack?vulnerable-website.com`

#### Nota

Aunque es posible que puedas identificar este comportamiento usando Burp, a menudo encontrarás que este enfoque ya no funciona cuando pruebas tu prueba de concepto en un navegador. En un intento por reducir el riesgo de que se filtren datos confidenciales de esta manera, muchos navegadores ahora eliminan la cadena de consulta del `Referer`encabezado de forma predeterminada.

Puede anular este comportamiento asegurándose de que la respuesta que contiene su exploit tenga el `Referrer-Policy: unsafe-url`encabezado configurado (tenga en cuenta que `Referrer`está escrito correctamente en este caso, ¡solo para asegurarse de que está prestando atención!). Esto garantiza que se enviará la URL completa, incluida la cadena de consulta.