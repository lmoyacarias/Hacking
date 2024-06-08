## ¿Qué es la autenticación?

La autenticación es el proceso de verificar la identidad de un usuario o cliente. Los sitios web están potencialmente expuestos a cualquier persona que esté conectada a Internet. Esto hace que los mecanismos de autenticación sólidos sean parte integral de una seguridad web eficaz.

##### Hay tres tipos principales de autenticación:

- Algo que **sabes** , como una contraseña o la respuesta a una pregunta de seguridad. A veces se les llama "factores de conocimiento".
- Algo que **tienes** , es un objeto físico como un teléfono móvil o un token de seguridad. A veces se les llama "factores de posesión".
- Algo que **eres** o haces. Por ejemplo, su biometría o patrones de comportamiento. A veces se les llama "factores de inherencia".

Los mecanismos de autenticación se basan en una variedad de tecnologías para verificar uno o más de estos factores.

## ¿Cuál es la diferencia entre autenticación y autorización?

La autenticación es el proceso de verificar que un usuario es quien dice ser. La autorización implica verificar si un usuario tiene permiso para hacer algo.

Por ejemplo, la autenticación determina si alguien que intenta acceder a un sitio web con el nombre de usuario `Carlos123`es realmente la misma persona que creó la cuenta.

Una vez `Carlos123`autenticado, sus permisos determinan qué está autorizado a hacer. Por ejemplo, pueden estar autorizados a acceder a información personal sobre otros usuarios o realizar acciones como eliminar la cuenta de otro usuario.

## ¿Cómo surgen las vulnerabilidades de autenticación?

La mayoría de las vulnerabilidades en los mecanismos de autenticación ocurren de dos maneras:

- Los mecanismos de autenticación son débiles porque no protegen adecuadamente contra ataques de fuerza bruta.
- Los fallos lógicos o una codificación deficiente en la implementación permiten que un atacante eluda por completo los mecanismos de autenticación. A esto a veces se le llama "autenticación rota".

En muchas áreas del desarrollo web, las fallas lógicas hacen que el sitio web se comporte inesperadamente, lo que puede ser o no un problema de seguridad. Sin embargo, como la autenticación es tan crítica para la seguridad, es muy probable que una lógica de autenticación defectuosa exponga el sitio web a problemas de seguridad.

## ¿Cuál es el impacto de la autenticación vulnerable?

El impacto de las vulnerabilidades de autenticación puede ser grave. Si un atacante elude la autenticación o ingresa por fuerza bruta a la cuenta de otro usuario, tiene acceso a todos los datos y funciones que tiene la cuenta comprometida. Si pueden comprometer una cuenta con altos privilegios, como la de un administrador de sistemas, podrían tomar el control total de toda la aplicación y potencialmente obtener acceso a la infraestructura interna.

## ¿Cuál es el impacto de la autenticación vulnerable? - Continuación

Incluso comprometer una cuenta con pocos privilegios podría otorgarle a un atacante acceso a datos que de otro modo no debería tener, como información comercial comercialmente confidencial. Incluso si la cuenta no tiene acceso a ningún dato confidencial, aún podría permitir al atacante acceder a páginas adicionales, lo que proporciona una superficie de ataque adicional. A menudo, los ataques de alta gravedad no son posibles desde páginas de acceso público, pero pueden ser posibles desde una página interna.

## Vulnerabilidades en el inicio de sesión basado en contraseña

Para los sitios web que adoptan un proceso de inicio de sesión basado en contraseña, los usuarios se registran ellos mismos para obtener una cuenta o un administrador les asigna una cuenta. Esta cuenta está asociada con un nombre de usuario único y una contraseña secreta, que el usuario ingresa en un formulario de inicio de sesión para autenticarse.

En este escenario, el hecho de que conozcan la contraseña secreta se toma como prueba suficiente de la identidad del usuario. Esto significa que la seguridad del sitio web se ve comprometida si un atacante puede obtener o adivinar las credenciales de inicio de sesión de otro usuario.

Esto se puede lograr de varias maneras. Las siguientes secciones muestran cómo un atacante puede utilizar ataques de fuerza bruta y algunas de las fallas en la protección de fuerza bruta. También aprenderá sobre las vulnerabilidades en la autenticación básica HTTP.

## Ataques de fuerza bruta

Un ataque de fuerza bruta se produce cuando un atacante utiliza un sistema de prueba y error para adivinar las credenciales de usuario válidas. Estos ataques suelen automatizarse mediante listas de palabras de nombres de usuario y contraseñas. Automatizar este proceso, especialmente utilizando herramientas dedicadas, potencialmente permite a un atacante realizar una gran cantidad de intentos de inicio de sesión a alta velocidad.

