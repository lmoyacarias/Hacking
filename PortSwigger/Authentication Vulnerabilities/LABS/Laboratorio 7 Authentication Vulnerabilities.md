
## Objetivo:

![[Pasted image 20240603224657.png]]

## Solucion:

Iniciamos sesión con nuestra cuenta para ver como fluye la peticion
![[Pasted image 20240603224822.png]]

Vemos que nos llega una nueva cookie, que al parecer es una cookie temporal la cual) el servidor la solicita en el Paso #2  (Login 2) para llevar al formulario de ingresar OTP

![[Pasted image 20240603230540.png]]


Luego enviamos la request de "/login2" al Repeater...Y enviamos la peticion de Login  le cambiamos el verify a "carlos"... Al hacer eso lo que pasara es que al usuario "carlos" le llegara un OTP a su correo

![[Pasted image 20240603230651.png]]

![[Pasted image 20240603230824.png]]

Ahora aplicaremos fuerza bruta a ese formulario para intentar adivinar el OTP de "carlos" ya que no tenemos acceso al correo de él...

Damos click derecho en la pantalla del response, luego en "Request in Browser" y luego en "In original session" y copiamos esa url y la pegamos en el navegador, y hacemos una peticion en ese formulario y la enviamos al Intruder y aplicamos fuerza bruta...

Cambiamos el verify a "carlos" y metemos el "mfa-code" a variable
![[Pasted image 20240603231245.png]]

Configuramos el Payload de esta manera

![[Pasted image 20240603231426.png]]


Le metemos un Grep en "Incorrect security code" pero igual podemos guiarnos por el "Status code" que sera una redireccion (status code 302)

![[Pasted image 20240603231538.png]]

Y comenzar ataque
![[Pasted image 20240603235057.png]]

Damos click derecho en la peticion, luego "Request in browser" y por ultimo en "In original session" y la pegamos en el navegador


![[Pasted image 20240603235212.png]]

![[Pasted image 20240603235226.png]]


