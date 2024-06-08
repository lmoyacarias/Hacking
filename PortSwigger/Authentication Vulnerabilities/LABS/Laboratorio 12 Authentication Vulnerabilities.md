
## Objetivo:

![[Pasted image 20240607214208.png]]

## Solucion:

Ingresamos con nuestro usuario

![[Pasted image 20240607214809.png]]

Hacemos un cambio de contraseña y enviamos la peticion al Repeater

![[Pasted image 20240607214856.png]]

Primero veremos el flujo de como funcion el cambio de contraseña. Vemos que cuando se produce un cambio de contraseña la respuesta es la siguiente

![[Pasted image 20240607215519.png]]

Vemos que al intentar poner la "contraseña actual correcta" y las contraseñas "new-password" y "new-password-2" diferentes entre ellas nos devuelve un error que dice "New passwords do not match"

![[Pasted image 20240607215754.png]]

Ahora si intentamos poner la "contraseña actual incorrecta" y las demas diferentes, ahora tenemos un error que dice "Current password is incorrect".

![[Pasted image 20240607215839.png]]

Ahora bien, si ponemos la "contraseña actual incorrecta" y las demas que coicidan entre ambas...

Vemos que nos saca de la app por seguridad, y nos cierra la sesión

![[Pasted image 20240607220019.png]]

Ahora pondremos la "contraseña actual correcta" y las contraseñas a cambiar las pondremos igual que la contraseña actual y ambas que coicidan a ver si da error de "contraseña no puede ser igual a la existente" o algo similar... Y vemos que NO, lo unico que sucedió fue que se cambio la contraseña actual a a la misma que ya tenia

![[Pasted image 20240607220422.png]]


Ahora podriamos meter la "contraseña actual incorrecta" y en las demas la contraseña existente a ver que error lanza, pero no tiene sentido. Ya sabemos que el match para explotar este fallo de fuerza bruta seria ingresar "contraseña actual incorrecta o correcta" y "contraseñas nuevas que no coincidan", todo se basa en ***"Ingresar contraseñas nuevas que NO coincidan"*** SOLAMENTE EN ESTE CASO, en alguna web que tengas que evaluar probá todos los casos posibles, incluso el último que yo no probé porque ya vimos que encontramos un match para explotar esto...


Este seria el match

![[Pasted image 20240607220808.png]]

Ahora cambiamos el "username" a carlos

![[Pasted image 20240607220843.png]]

Ahora enviamos esta peticion al Intruder y seteamos como variable al "current-password"

![[Pasted image 20240607220932.png]]

Agregamos nuestros payloads de contraseña

![[Pasted image 20240607221008.png]]

Le metemos un Grep en "Current password is incorrect"

![[Pasted image 20240607221120.png]]

Y lanzamos el ataque y encontramos en el Grep el mensaje "New passwords do not match", que recordemos esto pasa cuando ingresamos con la "contraseña actual correcta" y las "nuevas contraseñas no coinciden"...

![[Pasted image 20240607221605.png]]

Una vez aqui dentro podemos hacer 2 cosas, cambiar la contraseña del usuario o simplemente iniciar sesión con dicho usuario... Si fuera un caso de la vida real y queres "robar" directamente la cuenta del usuario lo mas rápido posible, lo mejor sería hacer el cambio de contraseña directamente... En mi caso solo iniciaré sesión

![[Pasted image 20240607221844.png]]