La fuerza bruta no siempre consiste simplemente en hacer conjeturas completamente aleatorias sobre nombres de usuarios y contraseñas. Al utilizar también lógica básica o conocimiento disponible públicamente, los atacantes pueden ajustar los ataques de fuerza bruta para hacer conjeturas mucho más fundamentadas. Esto aumenta considerablemente la eficacia de este tipo de ataques. Los sitios web que dependen del inicio de sesión basado en contraseña como único método para autenticar a los usuarios pueden ser muy vulnerables si no implementan suficiente protección de fuerza bruta.

## Nombres de usuario de fuerza bruta

Los nombres de usuario son especialmente fáciles de adivinar si siguen un patrón reconocible, como una dirección de correo electrónico. Por ejemplo, es muy común ver inicios de sesión comerciales en formato `firstname.lastname@somecompany.com`. Sin embargo, incluso si no existe un patrón obvio, a veces incluso cuentas con altos privilegios se crean utilizando nombres de usuario predecibles, como `admin`o `administrator`.

Durante la auditoría, verifique si el sitio web revela públicamente posibles nombres de usuario. Por ejemplo, ¿puedes acceder a los perfiles de usuario sin iniciar sesión? Incluso si el contenido real de los perfiles está oculto, el nombre utilizado en el perfil a veces es el mismo que el nombre de usuario de inicio de sesión. También debe verificar las respuestas HTTP para ver si se divulga alguna dirección de correo electrónico. En ocasiones, las respuestas contienen direcciones de correo electrónico de usuarios con altos privilegios, como administradores o soporte de TI.

## Contraseñas de fuerza bruta

De manera similar, las contraseñas pueden ser de fuerza bruta, y la dificultad varía según la seguridad de la contraseña. Muchos sitios web adoptan algún tipo de política de contraseñas, lo que obliga a los usuarios a crear contraseñas de alta entropía que son, al menos en teoría, más difíciles de descifrar utilizando únicamente la fuerza bruta. Por lo general, esto implica hacer cumplir las contraseñas con:

- Un número mínimo de caracteres.
- Una mezcla de letras minúsculas y mayúsculas.
- Al menos un carácter especial

## Contraseñas de fuerza bruta - Continuación

Sin embargo, si bien las contraseñas de alta entropía son difíciles de descifrar solo para las computadoras, podemos utilizar un conocimiento básico del comportamiento humano para explotar las vulnerabilidades que los usuarios introducen involuntariamente en este sistema. En lugar de crear una contraseña segura con una combinación aleatoria de caracteres, los usuarios a menudo toman una contraseña que pueden recordar e intentan manipularla para que se ajuste a la política de contraseñas. Por ejemplo, si `mypassword`no está permitido, los usuarios pueden probar algo como `Mypassword1!`o `Myp4$$w0rd`en su lugar.

En los casos en que la política exige que los usuarios cambien sus contraseñas periódicamente, también es común que los usuarios simplemente realicen cambios menores y predecibles en su contraseña preferida. Por ejemplo, `Mypassword1!`se convierte `Mypassword1?`o`Mypassword2!.`

Este conocimiento de las credenciales probables y los patrones predecibles significa que los ataques de fuerza bruta a menudo pueden ser mucho más sofisticados y, por lo tanto, efectivos, que simplemente iterar a través de cada combinación posible de personajes.

## Enumeración de nombre de usuario

La enumeración de nombres de usuario ocurre cuando un atacante puede observar cambios en el comportamiento del sitio web para identificar si un nombre de usuario determinado es válido.

La enumeración de nombres de usuario generalmente ocurre en la página de inicio de sesión, por ejemplo, cuando ingresa un nombre de usuario válido pero una contraseña incorrecta, o en los ***formularios de registro*** cuando ingresa un nombre de ***usuario que ya está en uso.*** Esto reduce en gran medida el tiempo y el esfuerzo necesarios para forzar un inicio de sesión porque el atacante puede generar rápidamente una lista corta de nombres de usuario válidos.

## Enumeración de nombres de usuario - Continuación

Al intentar forzar una página de inicio de sesión por fuerza bruta, debes prestar especial atención a las diferencias en:

