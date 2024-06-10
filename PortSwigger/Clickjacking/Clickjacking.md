
## ¿Qué es el clickjacking?

Clickjacking es un ataque basado en interfaz en el que se engaña a un usuario para que haga clic en contenido procesable en un sitio web oculto haciendo clic en algún otro contenido en un sitio web señuelo. Considere el siguiente ejemplo:

Un usuario web accede a un sitio web señuelo (quizás sea un enlace proporcionado por un correo electrónico) y hace clic en un botón para ganar un premio. Sin saberlo, un atacante los engañó para que presionen un botón oculto alternativo y esto resulta en el pago de una cuenta en otro sitio. Este es un ejemplo de un ataque de clickjacking. La técnica depende de la incorporación de una página web invisible y procesable (o varias páginas) que contenga un botón o un enlace oculto, por ejemplo, dentro de un iframe. El iframe se superpone sobre el contenido de la página web señuelo previsto por el usuario. Este ataque se diferencia de un ataque CSRF en que se requiere que el usuario realice una acción como hacer clic en un botón, mientras que un ataque CSRF depende de falsificar una solicitud completa sin el conocimiento o la participación del usuario.

![[Pasted image 20240609181238.png]]

## ¿Qué es el clickjacking? - Continuación

La protección contra ataques CSRF a menudo se proporciona mediante el uso de un token CSRF: un número o nonce específico de la sesión y de un solo uso. Los ataques de clickjacking no se mitigan con el token CSRF, ya que se establece una sesión de destino con contenido cargado desde un sitio web auténtico y con todas las solicitudes que se realizan en el dominio. Los tokens CSRF se colocan en solicitudes y se pasan al servidor como parte de una sesión de comportamiento normal. La diferencia con respecto a una sesión de usuario normal es que el proceso ocurre dentro de un iframe oculto.

## Cómo construir un ataque de clickjacking básico

Los ataques de clickjacking utilizan CSS para crear y manipular capas. El atacante incorpora el sitio web objetivo como una capa de iframe superpuesta al sitio web señuelo. Un ejemplo que utiliza la etiqueta de estilo y los parámetros es el siguiente:

`<head> <style> #target_website { position:relative; width:128px; height:128px; opacity:0.00001; z-index:2; } #decoy_website { position:absolute; width:300px; height:400px; z-index:1; } </style> </head> ... <body> <div id="decoy_website"> ...decoy web content here... </div> <iframe id="target_website" src="https://vulnerable-website.com"> </iframe> </body>`

![[Pasted image 20240609181321.png]]

## Cómo construir un ataque de clickjacking básico - Continuación

El iframe del sitio web de destino se coloca dentro del navegador de modo que haya una superposición precisa de la acción de destino con el sitio web señuelo utilizando valores de posición de ancho y alto adecuados. Se utilizan valores de posición absolutos y relativos para garantizar que el sitio web de destino se superponga con precisión al señuelo, independientemente del tamaño de la pantalla, el tipo de navegador y la plataforma. El índice z determina el orden de apilamiento de las capas del iframe y del sitio web. El valor de opacidad se define como 0,0 (o cerca de 0,0) para que el contenido del iframe sea transparente para el usuario. La protección contra el clickjacking del navegador puede aplicar una detección de transparencia de iframe basada en umbrales (por ejemplo, Chrome versión 76 incluye este comportamiento, pero Firefox no). El atacante selecciona valores de opacidad para lograr el efecto deseado sin activar comportamientos de protección.

## Clickbandit

Aunque puede crear manualmente una prueba de concepto de clickjacking como se describe anteriormente, esto puede resultar bastante tedioso y llevar mucho tiempo en la práctica. Cuando estés probando el clickjacking en la naturaleza, te recomendamos usar la herramienta Clickbandit de Burp. Esto le permite utilizar su navegador para realizar las acciones deseadas en la página que se puede enmarcar y luego crea un archivo HTML que contiene una superposición de clickjacking adecuada. Puedes utilizar esto para generar una prueba de concepto interactiva en cuestión de segundos, sin tener que escribir una sola línea de HTML o CSS.

