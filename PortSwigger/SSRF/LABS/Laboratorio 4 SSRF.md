## Objetivo:

![[Pasted image 20240524202052.png]]


## Solucion:

![[Pasted image 20240524202533.png]]

Interceptamos la peticion para ver que pasa por abajo y la enviamos la Repeater para no perderla

![[Pasted image 20240524202558.png]]

Le damos al boton de Next Product

![[Pasted image 20240524204247.png]]

Capturamos la peticion

![[Pasted image 20240524204328.png]]


![[Pasted image 20240524205131.png]]


![[Pasted image 20240524205106.png]]

Se redirigio acá, con esto YA COMPROBAMOS que podemos manipular la redireccion.

![[Pasted image 20240524210937.png]]


Y como vimos en el Check Stock existe un parametro llamado **stockApi**, PERO éste esta validado con un **FILTROS DE ENTRADA BASADOS EN LISTA BLANCA**, entonces para intentar ByPassearlo, como vemos en esta web existe una **Redireccion Abierta** lo que conlleva a que podemos intentar saltar ese filtro. Entonces harémos lo siguiente..

En el checkStock pondremos la ruta que hace que el servidor nos responda con una redireccion, entonces ahi inyectaremos nuestor SSRF, pero para esto interceptaremos la peticion de **checkStock**

![[Pasted image 20240524211604.png]]



![[Pasted image 20240524211721.png]]

Y nos saldra esto y lo eliminamos manualmente

![[Pasted image 20240524211807.png]]

Ya si queremos hacerlo desde burp entonces desde el repeater lo hacemos asi

![[Pasted image 20240524212011.png]]


![[Pasted image 20240524212033.png]]