- **Códigos de estado** : durante un ataque de fuerza bruta, es probable que el código de estado HTTP devuelto sea el mismo para la gran mayoría de las conjeturas porque la mayoría de ellas serán incorrectas. Si una suposición devuelve un código de estado diferente, esto es una fuerte indicación de que el nombre de usuario era correcto. Es una buena práctica que los sitios web devuelvan siempre el mismo código de estado independientemente del resultado, pero esta práctica no siempre se sigue.
- **Mensajes de error** : a veces el mensaje de error devuelto es diferente dependiendo de si tanto el nombre de usuario como la contraseña son incorrectos o solo la contraseña era incorrecta. Es una buena práctica que los sitios web utilicen mensajes genéricos idénticos en ambos casos, pero a veces se producen pequeños errores tipográficos. Solo un carácter fuera de lugar hace que los dos mensajes sean distintos, incluso en los casos en que el carácter no es visible en la página representada.
- **Tiempos de respuesta** : si la mayoría de las solicitudes se manejaron con un tiempo de respuesta similar, cualquiera que se desvíe de este sugiere que algo diferente estaba sucediendo detrás de escena. Esta es otra indicación de que el nombre de usuario adivinado podría ser correcto. Por ejemplo, es posible que un sitio web solo compruebe si la contraseña es correcta si el nombre de usuario es válido. Este paso adicional podría provocar un ligero aumento en el tiempo de respuesta. Esto puede ser sutil, pero un atacante puede hacer que este retraso sea más obvio ingresando una contraseña excesivamente larga que el sitio web tarda mucho más en procesar.

## Protección defectuosa contra fuerza bruta

Es muy probable que un ataque de fuerza bruta implique muchas conjeturas fallidas antes de que el atacante comprometa con éxito una cuenta. Lógicamente, la protección de fuerza bruta gira en torno a intentar que sea lo más complicado posible para automatizar el proceso y reducir la velocidad a la que un atacante puede intentar iniciar sesión. Las dos formas más comunes de prevenir ataques de fuerza bruta son:

- Bloquear la cuenta a la que el usuario remoto intenta acceder si realiza demasiados intentos fallidos de inicio de sesión
- Bloquear la dirección IP del usuario remoto si realiza demasiados intentos de inicio de sesión en rápida sucesión

Ambos enfoques ofrecen distintos grados de protección, pero ninguno es invulnerable, especialmente si se implementa utilizando una lógica defectuosa.

Por ejemplo, es posible que a veces descubras que tu IP está bloqueada si no inicias sesión demasiadas veces. ***En algunas implementaciones, el contador de la cantidad de intentos fallidos se reinicia si el propietario de la IP inicia sesión correctamente. Esto significa que un atacante simplemente tendría que iniciar sesión en su propia cuenta cada pocos intentos para evitar que se alcance este límite.***

En este caso, simplemente incluir sus propias credenciales de inicio de sesión a intervalos regulares en la lista de palabras es suficiente para que esta defensa sea prácticamente inútil.

## Bloqueo de cuenta

Una forma en que los sitios web intentan evitar la fuerza bruta es bloquear la cuenta si se cumplen ciertos criterios sospechosos, generalmente un número determinado de intentos fallidos de inicio de sesión. Al igual que con los errores de inicio de sesión normales, las respuestas del servidor que indican que una cuenta está bloqueada también pueden ayudar a un atacante a enumerar los nombres de usuario.

Bloquear una cuenta ofrece cierta protección contra la fuerza bruta dirigida a una cuenta específica. Sin embargo, este enfoque no logra prevenir adecuadamente los ataques de fuerza bruta en los que el atacante simplemente intenta obtener acceso a cualquier cuenta aleatoria que pueda.

## Bloqueo de cuenta - Continuación

Por ejemplo, se puede utilizar el siguiente método para solucionar este tipo de protección:

1. Establezca una lista de nombres de usuario candidatos que probablemente sean válidos. Esto podría realizarse mediante la enumeración de nombres de usuarios o simplemente basándose en una lista de nombres de usuarios comunes.
2. Decida una lista muy pequeña de contraseñas que crea que probablemente tenga al menos un usuario. Fundamentalmente, la cantidad de contraseñas que seleccione no debe exceder la cantidad de intentos de inicio de sesión permitidos. Por ejemplo, si ha calculado que el límite es de 3 intentos, deberá elegir un máximo de 3 intentos de contraseña.
3. Utilizando una herramienta como Burp Intruder, pruebe cada una de las contraseñas seleccionadas con cada uno de los nombres de usuario candidatos. De esta manera, puedes intentar aplicar fuerza bruta a cada cuenta sin activar el bloqueo de la cuenta. Solo necesita que un único usuario use una de las tres contraseñas para comprometer una cuenta.

## Bloqueo de cuenta - Continuación

