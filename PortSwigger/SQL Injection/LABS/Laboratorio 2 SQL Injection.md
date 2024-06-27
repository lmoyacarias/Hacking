## Objetivo:

![[Pasted image 20240622142723.png]]

## Solución:

Ingresamos a la pantalla del Login

![[Pasted image 20240622142826.png]]

Ahora ingresaremos el nombre de usuario **administrator** y suponiendo que la query esta por dentras hecha de esta manera **SELECT user FROM users WHERE username = 'administrator' AND password = 'miContrasena'** entonces intentaremos meter en el input de contraseña lo siguiente **' OR 1=1--** y al meter esto la query total quedaría de la siguiente forma **SELECT user FROM users WHERE username = 'administrator' AND password = '' OR 1=1--'** y asi podriamos bypassear el login... Igual podriamos hacer la injection de otra manera


![[Pasted image 20240622143326.png]]

![[Pasted image 20240622143336.png]]

Otra manera de hacerlo...

![[Pasted image 20240622143443.png]]