## Clickjacking con entrada de formulario precargado

Algunos sitios web que requieren completar y enviar formularios permiten completar previamente las entradas del formulario utilizando parámetros GET antes del envío. Otros sitios web pueden requerir texto antes de enviar el formulario. Como los valores GET forman parte de la URL, la URL de destino se puede modificar para incorporar valores elegidos por el atacante y el botón transparente "enviar" se superpone en el sitio señuelo como en el ejemplo básico de clickjacking.

## Guiones para romper marcos

Los ataques de clickjacking son posibles siempre que los sitios web se puedan enmarcar. Por tanto, las técnicas preventivas se basan en restringir la capacidad de enmarcado de los sitios web. Una protección común del lado del cliente implementada a través del navegador web es utilizar secuencias de comandos de ruptura de fotogramas o de ruptura de fotogramas. Estos se pueden implementar a través de complementos o extensiones de JavaScript del navegador propietario, como NoScript. Los scripts suelen diseñarse para que realicen algunos o todos los siguientes comportamientos:

- comprobar y hacer cumplir que la ventana de la aplicación actual sea la ventana principal o superior,
- hacer visibles todos los marcos,
- evitar hacer clic en marcos invisibles,
- interceptar y señalar posibles ataques de clickjacking al usuario.

## Scripts para romper cuadros - Continuación

Las técnicas de destrucción de marcos suelen ser específicas del navegador y de la plataforma y, debido a la flexibilidad del HTML, los atacantes normalmente pueden eludirlas. Como los frame busters son JavaScript, la configuración de seguridad del navegador puede impedir su funcionamiento o, de hecho, es posible que el navegador ni siquiera admita JavaScript. Una solución eficaz para los atacantes contra los destructores de marcos es utilizar el `sandbox`atributo iframe HTML5. Cuando esto se establece con los valores `allow-forms`o `allow-scripts`y se omite el `allow-top-navigation`valor, entonces el script de destrucción de cuadros se puede neutralizar ya que el iframe no puede verificar si es o no la ventana superior:

`<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>`

Tanto los valores `allow-forms`como `allow-scripts`permiten las acciones especificadas dentro del iframe, pero la navegación de nivel superior está deshabilitada. Esto inhibe los comportamientos de destrucción de marcos y al mismo tiempo permite la funcionalidad dentro del sitio de destino.

Explicacion de cada uno de estos valores:

ChatGPT

Claro, permíteme explicarte las diferencias entre los valores del atributo `sandbox` de un elemento `<iframe>`:

1. `allow-forms`: Este valor permite que el contenido del iframe pueda enviar formularios (por ejemplo, mediante el uso de etiquetas `<form>` y elementos de entrada como `<input>`). Sin embargo, el contenido no tendrá permiso para ejecutar scripts, lo que significa que no podrá manipular la página principal que contiene el iframe.
    
2. `allow-scripts`: Al utilizar este valor, el contenido del iframe tiene permiso para ejecutar scripts. Esto significa que puede ejecutar código JavaScript en el contexto del iframe. Sin embargo, el contenido no podrá enviar formularios ni realizar otras acciones que puedan afectar la página principal, a menos que se especifiquen explícitamente mediante otros valores de `sandbox`.
    
3. `allow-top-navigation`: Este valor permite que el contenido del iframe pueda navegar a otras páginas web en la misma ventana o pestaña que la página principal que contiene el iframe. Esto generalmente se utiliza para permitir que el contenido del iframe redirija al usuario a otras páginas web dentro del mismo contexto de navegación.

## Combinando clickjacking con un ataque DOM XSS

Hasta ahora, hemos considerado el clickjacking como un ataque autónomo. Históricamente, el clickjacking se ha utilizado para realizar comportamientos como aumentar los "me gusta" en una página de Facebook. Sin embargo, la verdadera potencia del clickjacking se revela cuando se utiliza como portador de otro ataque, como un ataque DOM XSS. La implementación de este ataque combinado es relativamente sencilla suponiendo que el atacante haya identificado primero el exploit XSS. Luego, el exploit XSS se combina con la URL de destino del iframe para que el usuario haga clic en el botón o enlace y, en consecuencia, ejecute el ataque DOM XSS.