El bloqueo de cuentas tampoco protege contra ataques de relleno de credenciales. Esto implica el uso de un diccionario masivo de `username:password`pares, compuesto por credenciales de inicio de sesión genuinas robadas en violaciones de datos. El relleno de credenciales se basa en el hecho de que muchas personas reutilizan el mismo nombre de usuario y contraseña en varios sitios web y, por lo tanto, existe la posibilidad de que algunas de las credenciales comprometidas en el diccionario también sean válidas en el sitio web de destino. El bloqueo de cuentas no protege contra el relleno de credenciales porque cada nombre de usuario solo se intenta una vez. El relleno de credenciales es particularmente peligroso porque a veces puede hacer que el atacante comprometa muchas cuentas diferentes con un solo ataque automatizado.

## Limitación de tasa de usuario

Otra forma en que los sitios web intentan prevenir ataques de fuerza bruta es mediante la limitación de la tasa de usuarios. En este caso, realizar demasiadas solicitudes de inicio de sesión en un corto período de tiempo provocará que se bloquee su dirección IP. Normalmente, la IP sólo se puede desbloquear de una de las siguientes maneras:

- Automáticamente después de que haya transcurrido un cierto período de tiempo
- Manualmente por un administrador
- Manualmente por el usuario después de completar exitosamente un CAPTCHA

A veces se prefiere la limitación de la tasa de usuario al bloqueo de cuentas debido a que es menos propenso a la enumeración de nombres de usuario y a ataques de denegación de servicio. Sin embargo, todavía no es completamente seguro. Como vimos en un ejemplo en una práctica de laboratorio anterior, hay varias formas en que un atacante puede manipular su IP aparente para evitar el bloqueo.

Como el límite se basa en la tasa de solicitudes HTTP enviadas desde la dirección IP del usuario, a veces también es posible evitar esta defensa si puedes descubrir cómo adivinar varias contraseñas con una sola solicitud.

## Autenticación básica HTTP

Aunque es bastante antiguo, su relativa simplicidad y facilidad de implementación significa que a veces es posible que veas que se utiliza la autenticación básica HTTP. En la autenticación básica HTTP, el cliente recibe un token de autenticación del servidor, que se construye concatenando el nombre de usuario y la contraseña y codificándolos en Base64. Este token lo almacena y administra el navegador, que lo agrega automáticamente al `Authorization`encabezado de cada solicitud posterior de la siguiente manera:

`Authorization: Basic base64(username:password)`

## Autenticación básica HTTP: continuación

Por diversas razones, esto generalmente no se considera un método de autenticación seguro. En primer lugar, implica enviar repetidamente las credenciales de inicio de sesión del usuario con cada solicitud. A menos que el sitio web también implemente HSTS, las credenciales de usuario están abiertas a ser capturadas en un ataque de intermediario.

Además, las implementaciones de autenticación básica HTTP a menudo no admiten la protección de fuerza bruta. Como el token consta exclusivamente de valores estáticos, esto puede dejarlo vulnerable a la fuerza bruta.

La autenticación básica HTTP también es particularmente vulnerable a exploits relacionados con la sesión, en particular CSRF, contra el cual no ofrece protección por sí solo.

En algunos casos, explotar la autenticación básica HTTP vulnerable solo puede otorgarle a un atacante acceso a una página aparentemente poco interesante. Sin embargo, además de proporcionar una superficie de ataque adicional, las credenciales expuestas de esta manera podrían reutilizarse en otros contextos más confidenciales.

## Vulnerabilidades en la autenticación multifactor

En esta sección, veremos algunas de las vulnerabilidades que pueden ocurrir en los mecanismos de autenticación multifactor. También proporcionamos varios laboratorios interactivos para demostrar cómo se pueden aprovechar estas vulnerabilidades en la autenticación multifactor.

Muchos sitios web dependen exclusivamente de la autenticación de un solo factor mediante una contraseña para autenticar a los usuarios. Sin embargo, algunos requieren que los usuarios demuestren su identidad mediante múltiples factores de autenticación.

## Vulnerabilidades en la autenticación multifactor - Continuación

Verificar los factores biométricos no es práctico para la mayoría de los sitios web. Sin embargo, es cada vez más común ver autenticación de dos factores (2FA) tanto obligatoria como opcional basada en **algo que sabes** y **algo que tienes** . Por lo general, esto requiere que los usuarios ingresen una contraseña tradicional y un código de verificación temporal desde un dispositivo físico fuera de banda que tengan en su poder.

Si bien a veces es posible que un atacante obtenga un único factor basado en el conocimiento, como una contraseña, es considerablemente menos probable que pueda obtener simultáneamente otro factor de una fuente fuera de banda. Por este motivo, se ha demostrado que la autenticación de dos factores es más segura que la autenticación de un solo factor. Sin embargo, como ocurre con cualquier medida de seguridad, su seguridad sólo es tan segura como su implementación. La autenticación de dos factores mal implementada puede ser superada, o incluso omitida por completo, al igual que la autenticación de un solo factor.

