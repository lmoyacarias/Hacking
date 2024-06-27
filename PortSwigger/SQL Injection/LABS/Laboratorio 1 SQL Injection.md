## Objetivo:

![[Pasted image 20240622141949.png]]

## Solución:

Entramos a una categoría

![[Pasted image 20240622142051.png]]

Metemos una comilla para probar si es vulnerable y vemos que devuelve un error en la query de " category=' "

![[Pasted image 20240622142151.png]]

Ahora probamos meter un **'--** y vemos que no devuelve "Internal Server Error" por lo tanto la query fue completamente valida

![[Pasted image 20240622142323.png]]

Ahora para recuperar todos los datos usaremos **' OR 1=1--**

![[Pasted image 20240622142419.png]]
