
## Objetivo:

![[Pasted image 20240605222014.png]]

## Solución:

Iniciamos sesion en nuestro usuario

![[Pasted image 20240605222341.png]]

Agregamos un comentario y enviamos la peticion al Repeater

![[Pasted image 20240605223002.png]]


Comprobamos a ver si alguno de los campos es vulnerable a XSS y vemos que si

![[Pasted image 20240605223153.png]]

![[Pasted image 20240605223224.png]]

Ya con esto podemos intentar robar la cookie de los usuarios que lleguen a ver dicho comentario podemos hacerlo de varias maneras... En este caso lo hare con fetch y hare una peticion a mi servidor Atacante robando la cookie de dicho usuario

![[Pasted image 20240605223703.png]]

Y vemos que ya cayó un usuario en la trampa

![[Pasted image 20240605223736.png]]

Ahora solamente usaremos dicha cookie para intentar iniciar sesion.... Entramos a "My account" con nuestro usuario y luego enviamos eso al Repeater y cambiamos el valor de la cookie "stay-logged-in" a la que robamos. Cerramos sesion con nuestor usuario "wiener" y luego enviamos la peticion con la cookie de "carlos"

![[Pasted image 20240605224115.png]]

![[Pasted image 20240605224244.png]]

Luego click derecho, "Request In Browser" despues "In original session" copiamos y pegamos y eliminamos el usuario

![[Pasted image 20240605224354.png]]

Luego vemos que nos pide la password... Como NO la sabemos, entonces usaremos la cookie (previamente estudiada con la nuestra) para intentar ver como esta formada y luego aplicar fuerza bruta y romperla...

Y como vemos sigue el mismo patron "user:contraseñaEnMd5" y todo eso en Base64

![[Pasted image 20240605224545.png]]

![[Pasted image 20240605224616.png]]

Ya una vez crackeada la contraseña, la usamos y eliminamos la cuenta

![[Pasted image 20240605224724.png]]

![[Pasted image 20240605224734.png]]

