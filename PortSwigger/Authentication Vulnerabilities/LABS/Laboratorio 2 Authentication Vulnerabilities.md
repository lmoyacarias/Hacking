
## Objetivo:

![[Pasted image 20240602140614.png]]


## Solucion:

Enviamos la peticion al Intruder

![[Pasted image 20240602141430.png]]

Metemos la variable en el "username" y configuramos el Attack Type a "Sniper"

![[Pasted image 20240602141536.png]]

Y metemos la lista de posibles usuarios como payload

![[Pasted image 20240602141626.png]]

Le metemos un Grep en la respuesta de "***Invalid username or password.***"

![[Pasted image 20240602141733.png]]


Y Comenzar Ataque.

Vemos que hay una pequeña diferencia entre las respuestas, ésta viene sin el ***punto*** y las demas si lo traen... Y hay que comprobarlo.

![[Pasted image 20240602142051.png]]

![[Pasted image 20240602142213.png]]

![[Pasted image 20240602142258.png]]


Una vez que ya tenemos un usuario encontrado, ahora haremos lo mismo pero utilizando como variable la "password" y el diccionario de las posibles contraseñas.


![[Pasted image 20240602142431.png]]

Le metemos un Grep, aunque igual es fijo que devolverá una redirección (status code 302)

![[Pasted image 20240602142456.png]]

Y Comenzar Ataque
![[Pasted image 20240602142637.png]]


![[Pasted image 20240602142710.png]]


![[Pasted image 20240602142722.png]]

