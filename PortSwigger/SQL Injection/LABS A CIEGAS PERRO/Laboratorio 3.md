## Objetivo:

![[Pasted image 20240626203037.png]]

## Solución:

Entramos a cualquier lado de la pagina e interceptamos la peticion y la enviamos al repeater.
Luego intentamos meter un caracter como una comilla simple para ver que sucede...

Y vemos que devuelve un error y aparte tambien nos devuelve la query que se está ejecutando de manera interna

![[Pasted image 20240626203210.png]]

Vemos que en el error nos dice que está esperando un dato de tipo "Char" entonces lo que haremos será dejarle una comilla y meterle un operador AND con una siguiente instruccion

```
' AND CAST((SELECT 'a' FROM users) AS int)-- -
```

Y vemos que ahora nos da el siguiente error que basicamente dice que el argumento del operador AND debe ser de tipo booleano y no de tipo entero....

![[Pasted image 20240626204058.png]]

Entonces modificaremos la query de la siguiente manera y vemos que lanza el siguiente error

```
' AND CAST((SELECT 'a') AS int)=1-- -
```

![[Pasted image 20240626204418.png]]
Como vemos en este error nos muestra que "a" no es válida para el tipo entero... Ya con esto tenemos practicamente hecho el laboratorio, ahora en lugar de mostrar "a" como tal mostrariamos el dato de una columna de alguna tabla..

Para que quede mas claro hare lo siguiente

```
' AND CAST((SELECT 'a' FROM users) AS int)=1 -- -
```

![[Pasted image 20240626204703.png]]

Y ahora vemos que el error cambia y nos dice hay ms de una fila devuelta por una sub consulta utilizada como expresion... Entonces quiere decir que si pudimos ejecutar la query y extraer datos, ya solo tocaría extraer UNA SOLA FILA y lo hacemos de la siguiente manera

```
' AND CAST((SELECT 'a' FROM users LIMIT 1) AS int)=1 -- -
```

Y efectivamente volvemos al error anterior

![[Pasted image 20240626204909.png]]

Ahora en lugar de 'a' mostraremos el nombre de una columna

```
' AND CAST((SELECT username FROM users LIMIT 1) AS int)=1 -- -
```

![[Pasted image 20240626204954.png]]

Y como pudimos ver ya extraimos el username de un usuario, en este caso administrador... Ahora haremos una concadenacion para ahi mismo extraer la contraseña. Claro podemos tambien extraer el "LIMIT 1" de la columna "password" y sabríamos que ésta es la contraseña del administrador...

![[Pasted image 20240626205113.png]]

Pero como eso es aburrido, y para hacer el ataque mas sofisticado podriamos meterle una concadenacion. Lastimoesamente este laboratorio tiene una limitacion del input de entrada y corta la query enviada...

```
' AND CAST((SELECT username||password FROM users LIMIT 1) AS int)=1--
```

![[Pasted image 20240626205506.png]]

Entonces de momeno solo usando el LIMIT extraeremos la contraseña e iniciamos sesión y listo.

![[Pasted image 20240626205639.png]]