## Clickjacking de varios pasos

La manipulación por parte de un atacante de las entradas de un sitio web objetivo puede requerir múltiples acciones. Por ejemplo, un atacante podría querer engañar a un usuario para que compre algo en un sitio web minorista, por lo que es necesario agregar los artículos a una cesta de compras antes de realizar el pedido. El atacante puede implementar estas acciones utilizando múltiples divisiones o iframes. Estos ataques requieren considerable precisión y cuidado desde la perspectiva del atacante para que sean efectivos y sigilosos.

# Cómo prevenir ataques de clickjacking

Hemos analizado un mecanismo de prevención del lado del navegador que se encuentra comúnmente: los scripts de destrucción de marcos. Sin embargo, hemos visto que a menudo resulta sencillo para un atacante eludir estas protecciones. En consecuencia, se han ideado protocolos impulsados ​​por el servidor que limitan el uso del iframe del navegador y mitigan el clickjacking.

El clickjacking es un comportamiento del lado del navegador y su éxito o no depende de la funcionalidad del navegador y de la conformidad con los estándares web y las mejores prácticas vigentes. La protección del lado del servidor contra el clickjacking se proporciona definiendo y comunicando restricciones sobre el uso de componentes como iframes. Sin embargo, la implementación de la protección depende del cumplimiento del navegador y del cumplimiento de estas restricciones. Dos mecanismos para la protección contra el clickjacking del lado del servidor son X-Frame-Options y Content Security Policy.

## Opciones de X-Frame

X-Frame-Options se introdujo originalmente como un encabezado de respuesta no oficial en Internet Explorer 8 y fue adoptado rápidamente en otros navegadores. El encabezado proporciona al propietario del sitio web control sobre el uso de iframes u objetos, por lo que la inclusión de una página web dentro de un marco puede prohibirse con la `deny`directiva:

`X-Frame-Options: deny`

Alternativamente, el framing se puede restringir al mismo origen que el sitio web utilizando la `sameorigin`directiva

`X-Frame-Options: sameorigin`

o a un sitio web designado utilizando la `allow-from`directiva:

`X-Frame-Options: allow-from https://normal-website.com`

X-Frame-Options no se implementa de manera consistente en todos los navegadores (la `allow-from`directiva no es compatible con Chrome versión 76 o Safari 12, por ejemplo). Sin embargo, cuando se aplica correctamente junto con la Política de seguridad de contenido como parte de una estrategia de defensa multicapa, puede proporcionar una protección eficaz contra ataques de clickjacking.

## Política de seguridad de contenido (CSP)

La Política de seguridad de contenido (CSP) es un mecanismo de detección y prevención que proporciona mitigación contra ataques como XSS y clickjacking. CSP generalmente se implementa en el servidor web como un encabezado de retorno del formulario:

`Content-Security-Policy: policy`

donde política es una cadena de directivas de políticas separadas por punto y coma. El CSP proporciona al navegador del cliente información sobre las fuentes permitidas de recursos web que el navegador puede aplicar para la detección e interceptación de comportamientos maliciosos.

## Política de seguridad de contenido (CSP): continuación

La protección contra el clickjacking recomendada es incorporar la `frame-ancestors`directiva en la Política de seguridad de contenido de la aplicación. La `frame-ancestors 'none'`directiva tiene un comportamiento similar a la `deny`directiva X-Frame-Options. La `frame-ancestors 'self'`directiva es, en términos generales, equivalente a la directiva X-Frame-Options `sameorigin`. Los siguientes CSP incluyen en la lista blanca marcos únicamente para el mismo dominio:

`Content-Security-Policy: frame-ancestors 'self';`

Alternativamente, el encuadre se puede restringir a sitios nombrados:

`Content-Security-Policy: frame-ancestors normal-website.com;`

Para ser eficaces contra el clickjacking y XSS, los CSP necesitan un desarrollo, implementación y pruebas cuidadosos y deben usarse como parte de una estrategia de defensa multicapa.