También vale la pena señalar que todos los beneficios de la autenticación multifactor solo se logran verificando múltiples factores **diferentes** . Verificar el mismo factor de dos maneras diferentes no es una verdadera autenticación de dos factores. La 2FA basada en correo electrónico es un ejemplo de ello. Aunque el usuario debe proporcionar una contraseña y un código de verificación, acceder al código solo depende de que conozca las credenciales de inicio de sesión de su cuenta de correo electrónico. Por lo tanto, el factor de autenticación de conocimientos simplemente se verifica dos veces.

## Tokens de autenticación de dos factores

Los códigos de verificación generalmente los lee el usuario desde un dispositivo físico de algún tipo. Muchos sitios web de alta seguridad ahora brindan a los usuarios un dispositivo dedicado para este propósito, como el token RSA o el dispositivo con teclado que puede usar para acceder a su banca en línea o a su computadora portátil de trabajo. Además de estar diseñados específicamente para la seguridad, estos dispositivos dedicados también tienen la ventaja de generar el código de verificación directamente. También es común que los sitios web utilicen una aplicación móvil dedicada, como Google Authenticator, por el mismo motivo.

Por otro lado, algunos sitios web envían códigos de verificación al teléfono móvil del usuario como mensaje de texto. Si bien técnicamente esto sigue verificando el factor de "algo que tienes", está abierto a abusos. En primer lugar, el código se transmite a través de SMS en lugar de ser generado por el propio dispositivo. Esto crea la posibilidad de que el código sea interceptado. También existe el riesgo de intercambio de SIM, mediante el cual un atacante obtiene de manera fraudulenta una tarjeta SIM con el número de teléfono de la víctima. Luego, el atacante recibiría todos los mensajes SMS enviados a la víctima, incluido el que contiene su código de verificación.

## Omitir la autenticación de dos factores

En ocasiones, la implementación de la autenticación de dos factores es defectuosa hasta el punto de que se puede omitir por completo.

Si primero se le solicita al usuario que ingrese una contraseña y luego se le solicita que ingrese un código de verificación en una página separada, el usuario se encuentra efectivamente en un estado de "iniciar sesión" antes de ingresar el código de verificación. En este caso, vale la pena probar para ver si puede pasar directamente a las páginas "solo para quienes han iniciado sesión" después de completar el primer paso de autenticación. Ocasionalmente, encontrará que un sitio web en realidad no verifica si completó o no el segundo paso antes de cargar la página.


## Lógica de verificación de dos factores defectuosa

A veces, la lógica defectuosa en la autenticación de dos factores significa que después de que un usuario ha completado el paso de inicio de sesión inicial, el sitio web no verifica adecuadamente que el mismo usuario esté completando el segundo paso.

Por ejemplo, el usuario inicia sesión con sus credenciales normales en el primer paso de la siguiente manera:

`POST /login-steps/first HTTP/1.1 Host: vulnerable-website.com ... username=carlos&password=qwerty`

Luego se les asigna una cookie relacionada con su cuenta, antes de pasar al segundo paso del proceso de inicio de sesión:

`HTTP/1.1 200 OK Set-Cookie: account=carlos GET /login-steps/second HTTP/1.1 Cookie: account=carlos`

Al enviar el código de verificación, la solicitud utiliza esta cookie para determinar a qué cuenta intenta acceder el usuario:

`POST /login-steps/second HTTP/1.1 Host: vulnerable-website.com Cookie: account=carlos ... verification-code=123456`

En este caso, un atacante podría iniciar sesión con sus propias credenciales pero luego cambiar el valor de la `account`cookie a cualquier nombre de usuario arbitrario al enviar el código de verificación.

`POST /login-steps/second HTTP/1.1 Host: vulnerable-website.com Cookie: account=victim-user ... verification-code=123456`

Esto es extremadamente peligroso si el atacante puede aplicar fuerza bruta al código de verificación, ya que le permitiría iniciar sesión en cuentas de usuarios arbitrarios basándose exclusivamente en su nombre de usuario. Ni siquiera necesitarían saber la contraseña del usuario.

## Códigos de verificación 2FA de fuerza bruta

Al igual que con las contraseñas, los sitios web deben tomar medidas para evitar la fuerza bruta del código de verificación 2FA. Esto es especialmente importante porque el código suele ser un número simple de 4 o 6 dígitos. Sin una protección adecuada contra la fuerza bruta, descifrar dicho código es trivial.

