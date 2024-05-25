
#### **Objetivo:**

![[Pasted image 20240503210331.png]]


#### Pasos:

Paso #1:
Creamos en exploit en este caso es uno en .php y luego le cargare en el servidor.

![[Pasted image 20240503210443.png]]


Paso #2:
Cargamos el exploit y lo subimos.

![[Pasted image 20240503210626.png]]


Paso #3:
Una vez subido apuntamos a la direccion donde se muestra la subida de ese archivo para poder ejecutarlo. En este caso con ver la url de la "imagen cargada del avatar" podemos hacerlo. Y con eso conseguimos la flag.

![[Pasted image 20240503210841.png]]


### Conclusión:

Como pude notar no hay ninguna validacion del tipo de archivo que se sube, no tuve que aplicar ninguna forma de "bypass" para poder subir este archivo .php en el servidor y tampoco había ninguna forma de seguridad a la hora de cargar archivos desde el servidor.