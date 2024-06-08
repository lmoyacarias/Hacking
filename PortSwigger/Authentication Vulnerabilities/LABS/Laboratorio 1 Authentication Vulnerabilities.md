
## Objetivo:

![[Pasted image 20240602134223.png]]

## Solución:

Capturamos la peticion y la enviamos al Intruder
![[Pasted image 20240602134631.png]]

Configuramos la variable en el **username** y configuramos el Attack Type a "Sniper"

![[Pasted image 20240602134914.png]]

Cargamos la lista de posibles usuarios en el Payload

![[Pasted image 20240602135127.png]]

Le metemos un Grep al mensaje de respuesta de "Invalid username"
![[Pasted image 20240602135324.png]]

Y comenzar ataque... Y filtramos por el mensaje al cual le aplicamos el Grep y vemos que ya devolvio una respuesta diferente, por lo tanto quiere decir que ese usuario existe.

![[Pasted image 20240602135649.png]]

Ahora haremos lo mismo, pero esta vez pasando como diccionario las contraseñas y dejando a ese usuario de manera estática y la contraseña con variable del Intruder.
![[Pasted image 20240602135904.png]]

![[Pasted image 20240602135937.png]]

Y le metemos un Grep en el mensaje de "Incorrect password", aunque igual lo mas probable es que esta vez al acertar una contraseña, nos devuelva una redirección... No sería tan necesario el Grep de "Incorrect password"

![[Pasted image 20240602140105.png]]

Comenzar el Ataque

![[Pasted image 20240602140220.png]]


Una vez encontrada la contraseña, terminamos el resto del objetivo inicial del laboratorio.

![[Pasted image 20240602140258.png]]


![[Pasted image 20240602140311.png]]