Algunos sitios web intentan evitar esto cerrando automáticamente la sesión de un usuario si ingresa una cierta cantidad de códigos de verificación incorrectos. Esto es ineficaz en la práctica porque un atacante avanzado puede incluso automatizar este proceso de varios pasos creando macros para Burp Intruder. La extensión Turbo Intruder también se puede utilizar para este propósito.

## Vulnerabilidades en otros mecanismos de autenticación

Además de la funcionalidad básica de inicio de sesión, la mayoría de los sitios web ofrecen funciones adicionales para permitir a los usuarios administrar su cuenta. Por ejemplo, los usuarios normalmente pueden cambiar su contraseña o restablecerla cuando la olvidan. Estos mecanismos también pueden introducir vulnerabilidades que un atacante puede aprovechar.

Los sitios web suelen tener cuidado de evitar vulnerabilidades conocidas en sus páginas de inicio de sesión. Pero es fácil pasar por alto el hecho de que es necesario tomar medidas similares para garantizar que la funcionalidad relacionada sea igualmente sólida. Esto es especialmente importante en los casos en los que un atacante puede crear su propia cuenta y, en consecuencia, tiene fácil acceso para estudiar estas páginas adicionales.

## Mantener a los usuarios conectados

Una característica común es la opción de permanecer conectado incluso después de cerrar la sesión del navegador. Suele ser una casilla de verificación simple con la etiqueta "Recordarme" o "Mantenerme conectado".

Esta funcionalidad a menudo se implementa generando un token "recordarme" de algún tipo, que luego se almacena en una cookie persistente. Como poseer esta cookie de manera efectiva le permite omitir todo el proceso de inicio de sesión, es una buena práctica que no sea práctico adivinar esta cookie. Sin embargo, algunos sitios web generan esta cookie basándose en una concatenación predecible de valores estáticos, como el nombre de usuario y una marca de tiempo. Algunos incluso utilizan la contraseña como parte de la cookie. Este enfoque es particularmente peligroso si un atacante puede crear su propia cuenta porque puede estudiar su propia cookie y potencialmente deducir cómo se genera. Una vez que hayan resuelto la fórmula, pueden intentar forzar las cookies de otros usuarios por fuerza bruta para obtener acceso a sus cuentas.

## Mantener a los usuarios conectados - Continuación

Algunos sitios web asumen que si la cookie está cifrada de alguna manera, no será adivinable incluso si utiliza valores estáticos. Si bien esto puede ser cierto si se hace correctamente, "cifrar" ingenuamente la cookie utilizando una codificación bidireccional simple como Base64 no ofrece protección alguna. Incluso el uso de un cifrado adecuado con una función hash unidireccional no es completamente infalible. Si el atacante puede identificar fácilmente el algoritmo de hash y no se utiliza sal, puede potencialmente forzar la cookie con fuerza bruta simplemente aplicando hash en sus listas de palabras. Este método se puede utilizar para evitar los límites de intentos de inicio de sesión si no se aplica un límite similar a las suposiciones de cookies.

Incluso si el atacante no puede crear su propia cuenta, es posible que aún pueda aprovechar esta vulnerabilidad. Usando las técnicas habituales, como XSS, un atacante podría robar la cookie "recordarme" de otro usuario y deducir cómo se construye la cookie a partir de eso. Si el sitio web se creó utilizando un marco de código abierto, los detalles clave de la construcción de las cookies pueden incluso documentarse públicamente.

## Mantener a los usuarios conectados - Continuación

En algunos casos raros, es posible obtener la contraseña real de un usuario en texto sin cifrar a partir de una cookie, incluso si está codificada con hash. Las versiones hash de listas de contraseñas conocidas están disponibles en línea, por lo que si la contraseña del usuario aparece en una de estas listas, descifrar el hash en ocasiones puede ser tan trivial como simplemente pegar el hash en un motor de búsqueda. Esto demuestra la importancia de la sal en un cifrado eficaz.

## Restablecer contraseñas de usuario

En la práctica, algunos usuarios olvidarán su contraseña, por lo que es común tener una forma de restablecerla. Como la autenticación habitual basada en contraseña es obviamente imposible en este escenario, los sitios web tienen que confiar en métodos alternativos para asegurarse de que el usuario real restablezca su propia contraseña. Por este motivo, la función de restablecimiento de contraseña es intrínsecamente peligrosa y debe implementarse de forma segura.

Hay algunas formas diferentes en que se implementa comúnmente esta característica, con distintos grados de vulnerabilidad.

## Envío de contraseñas por correo electrónico

