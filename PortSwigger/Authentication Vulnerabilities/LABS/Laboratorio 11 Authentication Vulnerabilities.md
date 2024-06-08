
## Objetivo:

![[Pasted image 20240605232818.png]]

## Solución:

Iniciamos sesion con nuestro usuario

![[Pasted image 20240605233139.png]]

Reseteamos nuestra contraseña para ver el flujo

![[Pasted image 20240605233323.png]]

Agregamos "X-Forwarded-Host" con la ruta de nuestro host atacante y cambiamos el "username" a carlos..
Para que agregamos "X-Forwarded-Host"? pues es una cabecera HTTP que se utiliza comúnmente en aplicaciones web para indicar el nombre de host original de la solicitud, especialmente cuando la solicitud ha pasado a través de uno o más proxies o balanceadores de carga.

Por ejemplo, si un cliente hace una solicitud a un servidor web a través de un proxy o un balanceador de carga, la dirección IP y el nombre de host del proxy o del balanceador de carga pueden reemplazar la dirección IP y el nombre de host originales del cliente. La cabecera "X-Forwarded-Host" permite al servidor web saber cuál era el nombre de host original de la solicitud antes de pasar por esos intermediarios.

Esto es útil para aplicaciones que necesitan saber el nombre de host original para generar enlaces absolutos o para tomar decisiones basadas en el nombre de host de la solicitud. Por ejemplo, una aplicación podría utilizar esta información para generar enlaces que apunten al nombre de host correcto o para aplicar lógica de enrutamiento específica basada en el nombre de host de la solicitud.


![[Pasted image 20240607212553.png]]

Buscamos en los logs del servidor una ruta similar a la que nos llegaría a nuestro usuario que ya conocemos como la siguiente 

![[Pasted image 20240607212858.png]]


Vemos que aqui está.

![[Pasted image 20240607212821.png]]

Y nos copiamos esa ruta en el navegador, pero agregando el Host original y NO nuestro host atacante... Esto porque?

Porque como vemos cuando el usuario dio click en la url que llego para cambiar su contraseña, a él le cargo esto:

![[Pasted image 20240607213127.png]]

Y por lo tanto ese "token" aun sigue vigente, ya que nunca se expiró al ser usado por dicho usuario, ya que apuntó a un host el cual no tiene esos "endpoints" con esas acciones disponibles. Y en caso de que pongamos el Host original, al si tener dichas acciones y funcionalidades entonces si se expiraría... Y como solo nosotros entraremos ahí usando le host original, por lo tanto el token solo lo usariamos bien nosotros.

Entonces entrariamos con ese token y cambiamos la contraseña de dicho usuario

![[Pasted image 20240607213417.png]]


Ingresamos las credenciales

![[Pasted image 20240607213447.png]]


![[Pasted image 20240607213457.png]]

![[Pasted image 20240607213509.png]]