No hace falta decir que enviar a los usuarios su contraseña actual nunca debería ser posible si, en primer lugar, un sitio web maneja las contraseñas de forma segura. En cambio, algunos sitios web generan una nueva contraseña y la envían al usuario por correo electrónico.

En términos generales, se debe evitar el envío de contraseñas persistentes a través de canales inseguros. En este caso, la seguridad depende de que la contraseña generada caduque después de un período muy corto o de que el usuario vuelva a cambiar su contraseña inmediatamente. De lo contrario, este enfoque es muy susceptible a ataques de intermediario.

Por lo general, el correo electrónico tampoco se considera seguro, dado que las bandejas de entrada son persistentes y no están realmente diseñadas para el almacenamiento seguro de información confidencial. Muchos usuarios también sincronizan automáticamente su bandeja de entrada entre múltiples dispositivos a través de canales inseguros.

## Restablecer contraseñas usando una URL

Un método más sólido para restablecer contraseñas es enviar una URL única a los usuarios que los lleve a una página de restablecimiento de contraseña. Las implementaciones menos seguras de este método utilizan una URL con un parámetro fácilmente adivinable para identificar qué cuenta se está restableciendo, por ejemplo:

`http://vulnerable-website.com/reset-password?user=victim-user`

En este ejemplo, un atacante podría cambiar el `user`parámetro para hacer referencia a cualquier nombre de usuario que haya identificado. Luego serán llevados directamente a una página donde potencialmente pueden establecer una nueva contraseña para este usuario arbitrario.

## Restablecer contraseñas usando una URL - Continuación

Una mejor implementación de este proceso es generar un token de alta entropía difícil de adivinar y crear la URL de reinicio en base a eso. En el mejor de los casos, esta URL no debería proporcionar pistas sobre qué contraseña de usuario se está restableciendo.

`http://vulnerable-website.com/reset-password?token=a0ba0d1cb3b63d13822572fcff1a241895d893f659164d4cc550b421ebdd48a8`

Cuando el usuario visita esta URL, el sistema debe verificar si este token existe en el back-end y, de ser así, qué contraseña de usuario se supone que debe restablecer. Este token debería caducar después de un corto período de tiempo y destruirse inmediatamente después de que se haya restablecido la contraseña.

Sin embargo, algunos sitios web no vuelven a validar el token cuando se envía el formulario de restablecimiento. En este caso, un atacante podría simplemente visitar el formulario de restablecimiento desde su propia cuenta, eliminar el token y aprovechar esta página para restablecer la contraseña de un usuario arbitrario.

## Restablecer contraseñas usando una URL - Continuación

Si la URL en el correo electrónico de restablecimiento se genera dinámicamente, esto también puede ser vulnerable a un envenenamiento por restablecimiento de contraseña. En este caso, un atacante puede potencialmente robar el token de otro usuario y utilizarlo para cambiar su contraseña.

## Cambiar contraseñas de usuario

Normalmente, cambiar su contraseña implica ingresar su contraseña actual y luego la nueva contraseña dos veces. Estas páginas se basan fundamentalmente en el mismo proceso para verificar que los nombres de usuario y las contraseñas actuales coincidan como lo hace una página de inicio de sesión normal. Por tanto, estas páginas pueden ser vulnerables a las mismas técnicas.

La funcionalidad de cambio de contraseña puede ser particularmente peligrosa si permite que un atacante acceda a ella directamente sin iniciar sesión como usuario víctima. Por ejemplo, si el nombre de usuario se proporciona en un campo oculto, un atacante podría editar este valor en la solicitud para apuntar a usuarios arbitrarios. Potencialmente, esto puede explotarse para enumerar nombres de usuarios y contraseñas de fuerza bruta.

# Prevención de ataques a sus propios mecanismos de autenticación

Hemos demostrado varias formas en las que los sitios web pueden ser vulnerables debido a cómo implementan la autenticación. Para reducir el riesgo de este tipo de ataques en sus propios sitios web, existen varios principios que siempre debe intentar seguir.

## Cuidado con las credenciales de usuario

Incluso los mecanismos de autenticación más sólidos son ineficaces si, sin saberlo, revela un conjunto válido de credenciales de inicio de sesión a un atacante. No hace falta decir que nunca debes enviar datos de inicio de sesión a través de conexiones no cifradas. Aunque es posible que haya implementado HTTPS para sus solicitudes de inicio de sesión, asegúrese de aplicarlo redirigiendo cualquier intento de solicitud HTTP a HTTPS también.

También debe auditar su sitio web para asegurarse de que ningún nombre de usuario o dirección de correo electrónico se divulgue a través de perfiles de acceso público ni se refleje en respuestas HTTP, por ejemplo.

## No cuente con los usuarios por seguridad

Las medidas de autenticación estrictas a menudo requieren un esfuerzo adicional por parte de los usuarios. La naturaleza humana hace que sea casi inevitable que algunos usuarios encuentren formas de ahorrarse este esfuerzo. Por lo tanto, es necesario imponer un comportamiento seguro siempre que sea posible.

El ejemplo más obvio es implementar una política de contraseñas eficaz. Algunas de las políticas más tradicionales fracasan porque las personas introducen sus propias contraseñas predecibles en la política. En cambio, puede ser más efectivo implementar un verificador de contraseñas simple de algún tipo, que permita a los usuarios experimentar con contraseñas y proporcione retroalimentación sobre su solidez en tiempo real. Un ejemplo popular es la biblioteca JavaScript `zxcvbn`, desarrollada por Dropbox. Al permitir únicamente contraseñas que tengan una calificación alta según el verificador de contraseñas, puede imponer el uso de contraseñas seguras de manera más efectiva que con las políticas tradicionales.

## Evitar la enumeración de nombres de usuario

Es considerablemente más fácil para un atacante romper sus mecanismos de autenticación si revela que existe un usuario en el sistema. Incluso hay determinadas situaciones en las que, debido a la naturaleza del sitio web, el conocimiento de que una persona concreta tiene una cuenta es información sensible en sí misma.

Independientemente de si un intento de nombre de usuario es válido, es importante utilizar mensajes de error genéricos idénticos y asegurarse de que realmente sean idénticos. Siempre debes devolver el mismo código de estado HTTP con cada solicitud de inicio de sesión y, finalmente, hacer que los tiempos de respuesta en diferentes escenarios sean lo más indistinguibles posible.

## Implementar una sólida protección contra la fuerza bruta

Dado lo simple que puede ser construir un ataque de fuerza bruta, es vital asegurarse de tomar medidas para prevenir, o al menos interrumpir, cualquier intento de inicio de sesión por fuerza bruta.

Uno de los métodos más eficaces es implementar una limitación estricta de la tasa de usuarios basada en IP. Esto debería implicar medidas para evitar que los atacantes manipulen su dirección IP aparente. Idealmente, debería exigir al usuario que complete una prueba CAPTCHA con cada intento de inicio de sesión después de alcanzar un cierto límite.

Tenga en cuenta que no se garantiza que esto elimine por completo la amenaza de la fuerza bruta. Sin embargo, hacer que el proceso sea lo más tedioso y manual posible aumenta la probabilidad de que cualquier posible atacante se dé por vencido y busque un objetivo más fácil.

## Verifique tres veces su lógica de verificación

Como lo demostraron nuestros laboratorios, es fácil que se introduzcan fallas lógicas simples en el código que, en el caso de la autenticación, tienen el potencial de comprometer completamente su sitio web y a sus usuarios. Auditar minuciosamente cualquier lógica de verificación o validación para eliminar fallas es absolutamente clave para una autenticación sólida. Un control que se puede eludir no es, en última instancia, mucho mejor que ningún control.

## No olvides la funcionalidad complementaria

Asegúrese de no centrarse únicamente en las páginas de inicio de sesión centrales y pasar por alto funciones adicionales relacionadas con la autenticación. Esto es particularmente importante en los casos en que el atacante es libre de registrar su propia cuenta y explorar esta funcionalidad. Recuerde que restablecer o cambiar una contraseña es una superficie de ataque tan válida como el mecanismo de inicio de sesión principal y, en consecuencia, debe ser igualmente sólido.

## Implementar una autenticación multifactor adecuada

Si bien la autenticación multifactor puede no ser práctica para todos los sitios web, cuando se realiza correctamente es mucho más segura que el inicio de sesión basado únicamente en una contraseña. Recuerde que verificar varias instancias del mismo factor no es una verdadera autenticación multifactor. Enviar códigos de verificación por correo electrónico es esencialmente una forma más extensa de autenticación de un solo factor.

La 2FA basada en SMS técnicamente verifica dos factores (algo que sabes y algo que tienes). Sin embargo, la posibilidad de que se produzcan abusos mediante el intercambio de SIM, por ejemplo, significa que este sistema puede resultar poco fiable.

Idealmente, la 2FA debería implementarse utilizando un dispositivo o aplicación dedicada que genere el código de verificación directamente. Como están diseñados específicamente para brindar seguridad, suelen ser más seguros.

Finalmente, al igual que con la lógica de autenticación principal, asegúrese de que la lógica en sus comprobaciones 2FA sea sólida para que no pueda omitirse fácilmente.

